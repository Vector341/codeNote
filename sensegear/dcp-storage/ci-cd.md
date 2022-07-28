# sensegear 前端交付与部署

本篇文章会带大家了解前端交付和部署的相关知识，介绍 sensegear 应用的网络架构，最后会介绍在项目中是怎么通过 make 命令进行串联的。

## 交付与部署

### 交付

sensegear 前端交付是指生成直接运行的 docker 和 chart 的 artifact，artifact 是前端代码的交付物。

### 部署

sensegear 前端部署是在对应的环境中把构建的 artifact 用合适的方式运行起来，启动前端服务。

在构建和部署中会涉及到 Docker，Kubernetes，Helm 和 Helmfile 的相关知识点，下面会对这些进行一个简要的介绍。

## 前置知识内容

### Docker

Docker 是一个开源的应用容器引擎，让开发者可以打包他们的应用以及依赖包到一个可移植的镜像中，然后发布到任何流行的 Linux或Windows 机器上，也可以实现虚拟化。容器是完全使用沙箱机制，相互之间不会有任何接口。

#### Docker 镜像

> docker images

![image-20210705154906495](./images/image-20210705154906495.png)

镜像可以理解为一个静态的系统，它包含各种你导入的环境变量，可执行程序，目录层级，代码内容等，但是它没有运行起来，没有提供服务。

#### Docker 容器

> docker run -it --rm -p 8088:80 registry.sensetime.com/sensegear/fe-dcp-operation:1.0.0 /bin/bash

![image-20210705155728243](./images/image-20210705155728243.png)

![image-20210705160345729](./images/image-20210705160345729.png)

可以通过 docker run 命令启动容器，上面的命令是指定本机的 8088 端口映射到容器内的 80 端口，容器启动的时候会运行 Dockerfile 指定的入口脚本文件，入口脚本文件一般是启动服务的脚本。容器启动后可以通过 `docker ps` 查看。

### Kubernetes(k8s)

Kubernetes是Google开源的一个容器编排引擎，它支持自动化部署、大规模可伸缩、应用容器化管理。在生产环境中部署一个应用程序时，通常要部署该应用的多个实例以便对应用请求进行负载均衡。在Kubernetes中，我们可以创建多个容器，每个容器里面运行一个应用实例，然后通过内置的负载均衡策略，实现对这一组应用实例的管理、发现、访问，而这些细节都不需要运维人员去进行复杂的手工配置和处理。

![image-20210705161051028](./images/image-20210705161051028.png)

Pod 是 k8s 运行的最小单元，可以理解为一个虚拟的节点，k8s 按照系统负载进行服务的扩缩容。这里可以理解为 docker 镜像在 k8s 上运行产生 pod 进程。

#### Helm

The package manager for Kubernetes，helm 是 Kubernetes 的包管理器，包的单位叫做 chart。

常见命令

``` bash
helm add repo sensegear # 添加 sensegear 仓库
helm repo update # 仓库的远端更新
helm push(pull) sensegear/fe-operation # 推送/拉取 包内容 
helm install/uninstall chartName # 卸载/安装 k8s 里的 chart 版本
```

