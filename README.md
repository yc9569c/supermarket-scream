# 🛒 超市尖叫 — Supermarket Scream

> 叫越大声，跑越快！一款声控购物车竞速游戏的网页版复刻。

**灵感来源：** Steam 上的《Supermarket Shriek》—— 一款靠"啊啊啊"尖叫控制购物车在超市里狂飙的声控竞速游戏。

## 🎮 玩法

1. 点屏幕 → 允许麦克风权限
2. **叫越大声，购物车跑越快**
3. 按住 ◀ / ▶ 控制方向
4. 躲避货架和障碍物 🚫
5. 收集金币 ⭐

## 🔬 原理

| 层级 | 技术实现 |
|------|---------|
| **音频捕获** | `navigator.mediaDevices.getUserMedia()` + Web Audio API |
| **音量分析** | `AnalyserNode(fftSize=256)` → `getByteTimeDomainData()` → RMS 振幅 |
| **物理引擎** | 音量 → 推力 → 加速度 → 速度 → 摩擦减速 |
| **碰撞检测** | AABB（轴对齐包围盒） |
| **渲染** | Canvas 2D + 摄像机跟随 |
| **适配** | 桌面 + iOS + Android，响应式布局 + 触摸控制 |

**纯前端，零依赖，一个 HTML 搞定。**

## 📱 访问

| 设备 | 说明 |
|------|------|
| 🖥️ 桌面 | 浏览器打开即可，键盘 A/D 或 ← → 转向 |
| 📱 手机 | 触摸按钮转向，需 HTTPS 才能用麦克风 |

> ⚠️ iOS Safari 需要 HTTPS 才能调用麦克风（安全上下文要求）。
> 部署时请使用有效证书，或自签证书首次访问点「显示详细信息→访问此网站」。

## 🚀 部署

```bash
# 只需要一个静态文件服务器
python3 -m http.server 8080
# 或
npx serve .
```

如需 HTTPS（iOS 需要）：

```bash
# 生成自签证书
openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
  -keyout server.key -out server.crt \
  -subj "/CN=你的IP"

# 用 Python 启动 HTTPS 服务
python3 -c "
import http.server, ssl, socketserver
d = socketserver.ThreadingTCPServer(('0.0.0.0', 443), http.server.SimpleHTTPRequestHandler)
d.socket = ssl.wrap_socket(d.socket, certfile='server.crt', keyfile='server.key', server_side=True)
d.serve_forever()
"
```

## 📄 文件结构

```
supermarket-scream/
├── index.html    ← 游戏本体（单文件）
├── README.md
└── .gitignore
```

## 🏗️ 技术栈

- Web Audio API（实时音频分析）
- Canvas 2D（游戏渲染）
- 无框架、零依赖

## 📜 License

[MIT](LICENSE)
