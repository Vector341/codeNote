npm 文档笔记（一）



**public npm registry** 包的发布地

The public npm registry is a database of JavaScript packages, each comprised of software and metadata. 



## 两个概念

package 和 module 是不同的概念。package 贴近于发行， module 贴近于调用 开发

关于 **package** 的描述

- A **package** is a file or directory that is described by a `package.json` file. 

- A package must contain a `package.json` file in order to be published to the npm registry.

- A package is a folder containing a program described by a `package.json` file

A **module** is any file or directory in the `node_modules` directory that can be loaded by the Node.js `require()` function.



## 包的可见性

Visibility of npm packages depends on the **scope** (namespace) in which the package is contained, and the **access level** (private or public) set for the package.

| Scope    | Access level | Can view and download                                        | Can write (publish)                                          |
| -------- | ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Org      | Private      | Members of a team in the organization with read access to the package | Members of a team in the organization with read and write access to the package |
| Org      | Public       | Everyone                                                     | Members of a team in the organization with read and write access to the package |
| User     | Private      | The package owner and users who have been granted read access to the package | The package owner and users who have been granted read and write access to the package |
| User     | Public       | Everyone                                                     | The package owner and users who have been granted read and write access to the package |
| Unscoped | Public       | Everyone                                                     | The package owner and users who have been granted read and write access to the package |