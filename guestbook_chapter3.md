快速实践，第三节：使用DataFoundry进行CI/CD

1 第三节所覆盖的知识点

在第三节，我们将学会：

如何对部署的应用进行CI/CD的配置
2 对部署的应用进行CI/CD配置

2.1 图形界面操作

新建服务-高级配置页面配置CI/CD选项配置，如下图所示：



勾选“镜像变化触发自动部署”后，当容器所基于的镜像发生改变时，就会触发服务基于新的镜像重新部署。

2.2 命令行操作

我们可以通过命令对dc,bc和rc的triggers进行操作。这里我们需要介绍triggers的概念：triggers就是指一些能够触发CI/CD的条件。datafoundry中包括两种triggers，分别为配置文件发生改变时触发自动部署和镜像发生改变时触发自动部署。

2.2.1 列出资源中所有的triggers条件

$ oc set triggers dc/wordpress
会得到以下结果：

NAME                         TYPE    VALUE                         AUTO
deploymentconfigs/wordpress  config                                true
deploymentconfigs/wordpress  image   wordpress:master (wordpress)  true
我们可以看到以上结果中列出了两个trigger条件，分别为dc配置文件发生改变时触发自动部署和WordPress:master这个镜像发生改变时触发自动部署。

2.2.2 删除所有的triggers

$ oc set triggers dc/wordpress --remove-all
再次查看dc/wordpress的triggers时，我们会发现没有任何trigger条件了。

也可以删除单独的trigger，使用以下命令：

$ oc set triggers dc/wordpress --from-config --remove
执行完这条命令后，当dc的配置文件发生改变时，就不会触发自动部署了。

下面我们来逐步为它增加triggers，以此来熟悉增加trigger的命令。

2.2.3 增加trigger

$ oc set triggers  dc/wordpress --from-image=wordpress:master --containers=wordpress
这条命令中，需要指定--containers，否则会报错。--containers内容为pod配置文件中Containers字段下的容器名字。也可以通过以下命令增加dc配置文件发生改变时触发部署的trigger：

$ oc set triggers  dc/wordpress --from-config=true
