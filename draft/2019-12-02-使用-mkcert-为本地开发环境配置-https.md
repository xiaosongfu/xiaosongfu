https://github.com/FiloSottile/mkcert

https://mkcert.dev/

---

```
$ brew install mkcert

$ brew install nss # δΈΊδΊζ―ζ Firefox

$ mkcert -install
Created a new local CA at "/Users/fuxiaosong/Library/Application Support/mkcert" π₯
The local CA is now installed in the system trust store! β‘οΈ
The local CA is now installed in the Firefox trust store (requires browser restart)! π¦

$ mkcert localhost 127.0.0.1 ::1
Using the local CA at "/Users/fuxiaosong/Library/Application Support/mkcert" β¨

Created a new certificate valid for the following names π
 - "localhost"
 - "127.0.0.1"
 - "::1"

The certificate is at "./localhost+2.pem" and the key at "./localhost+2-key.pem" β
```


---

```
$ mkcert example.com "*.example.com" example.test localhost 127.0.0.1 ::1
Using the local CA at "/Users/filippo/Library/Application Support/mkcert" β¨

Created a new certificate valid for the following names π
 - "example.com"
 - "*.example.com"
 - "example.test"
 - "localhost"
 - "127.0.0.1"
 - "::1"

The certificate is at "./example.com+5.pem" and the key at "./example.com+5-key.pem"
```