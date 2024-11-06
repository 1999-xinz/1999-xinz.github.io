## Lxinz 的博客

## 1-使用（创建文章）

Run `pnpm new-post <filename>` to create a new post and edit it in `src/content/posts/`.

## 2-文章头设置

```yaml
---
title: My First Blog Post
published: 2023-09-09
description: This is the first post of my new Astro blog.
image: /images/cover.jpg
tags: [Foo, Bar]
category: Front-end
draft: false
---
```

## 3-部署：（可能需要的指令）

git add .

git commit -m ""

git remote set-url origin https://github.com/1999-xinz/1999-xinz.github.io.git

git push origin master

## 4-常用pnpm指令操作

All commands are run from the root of the project, from a terminal:

| Command                                 | Action                                               |
| :-------------------------------------- | :--------------------------------------------------- |
| `pnpm install` AND `pnpm add sharp` | Installs dependencies                                |
| `pnpm dev`                            | Starts local dev server at `localhost:4321`        |
| `pnpm build`                          | Build your production site to `./dist/`            |
| `pnpm preview`                        | Preview your build locally, before deploying         |
| `pnpm new-post <filename>`            | Create a new post                                    |
| `pnpm astro ...`                      | Run CLI commands like `astro add`, `astro check` |
| `pnpm astro --help`                   | Get help using the Astro CLI                         |
