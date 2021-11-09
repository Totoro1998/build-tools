### npm access

设置已发布包的访问级别

### npm adduser

### npm audit

audit命令将项目中配置的依赖项的描述提交到your default registry，并要求提供已知漏洞的报告。如果发现任何漏洞，则将计算影响和适当的补救措施。如果提供了fix参数，那么修复将应用于包结构中

如果未发现漏洞，该命令将以 0 退出代码退出

### npm bin

Print the folder where npm will install executables。(Display npm bin folder

Version 7.x (Current release))

### npm bugs

### npm cache

管理包缓存

- add: Add the specified packages to the local cache. This command is primarily intended to be used internally by npm, but it can provide a way to add data to the local installation cache explicitly.
  
  npm cache add <name>@<version>...

- clean: 删除缓存文件夹中的所有数据。请注意，这通常是不必要的，因为 npm 的缓存是自我修复的并且可以抵抗数据损坏问题
  
  npm cache clean

- verify: 验证缓存文件夹的内容，垃圾收集任何不需要的数据，并验证缓存索引和所有缓存数据的完整性
  
  npm cache verify

### npm ci

- npm i依赖package.json，而npm ci依赖package-lock.json。
- 当package-lock.json中的依赖于package.json不一致时，npm ci退出但不会修改package-lock.json。
- npm ci只可以一次性的安装整个项目依赖，但无法添加单个依赖项。
- npm ci安装包之前，会删除掉node_modules文件夹，因此他不需要去校验已下载文件版本与控制版本的关系，也不用校验是否存在最新版本的库，所以下载的速度更快。
- npm安装时，不会修改package.json与package-lock.json

### npm completion

### npm config

管理npm配置文件

npm config set

npm config get

npm config delete

npm config list

npm config edit

### npm dedupe

### npm deprecate

### npm diff

### npm dist-tag

```
npm dist-tag add <pkg>@<version> [<tag>]
npm dist-tag rm <pkg> <tag>
npm dist-tag ls [<pkg>]

aliases: dist-tags
```

Add, remove, and enumerate(枚举) distribution tags on a package

A tag can be used when installing packages as a reference to a version instead of using a specific version number

Publishing a package sets the `latest` tag to the published version unless the `--tag` option is used. For example, `npm publish --tag=beta`

By default, `npm install <pkg>` (without any `@<version>` or `@<tag>` specifier) installs the `latest` tag.

### npm docs

### npm doctor

### npm edit

### npm exec

从本地或远程包中运行命令，此命令允许您从 npm 包（本地安装的或远程获取的）中运行任意命令，其上下文与通过 npm run 运行它的上下文类似

### npx

从本地或远程npm包运行命令

**`npx`的原理很简单，就是运行的时候，会到`./node_modules/.bin`路径和环境变量`$PATH`里面，检查命令是否存在**

由于`npx`会检查环境变量`$PATH`，所以系统命令也可以调用。

注意，`Bash`内置的命令不在`$PATH`里面，所以不能用。比如，`cd`是`Bash`命令，因此就不能用`npx cd`。

**除了调用项目内部包，`npx`还可以从`npm`仓库下载包到本地全局，使用完以后再删除。也就是说，可以使用`npx`来使用你本地没有安装过但是存在`npm`仓库上的包**

- --package 选项指定的任何包都将在执行命令的 PATH 中提供，以及任何本地安装的包可执行文件,--package 选项可以多次指定，以在所有指定的包都可用的环境中执行提供的命令

- 如果本地项目dependencies中不存在任何requested packages，则将它们安装到 npm 缓存中的文件夹中，该文件夹在执行过程中添加到 PATH 环境变量中,A prompt is printed (可以通过提供--yes和--no来抑制该提示).

- 不带specifier的包名将与本地项目中存在的任何版本匹配。如果包名与本地依赖项具有完全相同的名称和版本，则仅将带有specifier的包名视为匹配项

- 如果未提供 -c 或 --call 选项，则使用positional arguments生成命令字符串

