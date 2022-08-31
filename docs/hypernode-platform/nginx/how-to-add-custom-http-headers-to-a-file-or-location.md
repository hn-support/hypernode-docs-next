<!-- source: https://support.hypernode.com/en/hypernode/nginx/how-to-add-custom-http-headers-to-a-file-or-location/ -->
# How to Add Custom HTTP Headers to a File or Location

In some cases you may wish to add additional HTTP headers to a file or location. Most commonly these headers will be used to provide additional information for troubleshooting purposes, for [Cross-Origin Resource Sharing (CORS)](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) or to set certain caching directives.


Add HTTP Header to a Single File
--------------------------------

Adding a HTTP header to one single file is relatively easy. Simply create a `server.headers` file in `/data/web/nginx/` containing the following snippet:

```nginx
location /info.html {
  add_header "Some Header Name" "Some value";
}

```
Add HTTP Header to Multiple Files/Location
------------------------------------------

Most often you wish to add an additional HTTP header to multiple files, for instance to all the images that are loaded when someone is browsing your website. This can be done using a Regular Expression (regex) within the `server.headers` file. For example:

```nginx
location ~* (.+\.(jpg|jpeg|gif|css|png|js|ico|html|xml|txt))$ {
  add_header Pragma public;
  add_header Cache-Control "public";
}

```
It is even a possibility to add HTTP headers to **all** files by not defining any location or file. For example:

```nginx
add_header Pragma public;

```
If you want to override, for example the /media/ location but also other used locations in `/etc/nginx/magentoX.conf`, you must use an other regex then the already defined location block in our Magento Nginx configs:

Use the following in the `/data/web/nginx/server.headers` configuration file:

```nginx
location ~* ^/media/.*\.(css|js|png|gif|txt)$ {
add_header Access-Control-Allow-Origin "https://www.example.com";
}
```
And don’t forget to change the domain name in the above example. You could add more file extensions if you want.

Keep in Mind
============

* It is not possible in Nginx to add locations twice. Only the first location in the configuration file takes presence. This is because Nginx config works on a per request base.
* Don’t use a colon (`:`) when adding headers. This will result in inconsistent results in different browsers.