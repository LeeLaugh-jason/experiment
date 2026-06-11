# 移动端按钮点击实验 - Web 实验程序

## 文件结构

```
experiment/
├── index.html    ← 唯一需要打开的文件（所有代码都在里面）
└── README.md     ← 本文件
```

## 快速开始

### 方式一：GitHub Pages 部署（推荐）

```bash
# 1. 将 experiment/ 整个目录推送到 GitHub 仓库
# 2. 在仓库 Settings → Pages 中启用：
#    Source: Deploy from a branch
#    Branch: main (或 gh-pages), / (root)
# 3. 获得公网链接，如 https://你的用户名.github.io/实验仓库/
# 4. 用二维码生成工具将链接转为二维码
# 5. 被试扫码直接开始实验
```

### 方式二：本地局域网测试

```bash
# 在 experiment/ 目录下执行：
python3 -m http.server 8080

# 手机和电脑连同一 Wi-Fi
# 手机浏览器访问 http://电脑IP:8080
```

### 方式三：直接打开文件（离线）

用手机浏览器直接打开 `index.html` 即可。

> **注意**：部分浏览器通过 `file://` 协议打开时 IndexedDB 可能受限，建议优先使用方式一或二。

## 实验流程（被试视角）

```
1. 欢迎页面 → 勾选知情同意 → 点击"开始实验"
2. 填写被试信息（编号、年龄、性别、惯用手等）
   └─ 色盲/色弱会被排除
3. 练习阶段（2 次点击，熟悉操作）
4. 正式实验：
   ┌─ 共 9 组条件（随机顺序）
   ├─ 每组 10 次点击
   ├─ 每组结束后填写 NASA-TLX 问卷（6 题）
   └─ 组间有休息页显示进度
5. 完成页面 → 点击"导出 CSV 数据" → 文件下载
6. 将 CSV 文件发给实验人员
```

## 实验设计参数

| 参数 | 值 |
|------|-----|
| 按钮大小 | 40dp / 60dp / 80dp |
| 按钮间距 | 5dp / 15dp / 30dp |
| 设计类型 | 3×3 两因素被试内（每个被试完成全部 9 种条件） |
| 每条件试次 | 10 次（共 90 次） |
| 超时时间 | 8000ms |
| 预计总时长 | 15–20 分钟 |

## 目标与干扰颜色

- 目标按钮：红色 `#FF4444`
- 干扰按钮：灰色 `#CCCCCC`
- 命中反馈：绿色闪烁 80ms
- 误触反馈：红色闪烁 80ms

## 数据字段说明

导出的 CSV 包含两个数据表（用注释行分隔）：

### 试次数据 (trials)

| 字段 | 说明 |
|------|------|
| participantId | 被试编号 |
| conditionIndex | 条件编号 (0-8) |
| conditionOrderPosition | 被试执行该条件的顺序位置 (0-8) |
| isPractice | 是否为练习 |
| trialNumber | 条件内试次序号 |
| buttonSizeDp | 按钮大小 (CSS px) |
| buttonSpacingDp | 按钮间距 (CSS px) |
| gridRows | 网格行数 |
| gridCols | 网格列数 |
| totalButtons | 本次呈现的总按钮数 |
| targetPositionIdx | 目标按钮在网格中的索引 |
| hitPositionIdx | 实际点击的按钮索引 |
| isHit | 是否命中目标 |
| isError | 是否误触 |
| isTimeout | 是否超时 |
| responseTimeMs | 响应时间 (ms) |
| deviceTimestamp | 时间戳 |

### TLX 数据

| 字段 | 范围 |
|------|------|
| mentalDemand | 1-20 |
| physicalDemand | 1-20 |
| temporalDemand | 1-20 |
| performance | 1-20 |
| effort | 1-20 |
| frustration | 1-20 |

## 断点续传

被试如果在实验过程中不小心关闭了浏览器，重新打开页面并输入相同的被试编号后，系统会提示"是否继续未完成的实验"，选择"确定"即可从中断处继续。