- 如果未提供 --package 选项，则 npm 将尝试根据以下启发式从作为第一个positional argument提供的包说明符中确定可执行文件名称
  
  - 如果包在 package.json 的 bin 字段中有一个entry，或者如果所有entries都是同一命令的别名，则将使用该命令
  
  - 如果包有多个 bin entries，并且其中一个与 name 字段的unscoped部分匹配，则将使用该命令
  
  - If this does not result in exactly one option（因为没有 bin 条目，或者它们都不匹配包的名称），则 npm exec 退出并显示错误
  
  - To run a binary other than the named binary,指定 one or more --package options, which will prevent npm from inferring(推断) the package from the first command argument
  
  - npx vs npm exec
    
    - When run via the `npx` binary, all flags and options *must* be set prior to any positional arguments. When run via `npm exec`, a double-hyphen(连字符) `--` flag can be used to suppress(压制) npm's parsing of switches and options that should be sent to the executed command
    
    $ npx foo@latest bar --package=@npmcli/foo
    
    在这种情况下, npm will resolve the `foo` package name, and run the following command:  foo bar --package=@npmcli/foo，由于 --package 选项位于positional arguments之后，因此它被视为已执行命令的参数
    
    In contrast, due to npm's argument parsing logic, running this command is different
    
    $ npm exec foo@latest bar --package=@npmcli/foo
    
    In this case, npm will parse the --package option first, resolving the @npmcli/foo package. Then, it will execute the following command in that context:     $ foo@latest bar
    
    建议使用双连字符来明确告诉 npm 停止解析command line options and switches。因此，以下命令等效于上面的 npx 命令
    
    $ npm exec -- foo@latest bar --package=@npmcli/foo

### npm explain

### npm explore

### npm find-dupes

### npm fund

### npm help

### npm help-search

### **npm init**

创建package.json文件

npm init <initializer> 可用于设置新的或现有的 npm 包，在这种情况下，initializer是一个名为 create-<initializer> 的 npm 包，它将由 npm exec 安装，然后执行其main bin—— 大概是创建或更新 package.json 并运行任何其他与初始化相关的操作，可以转化为对应的 npm exec

- `npm init foo` -> `npm exec create-foo`
- `npm init @usr/foo` -> `npm exec @usr/create-foo`
- `npm init @usr` -> `npm exec @usr/create`

您还可以使用 -y/--yes 完全跳过问卷。如果传递--scope，它将创建一个scope包

`initializer`在这里是一个名为`create-<initializer>`的`npm`包，该包将由`npx`来安装，然后执行其`package.json`中`bin`属性对应的脚本，会创建或更新`package.json`并运行一些与初始化相关的操作。

`npm init <initializer>`时转换成`npx`命令的规则为：

- `npm init foo` -> `npx create-foo`
- `npm init @usr/foo` -> `npx @usr/create-foo`
- `npm init @usr` -> `npx @usr/create`

