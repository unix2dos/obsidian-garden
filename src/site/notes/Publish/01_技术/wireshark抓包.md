---
{"aliases":[],"tags":[],"title":"wireshark抓包","date":"2025-07-11T21:25:59+08:00","date_modify":"2025-07-12T12:11:35+08:00","dg-publish":true,"permalink":"/Publish/01_技术/wireshark抓包/","dgPassFrontmatter":true,"created":"2025-07-11T21:25:59+08:00","updated":"2025-07-12T12:11:35+08:00"}
---


# 1. 教程

## 1.1 你的第一次抓包

1. **打开 Wireshark**：启动后，你会看到一个网络接口列表。
2. **选择接口**：找到你正在使用的网络接口。通常是：
    - `Wi-Fi: en0` 或 `en1` (无线网络)
    - `Ethernet: enX` (有线网络)
    - `Thunderbolt Bridge` (雷电网桥)
    - 一个好的判断方法是看接口名字旁边有**实时流量图（像心电图一样跳动）**的那个，说明它正在收发数据。
3. **开始捕获**：双击你想监听的接口，或者选中后点击左上角的绿色鲨鱼鳍图标 (Start capturing packets)。
4. **观察数据**：你会看到屏幕上开始疯狂滚动数据包。这是你电脑上所有正在发生的网络通信。
5. **停止捕获**：点击红色的方块图标 (Stop capturing packets)。

## 1.2 显示过滤器

 **显示过滤器 (Display Filters)**: 在抓包**之后**使用，从已捕获的数据中筛选出你想看的内容。**这是日常使用中最频繁的功能**。
在界面上方绿色的 `Apply a display filter` 输入框中输入。

- **常用例子**:
    - 按 IP 地址过滤: `ip.addr == 8.8.8.8`
    - 按协议过滤: `dns`, `http`, `tcp`, `icmp` , `tls`
    - 按端口过滤: `tcp.port == 443` (HTTPS) 或 `udp.port == 53` (DNS)
    - 组合条件: `ip.addr == 192.168.1.101 && tcp.port == 80` (逻辑与)
    - 排除条件: `!(arp || dns)` (排除 ARP 和 DNS 协议)

## 1.3 追踪流 (Follow Stream)

想看一个完整的对话（比如一次完整的网页请求）？用追踪流！

1. 在数据包列表中，找到一个你感兴趣的协议的包（例如 HTTP 或 TCP）。
2. 右键点击该数据包。
3. 选择 `Follow` > `TCP Stream` (或 UDP/TLS/HTTP Stream)。
4. 一个新的窗口会弹出，将客户端和服务端之间的数据按对话顺序整理好，非常直观。红色是客户端发出的，蓝色是服务端返回的。

# 2. 抓包 https

## 2.1 密钥日志文件 (SSLKEYLOGFILE)

**第 1 步：配置 chrome 密钥**

```bash
# 创建一个空文件来存放密钥
touch ~/Desktop/sslkeylog.log

# 通过终端启动 Chrome 并设置环境变量
SSLKEYLOGFILE=~/Desktop/sslkeylog.log /Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome
```

**第 2 步：配置 Wireshark**

现在，告诉 Wireshark 去哪里找这个密钥文件。

1. 打开 Wireshark。
2. 进入设置菜单：点击屏幕左上角的 `Wireshark` -> `Settings...` (或 `Preferences...`)。
3. 在弹出的窗口中，展开左侧的 `Protocols` 列表。
4. 向下滚动找到并选中 `TLS` (旧版本可能叫 `SSL`)。
5. 在右侧，你会看到一个关键的选项：`(Pre)-Master-Secret log filename`。
6. 点击 `Browse...` 按钮，然后导航到你的用户主目录，选择刚刚创建的 `sslkeylog.log` 文件。
7. 点击 `OK` 保存设置。

**第 3 步：开始抓包和解密**

这是见证奇迹的时刻！

1. **完全退出** 你的浏览器（Chrome 或 Firefox）。**这一步至关重要**！必须是完全退出 (按 `Cmd + Q`)，而不是只关闭窗口。
2. **完全退出** Wireshark。
3. **重新启动 Wireshark**，并开始在你选择的网络接口上抓包。
4. **重新启动 Chrome 或 Firefox**，然后访问任何一个 HTTPS 网站，例如 `https://www.apple.com`。
5. 观察 Wireshark 的窗口！

**验证是否成功**：

- 在显示过滤器中输入 `http` 或 `http2`。
- 如果配置成功，你现在应该能看到原本被加密的 HTTPS 流量被解密成了 HTTP/2 或 HTTP/1.1 协议！你将能看到清晰的 `GET`, `POST` 请求。
- 点击一个数据包，在 "Packet Details" (数据包详情) 窗格中，除了 "Transport Layer Security" 这一层，下面会出现一个新的 "Decrypted TLS" 标签页，里面就是解密后的应用数据。

# 3. 设置

改时间格式：View-> Time Display Format -> Date and Time of Day （1970-01-01 01:02:03.123456）
