https://github.com/FiloSottile/mkcert

https://mkcert.dev/

---

```
$ brew install mkcert

$ brew install nss # 为了支持 Firefox

$ mkcert -install
Created a new local CA at "/Users/fuxiaosong/Library/Application Support/mkcert" 💥
The local CA is now installed in the system trust store! ⚡️
The local CA is now installed in the Firefox trust store (requires browser restart)! 🦊

$ mkcert localhost 127.0.0.1 ::1
Using the local CA at "/Users/fuxiaosong/Library/Application Support/mkcert" ✨

Created a new certificate valid for the following names 📜
 - "localhost"
 - "127.0.0.1"
 - "::1"

The certificate is at "./localhost+2.pem" and the key at "./localhost+2-key.pem" ✅
```


---

```
$ mkcert example.com "*.example.com" example.test localhost 127.0.0.1 ::1
Using the local CA at "/Users/filippo/Library/Application Support/mkcert" ✨

Created a new certificate valid for the following names 📜
 - "example.com"
 - "*.example.com"
 - "example.test"
 - "localhost"
 - "127.0.0.1"
 - "::1"

The certificate is at "./example.com+5.pem" and the key at "./example.com+5-key.pem"
```