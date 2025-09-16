# TrendRadar 飞书部署指南

## 🎯 完整部署步骤

### 第一步：创建飞书机器人

1. **打开飞书机器人构建页面**
   - 电脑浏览器打开：https://botbuilder.feishu.cn/home/my-app

2. **创建新机器人应用**
   - 点击"新建机器人应用"

3. **设置 Webhook 触发器**
   - 进入应用后：流程设计 > 创建流程 > 选择触发器
   - 选择"Webhook 触发"
   - **重要：复制 Webhook 地址保存**

4. **配置触发器参数**
   - 在参数框中粘贴以下内容：
   ```json
   {
     "message_type": "text",
     "content": {
       "total_titles": "{{内容}}",
       "timestamp": "{{内容}}",
       "report_type": "{{内容}}",
       "text": "{{内容}}"
     }
   }
   ```
   - 点击"完成"

5. **设置消息发送**
   - 选择操作 > 发送飞书消息
   - 勾选"群消息"
   - 选择目标群组（需要先在飞书创建群组）
   - 消息标题：`TrendRadar 热点监控`
   - 配置消息内容（按照飞书文档图片设置）

### 第二步：GitHub 部署

1. **Fork 项目**
   - 访问：https://github.com/sansan0/TrendRadar
   - 点击右上角 "Fork" 按钮

2. **配置 GitHub Secrets**
   - 在你 Fork 的仓库中：Settings > Secrets and variables > Actions
   - 点击 "New repository secret"
   - 添加：
     - 名称：`FEISHU_WEBHOOK_URL`
     - 值：第一步复制的 Webhook 地址

3. **启用 GitHub Actions**
   - 进入仓库的 Actions 页面
   - 如果提示启用，点击启用
   - 工作流会自动开始运行

### 第三步：配置自定义

1. **关键词配置** (可选)
   - 编辑 `config/frequency_words.txt` 文件
   - 添加你关心的关键词，每行一个
   - 支持词组分组（空行分隔）

2. **运行模式配置** (可选)
   - 编辑 `config/config.yaml` 文件
   - 修改 `report.mode` 设置：
     - `current`: 当前榜单模式（推荐）
     - `daily`: 当日汇总模式
     - `incremental`: 增量监控模式

### 第四步：测试部署

1. **手动触发测试**
   - 在 GitHub 仓库的 Actions 页面
   - 选择 "Hot News Crawler" 工作流
   - 点击 "Run workflow" 按钮

2. **查看结果**
   - 检查 Actions 运行日志
   - 查看飞书群组是否收到消息
   - 访问 GitHub Pages 页面查看网页版报告

## ⚙️ 自定义配置

### 运行时间设置
编辑 `.github/workflows/crawler.yml` 文件中的 cron 表达式：
```yaml
schedule:
  - cron: "0 * * * *"  # 每小时运行
  # 或者 "*/30 * * * *" # 每30分钟运行
```

### 关键词语法
- 普通词：直接写关键词
- 必须词：`+关键词` (必须包含)
- 过滤词：`!关键词` (排除包含此词的新闻)
- 词组：用空行分隔不同的词组

## 🔧 故障排查

### 常见问题
1. **飞书收不到消息**
   - 检查 Webhook URL 是否正确
   - 确认机器人配置完整
   - 查看 GitHub Actions 日志

2. **工作流运行失败**
   - 检查 config 文件格式
   - 确认 Secrets 配置正确
   - 查看错误日志

3. **消息内容为空**
   - 检查关键词配置
   - 确认有匹配的热点新闻

## 📱 使用技巧

1. **推送时间控制**
   - 可在 config.yaml 中设置静默推送时间段

2. **多平台同时推送**
   - 可同时配置多个通知平台的 webhook

3. **权重调整**
   - 根据需要调整热点排序算法权重

## 🎉 部署完成

恭喜！你的 TrendRadar 已经成功部署到飞书。系统会根据设定的时间自动运行，并将筛选后的热点新闻推送到你的飞书群组。

如有问题，请查看项目的 Issues 页面或文档。
