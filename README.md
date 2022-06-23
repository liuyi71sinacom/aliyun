# aliyun

通过阿里云容器镜像服务下载gcr.io镜像
通过kubeadm部署k8s集群，在执行kubeadm init命令时，默认生成的manifests文件夹下yaml文件的镜像都是gcr.io上的，在国内由于被墙而不能正常下载，也就导致了集群不能正常安装。那么问题来了，要怎样才能下载这些镜像呢？我们可以购买vpn，但是这个要带来一定的费用，所以我们要寻找免费下载gcr.io镜像的途径，阿里云就提供了这一服务。本文通过github设置代码源下载所需的gcr.io镜像。

1.Github上建立仓库
首先在github创建一个repository，创建好后然后git clone到本地，并在本地创建所需下载的镜像dockerfile，这里本地的目录层级为 镜像名称->版本号->dockerfile。创建之后再把所有的文件push到github仓库。最后的结果如下：

https://github.com/liuyi71sinacom/aliyun

选取其中一个组件的dockerfile，查看其中的内容
Vi Dockerfile.txt
From k8s.gcr.io/ingress-nginx/controller:v1.0.3@sha256:4ade87838eb8256b094fbb5272d7dda9b6c7fa8b759e6af5383c1300996a7452
MAINTAINER James liuyi71@sina.com

里面的内容非常少，就两行，其中第一行是基于某个镜像制作的新镜像，第二行为作者信息。通过这个dockerfile我们就可以看出端倪来了，其实我们什么都没做，就是利用阿里云这个介质帮我们翻墙去下载镜像。

2. 登录阿里云的容器镜像服务网址
https://cr.console.aliyun.com/cn-hangzhou/instance/repositories，进入管理控制台，在容器镜像服务->镜像列表->创建镜像仓库，按照要求填写相关信息，示例如下

 

这里的设置代码源选择step 1中的github的镜像仓库，在构建设置处选择海外机器构建，选择分支，以及dockerfile目录，这里没有统一的格式，本人只是为了方便以按照 镜像名称->版本号->dockerfile的目录层级做分类。
对于需要下载多个不同镜像需重复操作这一过程，最后的仓库列表如下

 
 

 



3.开始构建
创建完仓库之后，我们就可以进行构建了，在 容器镜像服务->镜像列表->(选择需要创建的镜像)->管理->构建->立即构建 就可以开始构建了，构建截图

 

可以通过构建日志查看构建过程中出现的错误。
下载镜像可以通过基本信息页面的公网网址和版本号下载，下载形式为docker pull 公网网址：版本号 。

4.总结
以上为通过阿里云容器镜像服务下载被墙的镜像，原理也挺简单的，这里把阿里云的服务器当作是一个代理服务器，重新制作新镜像达到下载gcr.io镜像，虽然麻烦了点，但是使用阿里云能够免费下载被墙的镜像，这点还是可取的，这点还是得感谢阿里云提供这样的服务。
