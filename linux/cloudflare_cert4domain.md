# cloudflare

custom certificate

* make sure to remove spaces form .pem and .key file stored.
* do not have dot in name like domainname.com.key. Save as domainname.key or domainname.pem only.

In NPM on SSL certificates tab > Add SSL certificate

* choose key and pem files no need for intermediate file.
* if you click 3 dots, it should give certificate no 9 etc.
* the directory is located on the host under /path_to_volumes/npm/npm_data/custom_ssl/npm-9/fullchain.pem and privkey.pem files
* attach this new custum cert for proxyhosts on NPM
* check cert using lock icon in the browser (may have to refresh)

On cloudflare 

* go to SSL/TLS > Overview > SSL/TLS encryption mode
* Change this to Full (strict)


Ref: https://www.youtube.com/watch?v=pwK1LnbTitI
