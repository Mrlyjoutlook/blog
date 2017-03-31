# Nginx

- 下面案例中服务器nginx目录路径假设为`/home/ubuntu/nginx/`
- 服务器查询当前目录路径命令 pwd

# 多配置文件

在`/home/ubuntu/nginx/include/`下存在`**.a.gopeak.cn.conf` `**.a.gopeak.cn.conf`。  

修改`/home/ubuntu/nginx/nginx.conf`文件中的http

```
...

http:{
    include /home/ubuntu/nginx/include/*.conf
}

...
```

如果`.conf`文件单独存放在每个项目中，可以使用如下命令。（`path/to`为项目的路径）

```
sudo ln -s path/to/nginx.conf /home/ubuntu/nginx/conf/include/**.gopeak.cn.conf
```

# 全局变量

- $args ： 这个变量等于请求行中的参数，同`$query_string`
- $content_length ： 请求头中的`Content-length`字段
- $content_type ： 请求头中的`Content-Type`字段
- $document_root ： 当前请求在`root`指令中指定的值
- $host ： 请求主机头字段，否则为服务器名称
- $http_user_agent ： 客户端`agent`信息
- $http_cookie ： 客户端`cookie`信息
- $limit_rate ： 这个变量可以限制连接速率
- $request_method ： 客户端请求的动作，通常为`GET`或`POST`
- $remote_addr ： 客户端的IP地址
- $remote_port ： 客户端的端口
- $remote_user ： 已经经过`Auth Basic Module`验证的用户名
- $request_filename ： 当前请求的文件路径，由`root`或`alias`指令与URI请求生成
- $scheme ： HTTP方法（如http，https）
- $server_protocol ： 请求使用的协议，通常是`HTTP/1.0`或`HTTP/1.1`
- $server_addr ： 服务器地址，在完成一次系统调用后可以确定这个值
- $server_name ： 服务器名称
- $server_port ： 请求到达服务器的端口号
- $request_uri ： 包含请求参数的原始URI，不包含主机名，如`/foo/bar.php?arg=baz`
- $uri ： 不带请求参数的当前URI，$uri不包含主机名，如`/foo/bar.html`
- $document_uri ： 与`$uri`相同

# listen

一般监听端口 80

# server_name

实现多个域名对多个应该端口的匹配，支持前，后缀通配符，正则表达式。

```
server{
    listen 80;
    server_name **.gopeak.cn（www.gopeak.** or ~[a-z]+\.gopeak\.cn）
}
```

# location

地址查找规则

```
server{
    ...
    location = / {}
    ...
}
```

# Try_files规则

try_files $uri $uri/ /index.php  
假设请求为http://www.gopeak.cn/test，则$uri为test

查找/$root/test文件  
查找/$root/test/目录  
发起/index.php的内部“子请求”

# Rewrite规则

rewrite ^/images/(.*).(png|jpg|gif)$ /images?name=$1.$4 last;

上面的rewrite规则会将文件名改写到参数中  

last : 相当于Apache的[L]标记，表示完成rewrite  
break : 停止执行当前虚拟主机的后续rewrite指令集  
redirect : 返回302临时重定向，地址栏会显示跳转后的地址  
permanent : 返回301永久重定向，地址栏会显示跳转后的地址  