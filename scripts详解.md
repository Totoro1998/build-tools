### 描述

package.json 文件的“scripts”属性支持许多内置脚本及其预设的生命周期事件以及任意脚本，这些都可以通过运行 npm run-script <stage> 或 npm run <stage> 来执行。具有匹配名称的前置和后置命令也将运行（例如 premyscript、myscript、postmyscript)

Scripts from dependencies can be run with `npm explore <pkg> -- npm run <stage>`

### 工作原理

**package.json中的bin字段**

`package.json`中的字段 [bin](https://link.juejin.cn/?target=https%3A%2F%2Fdocs.npmjs.com%2Ffiles%2Fpackage.json.html%23bin "https://docs.npmjs.com/files/package.json.html#bin") 表示的是一个**可执行文件到指定文件源的映射**。通过`npm bin`指令显示当前项目的`bin`目录的路径。例如在`@vue/cli`的`package.json`中

如果全局安装`@vue/cli`的话，**`@vue/cli`源文件会被安装在全局源文件安装目录（`/user/local/lib/node_modules`）下，而`npm`会在全局可执行`bin`文件安装目录（`/usr/local/bin`）下创建一个指向`/usr/local/lib/node_modules/@vue/cli/bin/vue.js`文件的名为`vue`的软链接**

如果局部安装`@vue/cli`的话，`npm`则会在本地项目`./node_modules/.bin`目录下创建一个指向`./node_moudles/@vue/cli/bin/vue.js`名为`vue`的软链接，这个时候需要在终端中输入`./node_modules/.bin/vue`来执行

> 软链接（符号链接）是一类特殊的可执行文件， 其包含有一条以绝对路径或者相对路径的形式指向其它文件或者目录的引用。在`bin`目录下执行`ll`指令可以查看具体的软链接指向。在对链接文件进行读或写操作的时候，系统会自动把该操作转换为对源文件的操作，但删除链接文件时，系统仅仅删除链接文件，而不删除源文件本身

**PATH环境变量**

在`terminal`中执行命令时，**命令会在`PATH`环境变量里包含的路径中去寻找相同名字的可执行文件**。局部安装的包只在`./node_modules/.bin`中注册了它们的可执行文件，不会被包含在`PATH`环境变量中，这个时候在`terminal`中输入命令将会报无法找到的错误。

**每当执行`npm run`时，会自动新建一个`Shell`，这个 `Shell`会将当前项目的`node_modules/.bin`的绝对路径加入到环境变量`PATH`中，执行结束后，再将环境变量`PATH`恢复原样**。

通过`npm run`可以在不添加路径前缀的情况下直接访问当前项目`node_modules/.bin`目录里面的可执行文件    

### Pre & Post Scripts

```
{
  "scripts": {
    "precompress": "{{ executes BEFORE the `compress` script }}",
    "compress": "{{ run command to compress files }}",
    "postcompress": "{{ executes AFTER `compress` script }}"
  }
}
```

### Life Cycle Scripts

有一些特殊的生命周期脚本仅在某些情况下发生。除了 pre<event>、post<event> 和 <event> 脚本之外，还会发生下列脚本 

`prepare`, `prepublishOnly`, `prepack`, `postpack`

**prepare**

- Runs any time before the package is packed, i.e. during `npm publish` and `npm pack`

- Runs BEFORE the package is packed

- Runs BEFORE the package is published

- Runs on local `npm install` without any arguments

- Run AFTER `prepublish`, but BEFORE `prepublishOnly`

- NOTE: If a package being installed through git contains a `prepare` script, its `dependencies` and `devDependencies` will be installed, and the prepare script will be run, before the package is packaged and installed.

- As of `npm@7` these scripts run in the background. To see the output, run with: `--foreground-scripts`.

**prepublishOnly**

- Runs BEFORE the package is prepared and packed, ONLY on `npm publish`.

**prepack**

- Runs BEFORE a tarball is packed (on "`npm pack`", "`npm publish`", and when installing a git dependencies).
- NOTE: "`npm run pack`" is NOT the same as "`npm pack`". "`npm run pack`" is an arbitrary user defined script name, where as, "`npm pack`" is a CLI defined command

**postpack**

- Runs AFTER the tarball has been generated but before it is moved to its final destination (if at all, publish does not save the tarball locally)

**案例**

If you need to perform operations on your package before it is used, in a way that is not dependent on the operating system or architecture of the target system, use a `prepublish` script.This includes tasks such as:

- Compiling CoffeeScript source code into JavaScript.
- Creating minified versions of JavaScript source code.
- Fetching remote resources that your package will use.

The advantage of doing these things at `prepublish` time is that they can be done once, in a single place, thus reducing 复杂性和可变性

- You can depend on `coffee-script` as a `devDependency`, and thus your users don't need to have it installed.
- You don't need to include minifiers in your package, reducing the size for your users.
- You don't need to rely on your users having `curl` or `wget` or other system tools on the target machines.

### 环境

Package scripts在一个环境中运行，在该环境中提供了许多关于 npm 设置和进程当前状态的信息

#### path

If you depend on modules that define executable scripts, like test suites, then those executables will be added to the `PATH` for executing the scripts. So, if your package.json has this:

```
{
  "name" : "foo",
  "dependencies" : {
    "bar" : "0.1.x"
  },
  "scripts": {
    "start" : "bar ./test"
  }
}
```

then you could run `npm start` to execute the `bar` script, which is exported into the `node_modules/.bin` directory on `npm install`

#### package.json vars

package.json 字段被添加到 npm_package_ 前缀上。So, for instance, if you had `{"name":"foo", "version":"1.2.5"}` in your package.json file, then your package scripts would have the `npm_package_name` environment variable set to "foo", and the `npm_package_version` set to "1.2.5". You can access these variables in your code with `process.env.npm_package_name` and `process.env.npm_package_version`, and so on for other fields.

#### current lifecycle event

最后，将 npm_lifecycle_event 环境变量设置为正在执行的循环阶段。因此，您可以将单个脚本用于流程的不同部分，该脚本根据当前发生的情况进行切换

Objects are flattened following this format, so if you had `{"scripts":{"install":"foo.js"}}` in your package.json, then you'd see this in the script:

```
process.env.npm_package_scripts_install === "foo.js"
```

### 用法指南

#### 传入参数

**除了第一个可执行的命令，以空格分割的任何字符串（除了一些shell的语法）都是参数，并且都能通过`process.argv`属性访问**

`process.argv`属性返回一个数组，这个数组包含了启动`node`进程时的命令行参数。第一个元素为启动`node` 进程的可执行文件的绝对路径名[process.execPath](https://link.juejin.cn/?target=http%3A%2F%2Fnodejs.cn%2Fapi%2Fprocess.html%23process_process_execpath "http://nodejs.cn/api/process.html#process_process_execpath")，第二个元素为当前执行的JavaScript文件路径。剩余的元素为其他命令行参数

参数解析工具 minimist和yargs

#### 多命令运行

**串行执行**

串行执行，要求前一个任务执行成功以后才能执行下一个任务，使用`&&`符号来连接

**并行执行**

并行执行，就是多个命令可以同时的平行执行，使用`&`符号来连接。

#### env环境变量

1、其中`package.json`中的所有字段，都会被设置为以`npm_package_` 开头的环境变量

可以通过`process.env.npm_package_repository_type`获取到嵌套属性`type`的值`git`

2、同时，`npm`相关的所有配置也会被设置为以`npm_config_`开头的环境变量。

此外，还会设置一个比较特殊的环境变量`npm_lifecycle_event`，表示正在运行的脚本名称。比如执行`npm run serve` 的时候，`process.env.npm_lifecycle_event`值为`serve`，通过判断这个变量，可以将一个脚本使用在不同的`npm scripts`中

3、这些环境变量只能在`npm run`的脚本执行环境内拿到，正常执行的`node`脚本是获取不到的。所以，不能直接通过`env $npm_package_name`的形式访问，但可以在`scripts`中定义脚本`"scripts": {"bundle": "echo $npm_package_name"}`来访问。

#### 指令钩子
