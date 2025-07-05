---
{"aliases":[],"tags":[],"title":"docker 笔记","date":"2025-06-28T21:49:46+08:00","date_modify":"2025-07-02T19:55:35+08:00","dg-publish":true,"permalink":"/Publish/01_技术/docker 笔记/","dgPassFrontmatter":true,"created":"2025-06-28T21:49:46+08:00","updated":"2025-07-02T19:55:35+08:00"}
---


# 1 Docker

## 1.1 多阶段构建

 你可以在一个 Dockerfile 中定义多个构建阶段。例如，一个阶段用来编译代码（包含所有开发依赖），另一个阶段只把编译好的产物复制到一个干净的基础镜像中。

## 1.2 CMD vs. ENTRYPOINT：容器的“使命”与“参数”

- 想象一下，你的容器是一个“命令行工具”。
- ENTRYPOINT : **定义了这个工具的“程序本身”** 。它是固定的，是容器的核心使命。比如，这个工具就是 git 。
- CMD : **定义了这个工具的“默认参数”** 。如果用户不指定任何参数，就用这个。比如， git 的默认参数是 --help 。
- cmd 被覆盖，entrypoint 被用来传参数用

```bash
# 怎么都会执行 `docker-entrypoint.sh`，你可以选择覆盖 cmd

ENTRYPOINT ["docker-entrypoint.sh"]
CMD ["redis-server"]
```

## 1.3 Volume vs. Bind Mount：数据的“公寓”与“别墅”

- 这两种都是将数据持久化到容器之外的方法，但管理方式和适用场景截然不同。
- Volume (数据卷) : 像是 Docker 为你管理的“精装公寓” 。你不用关心公寓在哪，只管拎包入住（存数据）。Docker 负责创建、管理和清理。
- Bind Mount (绑定挂载) : 像是你 已有的“私家别墅” 。你告诉 Docker：“把我的这个文件夹（别墅）直接给容器用”。你对这个文件夹有完全的控制权。
- 看冒号 (:) 左边的部分：
	- 如果它包含 /、./、../ (在 Linux/macOS) 或 \ (在 Windows)，那么它一定是 Bind Mount。 因为你在指定一个明确的、存在于宿主机文件系统上的路径。
	- 如果它只是一个简单的字符串，不包含任何路径分隔符，那么它一定是 Volume。 因为你只是在给 Docker 一个名字，让 Docker 自己去管理这个存储空间。

# 2. 其他

## 2.1 指定 IP 执行命令

`DOCKER_HOST` 是 Docker 的一个环境变量，用于指定 Docker 客户端要连接的 Docker 守护进程（daemon）的地址。

```bash
DOCKER_HOST=ssh://root@10.133.0.21 docker ps
```
