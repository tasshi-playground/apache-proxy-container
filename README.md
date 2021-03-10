# apache-proxy-container

Proxy HTTP request contains `absolute-form` Request-URI.

https://tools.ietf.org/html/rfc7230#section-5.3.2

> When making a request to a proxy, other than a CONNECT or server-wide
> OPTIONS request (as detailed below), a client MUST send the target
> URI in absolute-form as the request-target.

**THIS IS NOT FOR PRODUCTION.**

## Usage

Start proxy container on http://localhost:3128

```sh
$ docker-compose up -d
```

Sample request with [axios](https://github.com/axios/axios)

```javascript
// fetch today's weather overview.
const axios = require("axios");

axios
  .get("/bosai/forecast/data/overview_forecast/130000.json", {
    baseURL: "https://www.jma.go.jp",
    proxy: {
      host: "localhost",
      port: 3128,
    },
  })
  .then((resp) => console.log(resp.data))
  .catch((e) => console.log(e));
```

## Diff of httpd.conf

```diff
--- my-httpd.conf  2021-03-10 16:52:13.000000000 +0900
+++ my-httpd.conf  2021-03-10 16:52:06.000000000 +0900
@@ -142 +142 @@
-#LoadModule proxy_module modules/mod_proxy.so
+LoadModule proxy_module modules/mod_proxy.so
@@ -145 +145 @@
-#LoadModule proxy_http_module modules/mod_proxy_http.so
+LoadModule proxy_http_module modules/mod_proxy_http.so
@@ -159 +159 @@
-#LoadModule slotmem_shm_module modules/mod_slotmem_shm.so
+LoadModule slotmem_shm_module modules/mod_slotmem_shm.so
@@ -161 +161 @@
-#LoadModule ssl_module modules/mod_ssl.so
+LoadModule ssl_module modules/mod_ssl.so
@@ -551,0 +552,5 @@
+<IfModule mod_proxy.c>
+ProxyRequests On
+ProxyVia On
+SSLProxyEngine on
+</IfModule>
```

## references

- https://tools.ietf.org/html/rfc7230#section-5.3.2
- https://developer.mozilla.org/ja/docs/Web/HTTP/Messages#http_requests
- https://stackoverflow.com/questions/23931987/apache-proxy-no-protocol-handler-was-valid
