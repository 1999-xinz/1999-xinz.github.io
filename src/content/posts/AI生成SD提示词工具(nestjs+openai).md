---
title: AI生成SD提示词工具(nestjs+openai)
published: 2024-10-25
description: 'SD提示词ai生成工具（后台服务），可以根据镜头画面描述，生成正向提示词和负向提示词。'
image: '/images/demoImg.jpg'
tags: [后端]
category: '后端'
draft: false
---
# AI生成SD提示词工具（nestjs+openai）

> 本篇文章主要是代码学习，代码中你可以学习到下面的内容：
> 1、openai npm包的使用
> 2、SD提示词喂养模版和生成逻辑
> 3、nextjs接口服务编写和部署方法

## 前沿

下面是对应github项目的README.md，

学习方式：
主要查看项目下src/chat目录下的控制器模块（src/chat/chat.controller.ts）和服务模块(src/chat/chat.service.ts)的代码即可，有问题评论区提问。

项目地址 (github)：
[https://github.com/1999-xinz/xinz-sd-ai-serve](https://github.com/1999-xinz/xinz-sd-ai-serve)
ps: 有帮助的话，可以给个stars，谢谢

项目描述：
SD提示词ai生成工具（后台服务），可以根据镜头画面描述，生成正向提示词和负向提示词。

技术栈说明：
Stable Diffusion - AI + openai(chatgpt 3.5) + Nestjs

## 1-使用

**1-使用需要在根目录创建.env文件，并写入OPENAI_API_KEY，写自己申请的openai的KEY；**

.env

```
OPENAI_API_KEY=申请的openai的KEY
```

如果请求openai需要代理地址的，需要在/src/chat/chat.service.ts下修改baseURL的参数。

chat.service.ts

```
...
    this.client = new OpenAI({
      apiKey: process.env.OPENAI_API_KEY,
      baseURL: 'openai代理地址'
    });
...
```

**2-启动**

本地启动项目，需要先安装node_modules依赖

```
yarn install
```

部署服务器的话看下面部署部分。

## 2-示例

调用POST接口:（ip和端口可以按照nestjs文档在main.ts中进行自定义修改，目前默认本地）

`http://localhost:3000/chat/askQuestion`

传递参数：

```
{
    "prompt": "一只可爱的黑白色小猫"
}
```

响应结果：

```
{
    "index": 0,
    "message": {
        "role": "assistant",
        "content": "**Prompt:**\n(best quality,4k,8k,highres,masterpiece:1.2),ultra-detailed,(realistic,photorealistic,photo-realistic:1.37),close-up shot of a cute black and white kitten,adorable round eyes,soft fur texture,playful expression,curious and alert posture,paw raised as if reaching for something,artistic photo style,vivid monochrome color tones,soft studio lighting\n\n**Negative Prompt:**\n**Negative Prompt:**nsfw,(low quality,normal quality,worst quality,jpeg artifacts),cropped,monochrome,lowres,low saturation,((watermark)),(white letters),skin spots,acnes,skin blemishes,age spot,mutated hands,mutated fingers,deformed,bad anatomy,disfigured,poorly drawn face,extra limb,ugly,poorly drawn hands,missing limb,floating limbs,disconnected limbs,out of focus,long neck,long body,extra fingers,fewer fingers,,(multi nipples),bad hands,signature,username,bad feet,blurry,bad body",
        "refusal": null
    },
    "logprobs": null,
    "finish_reason": "stop"
}
```

上面响应的结果中，content字段下的Prompt后面为正向提示词，Negative Prompt为负面提示词。

## 3-部署

**1-打包项目**

> 用自己的包管理处理打包即可，不一定用yarn。

```
yarn build
```

这会在项目的 `dist`目录中生成编译后的文件。

**2-服务器环境搭建**

确保服务器上存在nodejs和npm即可。

**3-上传代码到服务器的目录**

使用 `scp`、`rsync`或者通过FTP等方式将你的项目代码上传到服务器上的一个目录（例如 `/var/www/my-nest-app`）。

> 只需要上传除node_modules之外的其他文件。

**4-在服务器上安装依赖**

在服务器上进入项目目录，并安装**生产依赖**：

```
cd /var/www/my-nest-app
npm install --only=production
```

**5-进程管理工具管理项目启动**

可以使用nodemon或pm2来管理应用项目，建议pm2。

```
pm2 start dist/main.js --name my-nest-app
```

之后，就可以和本地一样正常使用接口服务了。
