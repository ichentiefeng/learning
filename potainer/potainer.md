官网：https://www.portainer.io/

https://www.cnblogs.com/JulianHuang/p/12517074.html



### 安装Portaniner.io

为Porttainer.io 创建Volume

```
sudo docker volume create portainer_data
```

启动portainer容器,配置在宿主机9000端口映射

```shell
docker run -d --name portainer -p 59000:9000 -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer
```

http://192.168.56.150:59000

admin/12345678

