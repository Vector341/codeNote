本文是关于 npm 包内部的 package.json 和 package-lock.json 的说明



npm insatll 以 package.json 文件作为输入，构建一个包含依赖关系的 node_modules 树。

package-lock.json 文件提供了一个更准确的依赖关系，可以生成更准确的 node_modules 文件。

Conceptually, the "input" to [`npm install`](https://docs.npmjs.com/cli/v6/commands/npm-install) is a [package.json](https://docs.npmjs.com/cli/v6/configuring-npm/package-json), while its "output" is a fully-formed `node_modules` tree: a representation of the dependencies you declared. In an ideal world, npm would work like a pure function: the same `package.json` should produce the exact same `node_modules` tree, any time. In some cases, this is indeed true. But in many others, npm is unable to do this. 



npm 包的 package.json 文件里的 name 和 version 是包的唯一标识符

The name and version together form an identifier that is assumed to be completely unique. 





- `~version` "Approximately equivalent to version" See [semver]([semver | npm Docs (npmjs.com)](https://docs.npmjs.com/cli/v6/using-npm/semver#tilde-ranges-123-12-1)) 小版本一致
- `^version` "Compatible with version" See [semver]([semver | npm Docs (npmjs.com)](https://docs.npmjs.com/cli/v6/using-npm/semver#caret-ranges-123-025-004) 大版本一致（不准确，详见文档）