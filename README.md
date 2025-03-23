# tglibrary

**tglibrary** 是一个使用Flask编写的基于 Telegram Bot API 的媒体文件存储解决方案，让您能够轻松上传、管理和分享图片和视频文件，并突破 Telegram 的 20MB 文件大小限制。
# 主页截图：
![主页截图](https://img.888888.fit/d/AgACAgUAAxkDAAMCZ96TkTSBUMoJAvENglfCT3pxg9QAAsnBMRuvEPlWYiEAAaQw3YhoAQADAgADdwADNgQ)
# 后台截图：
![后台截图](https://img.888888.fit/d/AgACAgUAAxkDAAMDZ96Tq54oaHlpwgS8nTw51rqud3MAAsrBMRuvEPlWBuSIQ2cdUusBAAMCAAN3AAM2BA)

## 当前所有版本包括打包版均不保证稳定性,建议使用docker版本,如有问题及时提交lssuse,我会及时修复！！！

前端部分借鉴[SkyDependence](https://github.com/SkyDependence/)大佬的前端，大佬的[tgDrive](https://github.com/SkyDependence/tgDrive)也是不错的项目，大家可以自行前往查看

## 特性

* 🖼️ 支持图片上传和存储
* 🎬 支持视频上传和存储
* 📊 突破 Telegram 20MB 文件大小限制
* 🔗 生成可直接访问的文件链接
* 📂 文件管理功能
* 🔒 安全的私人存储空间
* 💻 简单易用的命令接口

## 工作原理

tglibrary 利用 Telegram Bot API 作为后端存储系统，通过以下方式实现功能：

1. **文件分片上传** - 对于超过 20MB 的视频文件，tglibrary 会自动将其分割成多个TS进行上传，播放时调用m3u8进行播放
2. **元数据管理** - 为每个文件维护详细的元数据记录，便于快速检索
3. **直接链接生成** - 为所有上传的媒体文件生成直接可访问的链接

## 安装

下载存储库中的 tglibrary 二进制文件

## 配置

在使用 tglibrary 前，您需要创建一个 Telegram Bot 并获取 API Token。

1. 在 Telegram 中与 @BotFather 聊天创建新的 Bot
2. 获取 API Token
3. 在 Telegram 中与 @get_myidbot 聊天获取你的 CHAT_ID
4. 程序自动创建配置文件 `config.json`，请自行更改：

```json
{
    "TOKEN": "telegram_bot_token",
    "CHAT_ID": "chat_id",
    "PASSWORD": "admin",
    "PORT": 5000,
    "MAX_VIDEO_SIZE_MB": 20,
    "SEGMENT_SIZE_MB": 10
}
```
默认密码为：admin
## 使用方法

### Docker部署(推荐)
```bash
docker push sulong/tglibrary:0.0.1
docker run -d -p 5000:5000 --name tglibrary sulong/tglibrary:0.0.1
```
程序运行自动创建config.json 也可以自行复制config.json，修改完成后合并docker运行一起运行
```bash
docker run -d -p 5000:5000 --name tglibrary -v $(pwd)/config.json:/app/config.json sulong/tglibrary:0.0.1
```
Docker部署不是很熟悉，如果你遇到问题有相关的解决方案，欢迎及时lssuse，谢谢！
### 命令行使用

```bash
# 运行tglibrary
./tglibrary
```

### 作为系统服务运行

您可以将 tglibrary 设置为系统服务，使其在后台持续运行。以下是使用 systemd 配置的方法：

1. 创建服务文件：

```bash
sudo nano /etc/systemd/system/tglibrary.service
```

2. 添加以下内容：

```ini
[Unit]
Description=Telegram Library Media Storage
After=network.target

[Service]
Type=simple
User=YOUR_USERNAME
WorkingDirectory=/path/to/tglibrary
ExecStart=/path/to/tglibrary
Restart=always
RestartSec=5
StandardOutput=syslog
StandardError=syslog
SyslogIdentifier=tglibrary

[Install]
WantedBy=multi-user.target
```

3. 启用并启动服务：

```bash
sudo systemctl daemon-reload
sudo systemctl enable tglibrary
sudo systemctl start tglibrary
```

4. 检查服务状态：

```bash
sudo systemctl status tglibrary
```

### 常用服务管理命令

```bash
# 启动服务
sudo systemctl start tglibrary

# 停止服务
sudo systemctl stop tglibrary

# 重启服务
sudo systemctl restart tglibrary

# 查看日志
sudo journalctl -u tglibrary -f
```

## 注意事项

- 确保 tglibrary 具有执行权限：`chmod +x /path/to/tglibrary`
- 首次运行自动创建`config.json` 请根据你获取的 `API Token` 和 `CHAT_ID` 正确配置 `config.json`
- 系统服务运行时将使用配置文件中指定的存储路径

## 反代设置
Nginx
```bash
server {
    listen 80;
    server_name your-domain.com;
    
    # 设置最大上传文件大小为100MB
    client_max_body_size 100M;
    
    location / {
        proxy_pass http://localhost:5000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

## 支持与反馈
如果您觉得本项目对您有帮助，欢迎：

* ⭐ 给项目点个 Star
* 🔄 分享给更多的朋友
* 🐛 提交 Issue
* 您的支持是项目持续发展的动力！
