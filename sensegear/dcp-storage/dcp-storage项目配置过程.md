## 运行配置

switchHost 配置

nginx 启动

登入内网或者挂vpn



## 开发设置

**切换环境**

```js
// nodejs/packages/dcp-storage/config/dev.proxy.js

const serves = {
  '/api': 'http://storage-test.sensetime.com/api/', // 测试环境
  // '/api': 'http://dev.storage.y.sensetime.com/api/', // y 计划环境
  // '/grafana_api': 'http://monitor.phoenix.sensetime.com/sensegear/grafana',
  '/grafana_api': 'http://storage-test.sensetime.com/sensegear/grafana',
}
```

通过 response header 的 x-real-url 判断环境



**切换路由布局**	

```js
// nodejs/packages/dcp-storage/src/models/base.js

yield put({ type: 'setState', payload: { attr: 'userInfo', value: info } })
yield put({ type: 'setQuotaInfo' })
yield put({ type: 'setState', payload: { attr: 'role', value: roleNamesEnMap[info.type] } })
// TODO 测试使用
// yield put({ type: 'setState', payload: { attr: 'role', value: developer } })
// yield put({ type: 'setState', payload: { attr: 'role', value: superAdmin } })
// yield put({ type: 'setState', payload: { attr: 'role', value: groupAdmin } })
// yield put({ type: 'setState', payload: { attr: 'role', value: devOpsAdmin } })
// yield put({ type: 'setState', payload: { attr: 'role', value: deputyGroupAdmin } })

```

