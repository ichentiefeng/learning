# Docker

参考：《Docker技术入门与实践第2版》

Docker的生命周期：

- 封装（Packaging）
- 分发（Distribution）
- 部署（Deployment）
- 运行（Runtime）



Docker的三大核心概念：

- 镜像（Image）
- 容器（Container ）
- 仓库（Repository）

Docker镜像：镜像是一个只读的模板，镜像是创建Docker容器。通过版本管理和增量的文件系统，Docker提供了一套十分简单的机制来创建和更新现有的镜像，用户甚至可以从网上下载一个已经做好的应用镜像，并直接使用。

Docker容器：通过版本管理和增量的文件系统，Docker提供了一套十分简单的机制来创建和更新现有的镜像，用户甚至可以从网上下载一个已经做好的应用镜像，并直接使用。

镜像自身是只读的。容器从镜像启动的时候，会在镜像的最上层创建一个可写层。

Docker仓库

Docker安装

卸载旧版本



添加yum软件源

阿里云

```shell
sudo yum-config-manager \
    --add-repo \
    http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

清华大学源

```shell
sudo yum-config-manager \
    --add-repo \
    https://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/centos/docker-ce.repo
```

更新yum软件源缓存

```
sudo yum update
```

安装 Docker Engine-Community

```
sudo yum install docker-ce docker-ce-cli containerd.io
```

 Start Docker. 

```
sudo systemctl start docker
```

  running the `hello-world` image. 

```
sudo docker run hello-world
```

启动一个Nginx容器

```
docker run -d -p 80:80 --name webserver nginx
```

然后使用docker ps指令查看当前运行的容器

```
 docker ps
```

可见Nginx容器已经在0.0.0.0：80启动，并映射了80端口，下面我们打开浏览器访问此地址



配置Docker服务

为了避免每次使用docker命令都要用特权身份，可以将当前用户加入安装中自动创建的docker用户组：

```
sudo usermod -aG docker USER_NAME
```

用户更新组信息后，退出并重新登录后即可生效。

通过service命令来重启Docker服务：

```
sudo systemctl restart docker.service
```

通过systemctl命令来管理Docker服务

```
sudo systemctl start docker.service
```

查看Docker版本信息

```
docker version
```



使用Docker镜像

Docker运行容器前需要本地存在对应的镜像，如果镜像没保存在本
地，Docker会尝试先从默认镜像仓库下载（默认使用Docker Hub公共注册服务器中的仓库），用户也可以通过配置，使用自定义的镜像仓库。

使用pull命令从Docker Hub仓库中下载镜像到本地

可以使用docker pull命令直接从Docker Hub镜像源来下载镜像。

命令格式：

```
docker pull NAME[：TAG]
```

- NAME是镜像仓库的名称
- TAG是镜像的标签（往往用来表示版本信息）

如果不显式指定TAG，则默认会选择 latest标签，会下载仓库中最新版本的镜像。

示例：

```
docker pull centos:centos7
```

查看本地已有的镜像信息

```
docker images
```

运行容器，并且可以通过 exec 命令进入 CentOS 容器。

```
docker run -itd --name centos-test centos:centos7
```

 通过 **docker ps** 命令查看容器的运行信息：



镜像文件一般由若干层（layer）组成，6c953ac5d795这样的串是层的唯一id（实际上完整的id包括256比特，由64个十六进制字符组成）。使用docker pull命令下载时会获取并输出镜像的各层信息。当不同的镜像包括相同的层时，本地仅存储层的一份内容，减小了需要的存储空间。



严格地讲，镜像的仓库名称中还应该添加仓库地址（即registry，册
服务器）作为前缀，只是我们默认使用的是Docker Hub服务，该前缀可以忽略。

如果从非官方的仓库下载，则需要在仓库名称前指定完整的仓库地址。

```
docker pull hub.c.163.com/public/ubuntu:14.04
```

pull子命令支持的选项主要包括：

- -a ，- - all-tags=true|false：是否获取仓库中的所有镜像，默认为否。

下载镜像到本地后，即可随时使用该镜像了，例如利用该镜像创建一个容器，在其中运行bash应用，执行ping localhost命令

```
docker run -it ubuntu:14.04 bash
root@9c74026df12a:/# ping localhost
```



查看镜像信息

使用docker images命令可以列出本地主机上已有镜像的基本信息。

```
docker images
```

在列出的信息中，可以看到以下几个字段信息。
来自于哪个仓库，比如ubuntu仓库用来保存ubuntu系列的基础镜像；
·镜像的标签信息，比如14.04、latest用来标注不同的版本信息。标签
只是标记，并不能标识镜像内容；
·镜像的ID（唯一标识镜像），如ubuntu：latest和u b u n t u ：16.04镜像的ID
都是2fa927b5cdd3，说明它们目前实际上指向同一个镜像；
·创建时间，说明镜像最后的更新时间；
·镜像大小，优秀的镜像往往体积都较小。
其中镜像的ID信息十分重要，它唯一标识了镜像。在使用镜像ID的时
候，一般可以使用该ID的前若干个字符组成的可区分串来替代完整的ID。
TAG信息用来标记来自同一个仓库的不同镜像。例如ubuntu仓库中有多
个镜像，通过TAG信息来区分发行版本，包括
10.04、12.04、12.10、13.04、14.04、16.04等标签。

镜像大小信息只是表示该镜像的逻辑体积大小，实际上由于相同的镜像
层本地只会存储一份，物理上占用的存储空间会小于各镜像的逻辑体积之
和。



images子命令主要支持如下选项，用户可以自行进行尝试。
·-a ，- - all=true|false：列出所有的镜像文件（包括临时文件），默认为
否；
·--digests=true|false：列出镜像的数字摘要值，默认为否；
·-f，--filter=[]：过滤列出的镜像，如
dangling=true只显示没有被使用
的镜像；也可指定带有特定标注的镜像等；
·--format="TEMPLATE"：控制输出格式，如.ID代表 ID信息，. R e p o s i t o r y
代表仓库信息等；
·--no-trunc=true|false：对输出结果中太长的部分是否进行截断，如镜像
的ID信息，默认为是；
·-q ，- - quiet=true|false：仅输出ID信息，默认为否。
其中，对输出结果进行控制的选项如-f，--filter=[]、--
no-trunc=true|
f a l s e 、- q ，- - quiet=true|false等，大部分子命令都支持。
更多子命令选项还可以通过man docker-images来查看。

2.使用tag命令添加镜像标签

可以使用docker tag命令来为本
地镜像任意添加新的标签。例如添加一个新的myubuntu：latest镜像标签：

```
docker tag ubuntu:latest myubuntu:latest
```

再次使用docker images列出本地主机上镜像信息，可以看到多了一个拥
有m y u b u n t u ：latest标签的镜像：

```
docker images
```

之后，用户就可以直接使用myubuntu：latest来表示这个镜像了

这些myubuntu：latest镜像的ID跟u b u n t u ：latest
完全一致。它们实际上指向同一个镜像文件，只是别名不同而已。dockertag
命令添加的标签实际上起到了类似链接的作用。



3.使用inspect命令查看详细信息

使用docker inspect命令可以获取该镜像的详细信息，包括制作者、适应
架构、各层的数字摘要等：

```
docker inspect ubuntu:14.04
```





管理镜像标签

在远端仓库使用search命令进行搜索和过滤

删除镜像标签和镜像文件

创建用户定制的镜像并且保存为外部文件

往Docker Hub仓库中推送自己的镜像