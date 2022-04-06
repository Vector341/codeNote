### switchHost

推荐使用 switchHost 软件切换 host 配置:

host 文件配置：

```
# SPE

# 218 开发环境
10.121.2.218 spe-dev.com oss.spe-dev.com
# 200 物理机压力测试环境

# v0.1.2 测试环境 10.198.30.200 spe-test.com oss.spe-test.com
10.198.30.200 spe-test.com oss.spe-test.com # v0.2.0 test 环境

10.198.30.202 spe-st.com # 对外演示环境

# Sensegear

# PretrelWeb
# .7 为 开发环境
10.5.41.7 storage-dev.sensetime.com passport.storage-dev.sensetime.com
# 中台为登录使用
# 10.5.41.7 passport.storage-test.sensetime.com storage-test.sensetime.com 
# 134 为提测环境
# 10.5.41.134 passport.storage-test.sensetime.com storage-test.sensetime.com 

10.198.30.50 test-2.bizops.sensetime.com
10.198.30.50 test-2.passport.sensegear.sensetime.com


# 新的 storage 测试环境
172.20.21.174 test-3.passport.sensegear.sensetime.com storage-test.sensetime.com
172.20.21.174 test-3.bizops.sensetime.com
172.20.21.174 test-3.passport.sensegear.sensetime.com
172.20.21.174 test-3.monitor.phoenix.sensetime.com

10.151.102.166 local.sensetime.com  # 注意： 这个域名需要使用本地电脑的 IP 地址


# Y-计划
10.142.6.31 dev.passport.y.sensetime.com
10.142.6.31 dev.console.y.sensetime.com # -y 控制台应用
10.142.6.31 dev.monitorboard.y.sensetime.com # -y 看板应用
10.142.6.31 dev.storage.y.sensetime.com # -y 存储应用
10.142.6.31 passport-dev.sensegear.sensetime.com # 登录模块
```

```
# Y-计划 2022/3/2
10.198.44.90 passport.aicp.sensetime.com     
10.198.44.90 dev.passport.y.sensetime.com
10.198.44.90 dev.console.y.sensetime.com
10.198.44.90 dev.monitorboard.y.sensetime.comdev.storage.y.sensetime.com
```

10.142.6.31 passport.aicp.sensetime.com
