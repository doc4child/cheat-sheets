## phpMyAdmin Docker Image with SSL/TLS

The official Docker phpMyAdmin Image does not come with any documented configuration option for SSL/TLS certificates. However the image runs an Apache2 webserver with pretty much default configuration.

In case you need SSL/TLS and you do not want to run phpMyAdmin behind a proxy you can build your own Docker Image by creating a Dockerfile like this

```
FROM phpmyadmin/phpmyadmin

RUN a2enmod ssl

RUN sed -ri -e 's,80,443,' /etc/apache2/sites-available/000-default.conf
RUN sed -i -e '/^<\/VirtualHost>/i SSLEngine on' /etc/apache2/sites-available/000-default.conf
RUN sed -i -e '/^<\/VirtualHost>/i SSLCertificateFile /cert/cert.pem' /etc/apache2/sites-available/000-default.conf
RUN sed -i -e '/^<\/VirtualHost>/i SSLCertificateKeyFile /cert/privkey.pem' /etc/apache2/sites-available/000-default.conf
RUN sed -i -e '/^<\/VirtualHost>/i SSLCertificateChainFile /cert/fullchain.pem' /etc/apache2/sites-available/000-default.conf

EXPOSE 443
```
The 000-default.conf will be adjusted with few changes:

* Line 3: Enable Apache2 SSL module
* Line 5: Switch to default SSL Port 443 instead of 80
* Line 6: Enable SSL for the vhost
* Line 7-9: Set paths for the certificate files

The image expects that you mount your certificates folder to the /certs folder. In this case I use Let's Encrypt certificates.

Build the image:
```
docker build --file ./Dockerfile -t my_pma_ssl_image .
```
Run the image:
```
docker run -d -p 8080:443 -v /path/to/certs/on/host:/certs:ro my_pma_ssl_image
```
Now you can reach PMA with: https://yourdomain.tld:8080/


Ref: https://blog.zotorn.de/phpmyadmin-docker-image-with-ssl-tls/
