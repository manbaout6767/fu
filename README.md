# 🌍 Country Weather CLI

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![Data Source](https://img.shields.io/badge/data%20source-Open--Meteo-brightgreen)](https://open-meteo.com/)
[![No API Key](https://img.shields.io/badge/API%20Key-not%20required-success)](https://open-meteo.com/)

一个轻量级的命令行天气查询工具，**支持查询全球各国城市的实时天气**。无需注册账号、无需申请 API Key，安装依赖后即可直接运行。

---

## ✨ 特性

- 🆓 **完全免费** —— 基于 [Open-Meteo](https://open-meteo.com/)，无需 API Key
- 🌐 **全球覆盖** —— 支持任意国家/地区的城市查询
- 🎯 **国家消歧** —— 同名城市可通过 `--country` 精确筛选（如区分法国 Paris 与美国 Paris）
- 🌡 **单位切换** —— 摄氏 / 华氏一键切换
- 🖱 **双击可用** —— 附带桌面启动脚本，无需记命令
- 📦 **零配置** —— `pip install` 后立即可用
- 🧩 **可作库调用** —— 核心逻辑与 CLI 分离，方便集成到其他项目
- ⚖️ **MIT 开源** —— 自由使用、修改、分发

---

## 📦 安装

### 1. 克隆仓库

```bash
git clone https://github.com/your-username/country-weather-cli.git
cd country-weather-cli
```

### 2. 安装依赖

```bash
pip install -r requirements.txt
```

> 如果系统提示找不到 `pip`，可改用：
> - Windows：`py -m pip install -r requirements.txt`
> - macOS / Linux：`python3 -m pip install -r requirements.txt`

---

## 🚀 快速开始

### 基本用法

```bash
# 查询东京天气（默认摄氏度）
python cli.py "Tokyo"

# 指定国家，避免歧义（例如区分美国 Paris 和法国 Paris）
python cli.py "Paris" --country FR

# 使用华氏度
python cli.py "New York" --country US --unit fahrenheit
```

> macOS / Linux 用户如果 `python` 命令不可用，请把上面的 `python` 换成 `python3`。

### 示例输出

```text
$ python cli.py "Paris" --country FR
📍 Paris, Île-de-France, France (FR)
🌡  温度：14.2°C
💨 风速：18.5 km/h
🧭 风向：230°
☁️ 天气代码：3
🕒 观测时间：2026-10-29T12:00
```

---

## 🖱 桌面启动（双击即用）

项目附带启动脚本，放在桌面上双击即可查询，无需打开终端敲命令。

### Windows

双击 `启动.bat`，按提示输入城市名和国家代码即可。

### macOS

双击 `启动.command`。若首次打开被系统拦截，请**右键 → 打开**，或在「系统设置 → 隐私与安全性」中允许运行。

### Linux

给脚本添加执行权限后双击运行（部分桌面环境需在文件属性中勾选「允许作为程序执行」）：

```bash
chmod +x 启动.sh
```

---

## 🧩 作为 Python 库使用

核心逻辑与命令行完全分离，可直接导入到自己的项目：

```python
from weather import query_weather

location, weather = query_weather("Tokyo", country="JP", unit="celsius")

if location and weather:
    print(f"{location['name']}, {location['country']}")
    print(f"温度：{weather['temperature']}°C")
    print(f"风速：{weather['windspeed']} km/h")
```

### 可用函数

| 函数 | 说明 |
|---|---|
| `geocode_city(city, country=None)` | 将城市名解析为坐标、国家等信息 |
| `fetch_weather(lat, lon, unit="celsius")` | 根据经纬度获取当前天气 |
| `query_weather(city, country=None, unit="celsius")` | 一站式查询，返回 `(location, weather)` |

---

## 🗂 项目结构

```text
country-weather-cli/
├── README.md
├── LICENSE
├── requirements.txt
├── weather.py          # 核心逻辑：地理编码 + 天气查询
├── cli.py              # 命令行接口
├── 启动.bat            # Windows 双击启动脚本
└── 启动.command        # macOS / Linux 双击启动脚本
```

---

## 🔌 数据源

本项目使用两个 **完全免费、无需认证** 的 Open-Meteo 端点：

| 用途 | 端点 |
|---|---|
| 城市名 → 坐标 | `https://geocoding-api.open-meteo.com/v1/search` |
| 坐标 → 当前天气 | `https://api.open-meteo.com/v1/forecast` |

Open-Meteo 对合理使用没有硬性频率限制，个人项目完全足够。
更多信息请访问 [Open-Meteo 官方文档](https://open-meteo.com/en/docs)。

---

## ❓ 常见问题

| 问题 | 解决方法 |
|---|---|
| `python` 不是内部或外部命令 | 改用 `py cli.py`（Windows）或 `python3 cli.py`（macOS / Linux） |
| `ModuleNotFoundError: No module named 'requests'` | 重新执行 `pip install -r requirements.txt` |
| 中文 / emoji 显示乱码（Windows CMD） | 启动脚本已包含 `chcp 65001`；如在终端手动运行，可先执行 `chcp 65001` |
| 找不到城市 | 城市名建议用英文（如 `Tokyo`、`München`），并配合 `--country` 缩小范围 |
| 天气数据获取失败 | 检查网络能否访问 `https://api.open-meteo.com`，必要时配置代理 |
| macOS 双击脚本无反应 | 右键 → 打开；或在终端执行 `chmod +x 启动.command` |

---

## 🛠 进阶：打包成可执行文件

如果希望分发给没有 Python 环境的用户，可以用 PyInstaller 打包：

```bash
pip install pyinstaller
pyinstaller --onefile --name weather cli.py
```

生成的 `dist/weather.exe`（Windows）或 `dist/weather`（macOS / Linux）可直接双击或命令行运行。

---

## 🤝 贡献

欢迎提交 Issue 或 Pull Request。

- 提交代码前请保持现有代码风格
- 新功能请同步补充文档
- 如涉及接口变更，请说明兼容性影响

---

## 📄 许可证

本项目采用 [MIT License](LICENSE) 开源。

---

## ⭐ 支持

如果这个项目对你有帮助，欢迎点一个 Star ⭐
