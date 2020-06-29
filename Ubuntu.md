# Ubuntu

## apt源

- aliyun

  ```bash
  sed -i 's|https\?://[^/]\+/|http://mirrors.aliyun.com/|' /etc/apt/sources.list && apt-get update
  ```
