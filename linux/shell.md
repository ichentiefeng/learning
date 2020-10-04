

- basename：获取路径中的文件名
- dirname：获取路径中的路径
- whoami：获取当前用户名称

```shell
# 获取文件名称
p1=$1
fname=`basename $p1`
echo fname=$fname

# 获取上级目录到绝对路径
pdir=`cd -P $(dirname $p1); pwd`
echo pdir=$pdir

# 获取当前用户名称
user=`whoami`
echo user=$user
```