Helm 的配置文件可以指定 Docker image 的 registry 和 tag，可以理解为 chart 包和 Docker image 是一一对应关系。[这篇文章](https://www.yuque.com/qinglu-yoz9e/dlayhu/xg1qi7)里有更清晰的描述。

#### Helmfile

一款可以让 helm 更好工作的软件，解决了 `Helm 不提供 apply 命令`，`不方便控制安装的 chart 版本`，`Values 必须是纯文本` 等。

## SenseGear 网络架构

SenseGear 以 phoenix 应用为例，其他的服务部署原理是一致的。

![image-20210705180807301](./images/image-20210705180807301.png)

在 SenseGear 的结构里，Ingress 网关承担了转发作用，前端 pods 只需要提供 80 端口可用访问的静态资源。

### SenseGear 镜像

SenseGear 镜像的生成分为下面两步。

* 运行 `npm run bootstrap` 按照依赖， 运行 `npm run build:pro` 生成各子项目的 dist 文件夹。
* Dockerfile EntryPoint 指定脚本为 start.sh，在各 dist 目录里启动静态服务器。

### SenseGear 前端服务的更新

![image-20210705181907841](./images/image-20210705181907841.png)

## 使用 Make 进行串联

系统里使用了 make 对应用的线下部署，打包，交付物推送等操作进行了封装。现在所有的操作都可以用 make 命令完成了！

### 本地部署

sensegear 系统运行本质是一个大的 k8s 集群，里面运行了前后端的各种 chart 资源，理论上我们是可以在本机起一个 k8s 集群，再把后端 chart 和前端 chart 分别部署进来，就完成了 sensegear 的本地部署。本地部署不是应用上线的必需条件，可以选读。

#### 前置依赖

* kubectl & docker
* helm & helm push
* jq(brew 安装)
* java/go 环境等

#### 拉起集群与网关

进入 sensegear/backend 仓库，运行：

``` bash
make deploy-demo
```

该命令会打包后端的 java/golang 等应用的 chart 到 k8s 集群，再启动网关服务，网关是一个 nginx-ingress 服务，通过域名来区分不同的前端 APP

#### 部署前端服务

前端服务以 operation 应用为例，operation 的本地域名指定是在 `helmfile/operation/helmfile/profile-dev/fe-operation` 目录下的，假定现在设置为 `213.bizops.sensetime.com`。

切回到前端仓库，运行下面命令，将 operation 在 k8s 集群里运行起来。

``` bash
make deploy-operation
```

再调整本机的 hosts 文件，将 `213.bizops.sensetime.com` 域名指向本机

``` bash
127.0.0.1 213.bizops.sensetime.com
```

如果一切都顺利，最后访问 `213.bizops.sensetime.com:8080` 应该可以成功。

### 本地调试

更多的时候，前端在本地开发将对于的路由直接转发到后端机器会更合适一些，各子应用内部都统一封装了 start 命令，如果是使用 umi 的应用会启动 webpack-dev-server 的 proxy 服务，对应的前端项目在开发的过程中可以直接修改对应的 proxy 配置，以 operation 为例。对应的路由直接转发到指定的后端地址。

``` javascript
const serves = {
  '/api': 'http://10.151.93.77:8080/api',
  '/grafana_api': 'http://monitor.phoenix.sensetime.com/sensegear/grafana',
}
```

### 打包与推送

打包命令是 `make package-*`，该命令会运行对应项目下的 `build:pro` 命令，再构建出对应的 docker 镜像与 chart 包。
推送命令是 `make publish-*`，该命令会将上一步构建出来的镜像与 chart 包推送到 sensegear 仓库中(本地终端要提前登陆 docker 与 helm 仓库)。

其中 "*" 的组成由应用服务系统和应用名组成，比如 dcp 系统下的 operation 即为 "dcp-operation"; 再比如 y 系统下的 console 即为 "y-console"

package-dcp-operation 命令会生成

* docker 镜像: registry.sensetime.com/sensegear/fe-dcp-operation:1.0.0
* helm 包: fe-dcp-operation:0.2.0

package-y-monitor-portal 命令会生成

* docker 镜像: registry.sensetime.com/sensegear/fe-y-monitor-portal:1.0.0
* helm 包: fe-y-monitor-portal:0.2.0

注意： 打包产生 docker image 的 tag 由 ```package.json``` 中填写的 version 决定，helm 包的版本由 helm 配置文件指定

### make 命令汇总

| 命令 | 用途 |
| ----- | --- |
| ```make package-operation``` | 构建 operation App 的 Docker image & helm chart|
| ```make publish-operation``` |  推送 operation App 的 Docker image |
| ```make clean``` | 删除构建的中间文件（不包括image） |

### 启用 gitlabCI

gitlab ci 是由根目录的 `.gitlab-ci.yml` 定义的，是否在远端编译是由分支名决定的，比如新建分支 `feature/spe-console-v1.1`，在此分支上面提交的代码会触发 `package-spe` 命令对 SPE 控制台镜像和 helm 包进行构建推送，同理分支 `feature/dcp-operation-v2.0`, `feature/y-console-v1.0` 会分别构建 dcp-operation 和 y-console 项目。

### 应用部署

在推送步骤确认成功后，我们要更新测试环境(线上环境)的代码，一般是走Jenkins的 ci/cd 流程。

目前 Jenkins 有区分 dcp 应用部署和 spe 应用部署

| 应用 | 部署环境 | Jenkins 地址 | 登录账户 |
| ----- | --- | --- | --- |
| spe-* | dev,test | `http://10.5.8.164:8090/` | zhouzhen/123456 |
| dcp-* | 测试环境2,测试环境3 | `http://10.5.8.164:8090/` | alankzh/123 |