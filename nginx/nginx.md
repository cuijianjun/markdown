#### nginx

- nginx 重启

  ````nginx
  systemctl restart nginx.service 
  systemctl reload nginx.service 
  ````

- 错误日志

  ````nginx
  tail -f /var/log/nginx/error.log
  ````

- 请求日志

  ````nginx
  tail -n 200 /var/log/nginx/access.log
  ````

- 语法检查

  ```nginx
  nginx -t -c /etc/nginx/nginx.conf   -t语法检查  -c指定路径
  ```

- 重新加载配置

  ````nginx
  nginx -s reload  -c /etc/nginx/nginx.conf
  ````

#### 生成ca证书

1. openssl version 查看是否安装
2. nginx -V  确认一下对应的http_ssl证书的模块的存在
3. cd /etc/nginx
4. mkdir ssl_key
5. cd ssl_key
6. openssl genrsa -idea -out jesonc.key 1024 生成密码文件
7. openssl req -new -key jesonc.key -out jesonc.csr 
8. openssl x509 -req -days 3650 -in jesonc.csr -signkey jesonc.key -out jesonc.crt 打包文件