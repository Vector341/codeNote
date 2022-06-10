在使用很多工具时（如typescript）时，官方文档都会要求全局安装，比如下面这样

```bash
$ npm install -g typescript
# after install
$ tsc --version
```

因为要在控制台直接调用 tsc，所以需要全局安装

但有时因为版本依赖的关系，我们不希望全局安装，但仍然希望能在项目中使用命令，那么可以像下面这样



[Run Locally Installed NPM Packages Without Global Install | rockyourcode](https://www.rockyourcode.com/run-locally-installed-npm-packages-without-global-install/)