Create a new React-based project using [`create-react-app`](https://npm.im/create-react-app):

    $ npm init react-app ./my-react-app

Create a new `esm`-compatible package using [`create-esm`](https://npm.im/create-esm):

    $ mkdir my-esm-lib && cd my-esm-lib
    $ npm init esm --yes

### npm link

链接到包文件夹中，在本地开发npm模块的时候，我们可以使用npm link命令，将npm 模块链接到对应的运行项目中去，方便地对模块进行调试和测试

首先，包文件夹中的 npm link 将在全局文件夹 {prefix}/lib/node_modules/<package> 中创建一个symlink,links to the package where the `npm link`

接下来，在其他位置， npm link package-name 将创建一个从全局安装的 package-name 到当前文件夹的 node_modules/ 的symlink

注意 package-name 取自 package.json，而不是目录名

### npm install

    -P, --save-prod: Package will appear in your dependencies. This is the default unless -D or -O are present.
    
    -D, --save-dev: Package will appear in your devDependencies.
    
    -O, --save-optional: Package will appear in your optionalDependencies.
    
    --no-save: Prevents saving to dependencies

### npm outdated

检测过时的包

This command will check the registry to see if any (or, specific) installed packages are currently outdated

By default, only the direct dependencies of the root project and direct dependencies of your configured *workspaces* are shown. Use `--all` to find all outdated meta-dependencies as well.

### npm pack

从包中创建源码压缩包,

对于任何可安装的内容（即，软件包文件夹，tarball，tarball url，name@tag ，name@version ，名称或作用域名称），此命令会将其提取到缓存中，

然后将 tarball 复制到当前的工作环境中目录为`<name>-<version>.tgz`，然后将文件名写出到 stdout。

如果多次指定相同的程序包，则文件将第二次被覆盖

如果未提供任何参数，则 npm 会打包当前包文件夹    

### npm publish

默认情况下，npm will publish to the public registry. This can be overridden by specifying a different default registry or using a [`scope`](https://docs.npmjs.com/cli/v7/using-npm/scope) in the name (see [`package.json`](https://docs.npmjs.com/cli/v7/configuring-npm/package-json))

    npm publish [<tarball>|<folder>] [--tag <tag>] [--access <public|restricted>] [--otp otpcode] [--dry-run]
    
    Publishes '.' if no argument supplied
    Sets tag 'latest' if no --tag specified
    
    
    
    
    
    <folder>：包含 package.json 文件的文件夹
    
    <tarball>：压缩后的 tar 存档的 url 或文件路径，其中包含单个文件夹，其中包含 package.json 文件。
    
    [--tag <tag>] 使用给定标签注册发布的软件包，例如npm install @将安装此版本。默认情况下，npm publish更新和npm install安装latest标签。有关标签的详细信息，请参见npm-dist-tag。
    
    [--access <public|restricted>] 告诉注册表此软件包是应公开发行还是受限制发行。仅适用于作用域包，默认为 restricted。如果您没有付费帐户，则必须使用发布与 –access public 发布有范围的软件包。
    
    [--otp <otpcode>] 如果您在 auth-and-writes 模式下启用了双重身份验证，那么您可以为此提供来自身份验证器的代码。如果您不包括此文件，而您正在从 TTY 中运行，则会提示您。
    
    [--dry-run] 从开始 npm@6，除了实际发布到注册表外，所有发布都可以完成。报告将要发布的内容的详细信息。
    
    如果指定的注册表中已经存在软件包名称和版本组合，则失败

### npm run-script

这会从包的“脚本”对象运行任意命令。如果没有提供“命令”，它将列出可用的脚本

run[-script] 由 test、start、restart 和 stop 命令使用，但也可以直接调用。包中的脚本打印出来后，分为生命周期（测试、启动、重启）和直接运行脚本

任何positional arguments都传递给指定的脚本。使用 -- 传递 --prefixed 标志和选项，否则它们会被 npm 解析

参数只会传递给 npm run 之后指定的脚本，不会传递给任何 pre 或 post 脚本

env 脚本是一个特殊的内置命令，可用于列出脚本在运行时可用的环境变量。如果包中定义了“env”命令，它将优先于内置命令

除了 shell 预先存在的 PATH 之外，npm run 还将 node_modules/.bin 添加到提供给脚本的 PATH

本地安装的依赖项提供的任何二进制文件都可以在没有 node_modules/.bin 前缀的情况下使用。例如，如果你的包中有一个 devDependency on tap，你应该写

    "scripts": {"test": "tap test/*.js"}

您的脚本在其中运行的实际 shell 取决于平台。默认情况下，在类 Unix 系统上它是 /bin/sh 命令，在 Windows 上它是 cmd.ex

/bin/sh 所指的实际 shell 也取决于系统。您可以使用script s-shell配置自定义shell

脚本从包文件夹的根目录运行，无论调用 npm run 时当前工作目录是什么

如果您希望脚本根据您所在的子目录使用不同的行为，您可以使用 INIT_CWD 环境变量，它保存您运行 npm run 时所在的完整路径

npm run 将 NODE 环境变量设置为执行 npm 的node可执行文件

如果您尝试在没有 node_modules 目录的情况下运行脚本并且失败了，您将收到运行 npm install 的警告，以防万一您忘记了

npm install-ci-test

npm install-test

npm logout

npm hook

npm org

npm owner

npm ping

npm pkg

npm perfix

npm profile

npm prune

npm rebuild

npm repo

npm restart

npm root

npm search

npm set-script

npm shirkwrap

npm star

npm ls

npm stars

npm start

npm stop

npm team

npm test

npm token

npm uninstall

这将卸载一个包，完全删除代表它安装的所有 npm

npm unpublish

npm unstart

### npm update

更新包，此命令会将列出的所有包更新为最新版本，它还将安装缺少的软件包。

### npm version

**在`git`环境中，执行`npm version`修改完版本号以后，还会默认执行`git add`->`git commit`->`git tag`操作**

npm view

### **npm whoami**

显示npm用户名
