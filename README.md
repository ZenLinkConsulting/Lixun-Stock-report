# 立讯精密股票分析报告 - 自动化部署包

## 文件说明

- `stock_report.py` - 主程序，获取股票数据并发送报告
- `requirements.txt` - Python依赖
- `cron_job.sh` - Linux定时任务脚本
- `windows_task.xml` - Windows计划任务配置
- `README.md` - 部署说明

## 快速部署指南

### 方案1：云服务器部署（推荐）

**阿里云/腾讯云/EasyPanel等：**

1. 上传所有文件到服务器
2. 执行安装依赖：
```bash
pip install requests schedule
```

3. 设置定时任务（每天早上9点执行）：
```bash
crontab -e
# 添加下面这行：
0 9 * * * cd /path/to/stock-report && python stock_report.py >> /tmp/stock_report.log 2>&1
```

### 方案2：Windows任务计划程序

1. 安装Python 3.8+
2. 安装依赖：`pip install requests schedule`
3. 打开「任务计划程序」→ 创建基本任务
4. 设置触发器：每天 09:00
5. 操作：启动程序 → `python.exe` 参数填 `stock_report.py`

### 方案3：部署为Web API

如果需要更灵活的触发方式，可以部署为Flask API：

```bash
pip install flask requests schedule gunicorn
gunicorn -w 1 -b 0.0.0.0:5000 app:app
```

然后用任意定时服务（如cron、GitHub Actions）调用：
```bash
curl https://your-server.com/send-report
```

## 飞书Webhook配置

打开 `stock_report.py`，修改以下行：
```python
FEISHU_WEBHOOK = "https://open.feishu.cn/open-apis/bot/v2/hook/你的webhook地址"
```

## 测试运行

```bash
python stock_report.py
```

成功后会看到飞书收到报告。