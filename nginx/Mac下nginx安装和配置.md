## Mac下nginx安装和配置

brew search nginx

brew install nginx



nginx -s quit 退出

nginx -s reload 重新加载

nginx -t 测试nginx.conf配置



/usr/local/etc/nginx/nginx.conf （配置文件路径）

/usr/local/var/www （服务器默认路径）

/usr/local/Cellar/nginx/1.8.0 （安装路径）

访问localhost:8080，成功说明安装好了



![image-20180722090840505](/var/folders/jp/36l2pbb13xx5jqwst3cdk_sr0000gn/T/abnerworks.Typora/image-20180722090840505.png)



export PATH="/usr/local/opt/openssl/bin:$PATH"'