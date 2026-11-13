/////////////////////
///now install FTP///
/////////////////////


sudo apt update
sudo apt install php-fpm -y

mkdir -p /var/www/FTP
cd /var/www/FTP

chown -R www-data:www-data /var/www/FTP
chmod -R 755 /var/www/FTP


wget https://github.com/mahfuzurrahmanrubel/h5ai/archive/refs/tags/h5ai.zip

apt install unzip
unzip h5ai.zip
rm h5ai.zip

nano /etc/nginx/sites-available/h5ai-8888.conf

```
server {
    listen 8888;
    server_name _;

    # আপনার ফাইল রাখার ফোল্ডার (FTP ফোল্ডারটি ব্যবহার করছি)
    root /var/www/FTP/;

    # h5ai কে ডিফল্ট ইনডেক্স হিসেবে সেট করা
    index index.html index.php /_h5ai/public/index.php;

    location / {
        # ফোল্ডার ব্রাউজিং এনাবল করা এবং h5ai লোড করা
        try_files $uri $uri/ /_h5ai/public/index.php;
    }

    # PHP 8.3 কনফিগারেশন (আপনার সকেট পাথ অনুযায়ী)
    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php8.3-fpm.sock;
    }
}
