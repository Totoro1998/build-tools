#### **name**

- The names of scoped packages can begin with a dot or an underscore. This is not permitted without a scope（有作用域范围的包可以以dot或underscore开头，但是无作用域范围的不被允许）

- New packages must not have uppercase letters in the name （不允许有大写字母）

- The name ends up being part of a URL, an argument on the command line, and a folder name. Therefore, the name can't contain any non-URL-safe characters（名称最终成为 URL 的一部分、命令行上的参数和文件夹名称。因此，名称不能包含任何非 URL 安全字符）

- A name can be optionally prefixed by a scope, e.g. @myorg/mypackage. See scope for more detail （名称可以选择性地以范围为前缀，例如@myorg/mypackage，针对于组织Organizations）

#### **version**

该值需要符合[语义化版本系统]([语义化版本 2.0.0 | Semantic Versioning](https://semver.org/lang/zh-CN/))

1. 主版本号：当你做了不兼容的 API 修改，
2. 次版本号：当你做了向下兼容的功能性新增，
3. 修订号：当你做了向下兼容的问题修正

![](/Users/totoro.fu/Library/Application%20Support/marktext/images/2021-11-03-14-49-02-image.png)

**依赖包版本号**

1. 指定版本：比如"classnames": "2.2.5"，表示安装2.2.5的版本

2. **"1.x" 、"1.X"、1.*"、"1"、"*"**,表示使用通配符的版本号。x、X、* 和 （空） 的含义相同，都表示可以匹配任何内容。

3. **"1.2.3-alpha.1"、"1.2.3-beta.1"、"1.2.3-rc.1"**
   
       alpha(α)：预览版，或者叫内部测试版；一般不向外部发布，会有很多bug；一般只有测试人员使用。
       
       beta(β)：测试版，或者叫公开测试版；这个阶段的版本会一直加入新的功能；在alpha版之后推出。
       
       rc(release candidate)：最终测试版本；可能成为最终产品的候选版本，如果未出现问题则可发布成为正式版本。

4.  波浪号~+指定版本：比如 "babel-plugin-import": "~1.1.0",表示安装1.1.x的最新版本（不低于1.1.0），但是不安装1.2.x，也就是说安装时不改变大版本号和次要版本号
   
   1. ^+指定版本：比如 "antd": "^3.1.4",，表示安装3.1.4及以上的版本，但是不安装4.0.0，也就是说安装时不改变大版本号,表示兼容补丁和小版本更新的版本号。官方的定义是“能够兼容除了最左侧的非 0 版本号之外的其他变化”
   
   **依赖包版本管理**
   
   1、在大版本相同的前提下，如果一个模块在`package.json`中的小版本要**大于**`package-lock.json`中的小版本，则在执行`npm install`时，会将该模块更新到大版本下的最新的版本，并将版本号更新至`package-lock.json`。如果**小于**，则被`package-lock.json`中的版本锁定
   
   2、如果一个模块在`package.json`和`package-lock.json`中的大版本不相同，则在执行`npm install`时，都将根据`package.json`中大版本下的最新版本进行更新，并将版本号更新至`package-lock.json`
   
   3、如果一个模块在`package.json`中有记录，而在`package-lock.json`中无记录，执行`npm install`后，则会在`package-lock.json`生成该模块的详细记录。同理，一个模块在`package.json`中无记录，而在`package-lock.json`中有记录，执行`npm install`后，则会在`package-lock.json`删除该模块的详细记录
   
   4、如果要更新某个模块大版本下的最新版本（升级小版本号），请执行如下命令：npm update packageName
   
   5、如果要更新到指定版本号（升级大版本号），请执行如下命令：npm install packageName@x.x.x
   
   6、卸载某个模块，请执行如下命令：npm uninstall packageName
   
   7、安装模块的确切版本：npm install packageName -D/S --save-exact # 安装的版本号将会是精准的，版本号前面不会出现^~字符
   
   #### **files**
- 是一个可选字段，是一个匹配文件的正则数组，描述了当您的包作为依赖项安装时要包含的文件条目

- 语法和.gitignore相同，但是作用相反

- including a file, directory, or glob pattern (*, **/*, and such) will make it so that file is included in the tarball when it's packed （包含文件、目录、全局模式可以在打包的时候将文件包含在压缩包中，默认值为*，包含所有文件）

- Files included with the "package.json#files" field *cannot* be excluded through `.npmignore` or `.gitignore`（被该值匹配上的文件不能被.npmignore或者.gitignore文件排除）

- 该值有默认值，详情可见 https://docs.npmjs.com/cli/v7/configuring-npm/package-json#files

#### **.npmignore**

您还可以在包的根目录或子目录中提供.npmignore文件，这将防止文件被包含,在包的根目录中，它不会覆盖“files”字段，但在子目录中会覆盖。npmignore文件的原理与gitignore类似。如果有.gitignore文件，而且.npmignore缺失，则.gitignore的内容将被使用

#### **main**

- 程序的主要入口点，在brower和Node环境中都可以使用。例如，假如你的包名是foo，如果有用户install,and then does `require("foo")`, then your main module's exports object will be returned

- 这应该是相对于包文件夹根目录的模块。

- 对于大多数模块来说，拥有一个main script是最有意义的，通常没有太多其他的东西

- 默认值为根目录下的index.js

#### **browser**

如果你的模块要在客户端使用，那么应该使用该而不是main字段

#### **module**

module字段可以定义 npm 包的 ESM 规范的入口文件，browser 环境和 node 环境均可使用。如果 npm 包导出的是 ESM 规范的包，使用 module 来定义入口文件

在Web环境中，如果使用loader加载ESM（ES module），那么这三个配置的加载顺序是browser→module→main，如果使用require加载CommonJS模块，则加载的顺序为main→module→browser。​

Webpack在进行项目构建时，有一个target选项，默认为Web，即构建Web应用。如果需要编译一些同构项目，如node项目，则只需将webpack.config.js的target选项设置为node进行构建即可。如果再Node环境中加载CommonJS模块，或者ESM，则只有main字段有效

#### **bin**

软链接（符号链接）是一类特殊的可执行文件， 其包含有一条以绝对路径或者相对路径的形式指向其它文件或者目录的引用。在`bin`目录下执行`ll`指令可以查看具体的软链接指向。在对链接文件进行读或写操作的时候，系统会自动把该操作转换为对源文件的操作，但删除链接文件时，系统仅仅删除链接文件，而不删除源文件本身。

- 在全局模式下，可执行文件在 Unix 上链接到 {prefix}/bin，或在 Windows 上直接链接到 {prefix}。确保路径是在终端的 PATH 环境中以运行它们

- 在本地模式下，可执行文件链接到 ./node_modules/.bin 以便它们可用于通过 npm 运行的脚本。 （例如，当您运行 npm test 时，测试运行器将位于路径中。)

- 许多包都有一个或多个希望安装到PATH中的可执行文件。NPM使这变得非常简单(事实上，它使用这个特性来安装“NPM”可执行文件)。

- 要使用它，请在 package.json 中提供一个 bin 字段，它是命令名到本地文件名的映射，当这个包被全局安装时，可以按命令名在全局内找到该文件。当该包作为一个依赖安装到其他包时

- 如果您有一个可执行文件，那么它的名称应该是包的名称

- 请确保 bin 中引用的文件以 #!/usr/bin/env 节点开头（告诉系统用node解析），否则脚本将在node没有可执行文件的情况下启动！

#### **scripts**

是包含在 package中不同生命周期中不同时间运行的脚本命令的dictionary，The key is the lifecycle event, and the value is the command to run at that point

除了运行基本的scripts命令，还可以结合pre和post完成前置和后续操作

**PATH环境变量**

在`terminal`中执行命令时，**命令会在`PATH`环境变量里包含的路径中去寻找相同名字的可执行文件**。局部安装的包只在`./node_modules/.bin`中注册了它们的可执行文件，不会被包含在`PATH`环境变量中，这个时候在`terminal`中输入命令将会报无法找到的错误

#### **config**

用于设置scirpts的配置参数，持续整个upgrades

#### **dependencies**

指定了项目运行所依赖的模块

Dependencies can also be identified with a tarball or git URL

As of version 2.0.0 you can provide a path to a local directory that contains a package. Local paths can be saved using `npm install -S` or `npm install --save`, using any of these forms: （本地paths）

关于版本的写法，可以参考 https://docs.npmjs.com/cli/v7/configuring-npm/package-json#dependencies

#### **devDependencies**

指定项目开发所需要的模块，与dependencies的区别是

一是在你安装一个外部模块时，不会自动安装这个模块下packagejson里边devDependencies的依赖（因为你只是要使用这个模块，而不是要开发这个模块，这个模块的正常功能不需要devDependencies的依赖）；二是在运行npm install时加入--production有区别，加入这个参数不会下载devDependencies的依赖

#### **peerDependencies**

有些情况下，我们的项目和所依赖的模块，都会同时依赖另一个模块，但是所依赖的版本不一样。比如，我们的项目依赖A模块和B模块的1.0版，而A模块本身又依赖B模块的2.0版。大多数情况下，这不是问题，B模块的两个版本可以并存，同时运行。但是，有一种情况，会出现问题，就是这种依赖关系将暴露给用户

最典型的场景就是插件，比如A模块是B模块的插件。用户安装的B模块是1.0版本，但是A插件只能和2.0版本的B模块一起使用。这时，用户要是将1.0版本的B的实例传给A，就会出现问题。因此，需要一种机制，在模板安装的时候提醒用户，如果A和B一起安装，那么B必须是2.0模块

需要注意，从npm 3.0版开始，peerDependencies不再会默认安装了。

#### **peerDependenciesMeta**

当用户安装你的包时，如果尚未安装peerDependencies中指定的包，npm将发出警告。peerDependenciesMeta字段为npm提供更多关于如何使用peerDependencies的信息。具体来说，它允许将对等依赖标记为可选的

```
{
  "name": "tea-latte",
  "version": "1.3.5",
  "peerDependencies": {
    "tea": "2.x",
    "soy-milk": "1.2"
  },
  "peerDependenciesMeta": {
    "soy-milk": {
      "optional": true
    }
  }
}
```

#### **bundledDependencies**

指定发布时会被一起打包的模块，发布包时需要打包包的依赖项

如果你需要在本地保存npm包或者通过单个文件下载来获取它们，你可以通过在bundledDependencies数组中指定包名并执行npm pack来将这些包打包到压缩包文件中

注意，包名不包括任何版本，因为这些信息是在依赖项中指定的

#### **optionalDependencies**

如果一个依赖可以被使用，但是你希望npm在找不到或者安装失败的情况下继续运行，那么你可以把它放在optionalDependencies对象中。这是包名到版本或url的映射，就像dependencies对象一样。不同之处在于构建失败不会导致安装失败。运行npm install——no-optional将阻止安装这些依赖项

optionalDependencies 中的条目将覆盖dependencies中的同名条目，因此通常最好只放在一个地方

#### **engines**

`engines`字段指明了该模块运行的平台，比如`Node`或者`npm`的某个版本或者浏览器。

#### **private**

如果这个属性被设置为`true`，`npm`将拒绝发布它，这是为了防止一个私有模块被无意间发布出去。

#### **publishConfig**

This is a set of config values that will be used at publish-time. It's especially handy if you want to set the tag, registry or access, so that you can ensure that a given package is not tagged with "latest", published to the global public registry or that a scoped module is private by default.

这个配置是会在模块发布时生效，用于设置发布用到的一些值的集合。如果你不想模块被默认标记为最新的，或者默认发布到公共仓库，可以在这里配置tag或仓库地址。

通常`publishConfig`会配合`private`来使用，如果你只想让模块被发布到一个特定的`npm`仓库，如一个内部的仓库

#### **description**

#### **keywords**

#### **homepage**

#### **bugs**

#### **license**

#### **author**

#### **contributors**

#### **funding**

#### **man**

#### **repository**

#### **os**

#### **cup**

#### **workspaces**

## 第三方配置

#### typeing

typings字段用来指定TypeScript的入口文件：

#### **eslintConfig**

eslint的配置可以写在单独的配置文件.eslintrc.json 中，也可以写在package.json文件的eslintConfig配置项中

#### **babel**

babel用来指定Babel的编译配置

#### unpkg

使用该字段可以让 npm 上所有的文件都开启 cdn 服务，该CND服务由unpkg提供：

#### lint-staged

lint-staged是一个在Git暂存文件上运行linters的工具，配置后每次修改一个文件即可给所有文件执行一次lint检查，通常配合gitHooks一起使用，使用lint-staged时，每次提交代码只会检查当前改动的文件。

#### **gitHooks**

gitHooks用来定义一个钩子，在提交（commit）之前执行ESlint检查。在执行lint命令后，会自动修复暂存区的文件。修复之后的文件并不会存储在暂存区，所以需要用git add命令将修复后的文件重新加入暂存区。在执行pre-commit命令之后，如果没有错误，就会执行git commit命令

#### browserslist

browserslist字段用来告知支持哪些浏览器及版本。Babel、Autoprefixer 和其他工具会用到它，以将所需的 polyfill 和 fallback 添加到目标浏览器。
