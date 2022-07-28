# dcp-storage 项目打包部署

## 环境url

线上、测试、开发

http://dcp.pages.gitlab.sh.sensetime.com/docpages/#/pages/projects/storage/storage

## 分支说明

feature/storage-v1.3.2 测试用分支，修bug切

feature/storage-v1.3.2-2 开发分支，修好bug合进来

feature/storage-zyf-v1.3.2-2 我的开发分支



## 打包并推送 docker 镜像

1、 在项目根目录执行： make package-y-storage
2、 登录 公司镜像仓库  docker login registry.sensetime.com
3、 推送目标镜像 docker push registry.sensetime.com/sensegear/fe-dcp-storage:1.3.1-prod-1657520584

**前置依赖（执行make前安装）**

- kubectl & docker
- helm & helm push
- jq(brew 安装)



## 部署 Jenkins



部署 IP http://10.5.8.164:8090/

账号、密码：alankzh  123 



