# Cocos Creator H5客户端自动下载工具 使用说明

## 工具概述

这是一个专为Cocos Creator H5游戏客户端设计的资源自动下载工具，支持批量下载、断点续传、多线程下载等功能。

## 快速开始

### 1. 启动工具

```bash
python start_downloader.py
```

启动后会自动检测配置文件，并提供两种运行模式：
- **图形界面模式**（推荐）：提供友好的GUI界面
- **命令行模式**：适合批量处理和自动化脚本

### 2. 配置文件

工具需要 `config.bae53.json` 配置文件，该文件包含：
- 资源路径映射
- UUID版本信息
- 资源类型定义

### 3. 服务器设置

在图形界面中设置资源服务器URL，例如：
```
http://your-game-server.com/assets/
https://cdn.your-game.com/resources/
```

## 功能特性

### ✅ 支持的资源类型

| 资源类型 | 文件扩展名 | 说明 |
|---------|-----------|------|
| cc.SpriteFrame | .png | 精灵帧图片 |
| cc.Texture2D | .png | 纹理图片 |
| cc.BufferAsset | .bin | 二进制资源 |
| cc.Asset | .bin | 通用资源 |
| cc.AnimationClip | .bin | 动画剪辑 |
| cc.Prefab | .bin | 预制体 |
| cc.AudioClip | .mp3 | 音频文件 |
| cc.JsonAsset | .json | JSON数据 |
| cc.TextAsset | .txt | 文本资源 |
| sp.SkeletonData | .bin | Spine骨骼数据 |
| dragonBones.DragonBonesAsset | .bin | DragonBones资源 |
| dragonBones.DragonBonesAtlasAsset | .json | DragonBones图集 |

### 🚀 核心功能

1. **批量下载**：一次性下载所有游戏资源
2. **断点续传**：支持暂停和恢复下载
3. **多线程下载**：可配置并发数量，提高下载速度
4. **实时进度**：显示下载进度、速度和剩余时间
5. **文件验证**：自动验证文件完整性
6. **灵活配置**：支持多服务器配置和切换
7. **失败地址记录**：自动记录下载失败的文件和URL地址
8. **失败列表导出**：支持导出CSV格式的失败下载报告

## 使用方法

### 图形界面模式

1. **选择配置文件**：点击"浏览"按钮选择 `config.bae53.json`
2. **设置服务器URL**：输入资源服务器地址
3. **选择下载目录**：设置资源保存位置
4. **配置下载参数**：
   - 并发数量（建议5-10）
   - 超时时间（默认30秒）
   - 重试次数（默认3次）
5. **开始下载**：点击"开始下载"按钮
6. **导出失败列表**：下载完成后，如有失败文件可点击"导出失败列表"按钮

### 命令行模式

```bash
# 基本用法
python batch_downloader.py --config config.bae53.json --download-dir ./downloads

# 高级用法
python batch_downloader.py \
  --config config.bae53.json \
  --download-dir ./downloads \
  --server-url https://cdn.example.com/assets/ \
  --max-workers 10 \
  --timeout 60 \
  --max-retries 5 \
  --file-types native,import \
  --failed-list failed_downloads.csv
```

#### 命令行参数说明

| 参数 | 说明 | 默认值 |
|------|------|--------|
| `--config` | 配置文件路径 | 必需 |
| `--download-dir` | 下载目录 | `./downloads` |
| `--server-url` | 服务器URL | 从配置读取 |
| `--max-workers` | 并发数量 | 5 |
| `--timeout` | 超时时间(秒) | 30 |
| `--max-retries` | 重试次数 | 3 |
| `--file-types` | 文件类型 | `native,import` |
| `--failed-list` | 失败列表文件名 | 自动生成 |
| `--resume` | 启用断点续传 | 默认启用 |
| `--verify` | 验证文件完整性 | 默认启用 |

## 失败地址记录功能

### 功能概述

工具会自动记录所有下载失败的文件信息，包括：
- 失败的文件名和路径
- 文件类型和UUID
- 完整的下载URL地址
- 详细的错误信息
- 失败时间戳

### 使用方法

#### 图形界面模式

