---
title: IP可用端口扫描器工具（bun + typescript）
published: 2024-11-06
description: '通过bun搭建nodejs项目，目的在于扫描检测指定IP下哪些端口可用'
image: '/images/demoImg.jpg'
tags: [后端]
category: '后端'
draft: false
---
## IP可用端口扫描器工具（bun + typescript）

```
学习方式：源码学习。

通过项目和源码可以学习到如下内容：

1、bun搭建项目，打包项目

2、net、dns等node内置模块的使用

3、yargs、assert、progress、cli-color等三方包的使用

ps：内置模块查询nodejs官方API介绍；三方模块查询NPM官网对应包信息。

nodejs官方API地址: [https://nodejs.org/docs/latest/api/](https://nodejs.org/docs/latest/api/)

NPM官网地址：[https://www.npmjs.com/](https://www.npmjs.com/)
```

## 前言

1、项目介绍：

项目通过bun搭建nodejs项目，目的在于扫描检测指定IP下哪些端口可用。项目使用net、dns实现ip端口的连接检测和域名解析端口的方法；使用了yargs实现用户终端输入参数；使用了assert、cli-color实现了终端错误信息提示和终端好看的输出内容颜色以及使用了progress美化终端输出扫描进度。

2、核心技术栈：

bun、net、yargs、typescript。

3、项目源码地址()github)：

[https://github.com/1999-xinz/xinz-port-scanner.git](https://github.com/1999-xinz/xinz-port-scanner.git)

ps: 有帮助的话，可以给个stars，谢谢

## 1-项目运行

**1、执行下面指令，运行项目**

`bun start scan <ip> <portrange>`

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/980c6fb437ba4a12b1f02a18830e8e2a.png#pic_center)

> 1、start是package.json中script的配置。
>
> 2、如果没有bun的需要进行全局安装。
>
> macOS安装方式：
>
> ```
> curl -fsSL https://bun.sh/install | bash
> ```
>
> 建议查看bun安装介绍，地址：[https://bun.sh/docs/installation](https://bun.sh/docs/installation)

## 2-核心源码说明

源码在根目录src下index.ts中，在源码中优先找到最下面的main入口主函数，从主函数的代码逐步理解功能实现。

main主函数

```
/**
 * 入口主函数
 */
async function main() {
  let host = ip;
  
  // 判断是否是域名，如果是则需要进行域名解析
  if (!isIpCheck(host)) {
    host = await resolveDomain(host);
  }

  // 断言校验
  assert(isIpCheck(host), `IP地址不合理: ${ip}`)
  assert(isPortRangeCheck(portrange), `端口范围不合理: ${portrange}`)

  let ports = portrange.split('-').map(Number); // 转换为数字数组
  await scanPorts(host, ports[0], ports[1]);
}
// 执行，执行完成后终止进程任务
main()
  .then(() => process.exit(0)) // 成功退出
  .catch((err) => { // 如果失败，输出错误并退出码为 1
    console.error(err);
    process.exit(1); // 失败退出
  })
```

上面主函数执行时，给主函数使用了async，之后确保main函数执行完成后，终止当前任务进程执行process.exit(0)，如果出错，就捕获且打印错误，并退出进程process.exit(1)。

需要注意的是，在主函数执行之前，执行了yargs方法，用于获取终端用户输入的ip和端口范围的值，之后存储到全局变量ip和portrange，之后在main函数中一开始进行获取和后续处理。

yargs方法

```js
/** 
 * yargs 命令行参数配置初始化
 */
let ip: string;
let portrange: string;
yargs(
  hideBin(process.argv))
    .command('scan <ip> <portrange>', 'scan the given IP and port range', 
    (yargs) => {
      return yargs
        // 默认扫描本机
        .positional('ip', {
          describe: '扫描ip地址(支持输入域名)',
          type: 'string',
          default: '127.0.0.1'
        })
        // 默认扫描所有端口
        .positional('portrange', {
          describe: '扫描端口范围(端口范围：1-65535 或者指定某个端口：2000-2000)',
          type: 'string',
          default: '1-65535'
        })
    }, 
    (argv) => {
      // 调用main函数, 传递参数
      ip = argv.ip;
      portrange = argv.portrange;
    })
    .demandCommand(1)
    .help()  // 添加帮助信息
    .alias('h', 'help')  // 添加帮助信息
    .parse()
```

其它方法模块，可以根据执行函数模块的注释进行理解即可。

## 3-项目打包

**1、执行下面指令，进行项目打包**

`bun run build`

之后，会在项目根目录构建build目录，目录下就是打包后的入口文件。

打包完成后，还需要将package.json和node_modules复制过去，这样才是一个完整的生产环境包。

**2、运行build下的入口文件方式**

可以使用node或者bun进行运行，一般服务器上安装node即可，那么默认使用node（版本需要大于16.x.x）吧，操作如下：

`node index.js scan <ip> <portrange>`

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/4f26bc1d6d3149d1ac90fe1960babffe.png#pic_center)
