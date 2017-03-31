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