1. **自动记录**：下载过程中自动收集失败信息
2. **实时统计**：界面显示失败文件数量
3. **导出功能**：下载完成后点击"导出失败列表"按钮
4. **自动提示**：如有失败文件会自动提示导出

#### 命令行模式

```bash
# 指定失败列表文件名
python batch_downloader.py config.bae53.json --failed-list my_failed.csv

# 使用默认文件名（自动生成）
python batch_downloader.py config.bae53.json
```

### 失败列表文件格式

生成的CSV文件包含以下列：

| 列名 | 说明 | 示例 |
|------|------|------|
| 文件名 | 下载失败的文件名 | `abcd1234.v1.0.png` |
| 路径名 | 文件在游戏中的路径 | `textures/ui/button.png` |
| 类型 | 文件类型 | `native` 或 `import` |
| UUID | 资源的唯一标识符 | `abcd1234` |
| 错误信息 | 详细的错误描述 | `Connection timeout` |
| URL | 完整的下载地址 | `http://cdn.game.com/native/ab/abcd1234.v1.0.png` |

### 实际应用场景

1. **网络问题排查**：通过失败URL分析网络连接问题
2. **服务器状态监控**：识别特定服务器的问题
3. **资源完整性检查**：确保所有必需资源都已下载
4. **批量重试下载**：基于失败列表进行针对性重试
5. **问题报告**：向技术支持提供详细的失败信息

### 演示脚本

运行演示脚本查看失败记录功能：

```bash
python demo_failed_tracking.py
```

该脚本会展示：
- 如何配置和使用失败记录功能
- 失败信息的收集和存储
- 失败列表的生成和导出
- 实际的CSV文件格式示例

## 高级功能

### 服务器配置管理

```python
from download_config_manager import DownloadConfigManager

# 创建配置管理器
config_manager = DownloadConfigManager("server_config.json")

# 添加服务器
config_manager.add_server("cdn1", "主CDN", "https://cdn1.example.com/assets/")
config_manager.add_server("cdn2", "备用CDN", "https://cdn2.example.com/assets/")

# 设置默认服务器
config_manager.set_default_server("cdn1")

# 测试服务器连接
result = config_manager.test_server_connection("cdn1")
print(f"服务器连接状态: {result}")
```

### 自定义下载脚本

```python
from batch_downloader import BatchDownloader

# 创建下载器
downloader = BatchDownloader(
    config_path="config.bae53.json",
    download_dir="./my_downloads",
    max_workers=8
)

# 生成下载任务
downloader.generate_download_tasks()

# 开始下载
downloader.start_download()

# 获取下载统计
stats = downloader.get_download_stats()
print(f"下载完成: {stats['completed_files']}/{stats['total_files']}")
```

## 故障排除

### 常见问题

1. **配置文件不存在**
   - 确保 `config.bae53.json` 文件在正确位置
   - 检查文件权限和格式

2. **服务器连接失败**
   - 检查网络连接
   - 验证服务器URL是否正确
   - 尝试使用不同的服务器

3. **下载速度慢**
   - 适当增加并发数量
   - 检查网络带宽
   - 尝试使用更近的CDN服务器

4. **文件下载失败**
   - 检查磁盘空间
   - 验证文件权限
   - 查看错误日志

### 日志文件

工具会生成详细的日志文件：
- `download.log`：下载过程日志
- `error.log`：错误信息日志
- `download_report.json`：下载报告

### 性能优化建议

1. **并发设置**：根据网络条件调整，通常5-10个并发较为合适
2. **超时设置**：网络较差时可适当增加超时时间
3. **磁盘空间**：确保有足够的磁盘空间存储资源
4. **网络环境**：使用稳定的网络连接

## 技术支持

如果遇到问题，请：
1. 查看日志文件获取详细错误信息
2. 运行测试脚本验证组件功能：`python test_downloader.py`
3. 检查配置文件格式和内容
4. 确认服务器URL和网络连接

## 更新日志

### v1.0.0
- 初始版本发布
- 支持图形界面和命令行模式
- 实现批量下载和断点续传
- 添加多线程下载支持
- 集成服务器配置管理

---

**注意**：使用本工具时请确保遵守相关的服务条款和版权规定。
