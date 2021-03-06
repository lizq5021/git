# http转https 使用nginx配置文件

~~~shell
server
{
    listen 80;
    listen 443 ssl; #SSL 访问端口号为 443
    server_name blog.suhuanzhen.cn;
    index index.php index.html index.htm default.php default.htm default.html;
    root /www/wwwroot/blog.suhuanzhen.cn;
    
    #SSL-START SSL相关配置，请勿删除或修改下一行带注释的404规则
    #error_page 404/404.html;
    #SSl-START
    #如果不是443端口就重定向到安全链接
    if ($server_port !~ 443){
        rewrite ^(/.*)$ https://$host$1 permanent;
    }
    ssl_certificate /www/ssl/blog/1_blog.suhuanzhen.cn_bundle.crt;	#证书文件
    ssl_certificate_key /www/ssl/blog/2_blog.suhuanzhen.cn.key;		#私钥文件
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;							#使用的协议
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE;	#配置加密套件，写法遵循 openssl 标准
    ssl_prefer_server_ciphers on;
    ssl_session_cache shared:SSL:10m;
    ssl_session_timeout 10m;
    error_page 497  https://$host$request_uri;
    
    #SSL-END
    
    #ERROR-PAGE-START  错误页配置，可以注释、删除或修改
    error_page 404 /404.html;
    error_page 502 /502.html;
    #ERROR-PAGE-END
    
    #PHP-INFO-START  PHP引用配置，可以注释或修改
    include enable-php-70.conf;
    #PHP-INFO-END
    
    #REWRITE-START URL重写规则引用,修改后将导致面板设置的伪静态规则失效
    include /www/server/panel/vhost/rewrite/blog.suhuanzhen.cn.conf;
    #REWRITE-END
    
    #禁止访问的文件或目录
    location ~ ^/(\.user.ini|\.htaccess|\.git|\.svn|\.project|LICENSE|README.md)
    {
        return 404;
    }
    
    #一键申请SSL证书验证目录相关设置
    location ~ \.well-known{
        allow all;
    }
    
    location ~ .*\.(gif|jpg|jpeg|png|bmp|swf)$
    {
        expires      30d;
        error_log off;
        access_log off;
    }
    
    location ~ .*\.(js|css)?$
    {
        expires      12h;
        error_log off;
        access_log off; 
    }
    access_log  /www/wwwlogs/blog.suhuanzhen.cn.log;
    error_log  /www/wwwlogs/blog.suhuanzhen.cn.error.log;

}

~~~

