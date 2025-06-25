# 上游仓库同步自动化（含子目录提取）

本项目通过 GitHub Actions 自动同步多个上游开源仓库中的插件或主题，支持按需提取子目录并更新至本仓库根目录。适用于构建 OpenWrt 插件集或镜像插件管理。

## ✨ 功能特点

- ✅ 自动同步指定的 Git 仓库内容
- ✅ 支持只提取仓库中的某个子目录
- ✅ 每日定时更新（北京时间中午12点）
- ✅ 支持手动触发
- ✅ 自动记录上游提交，避免重复拉取
- ✅ 自动推送更新并清理旧的 workflow 运行记录

## 🧩 使用方式

### 配置同步仓库

在 `.github/workflows/sync.yml` 中配置仓库源：

```bash
declare -A repos=(
  ["插件名"]="仓库链接 分支（可省略）"
)
````

例如：

```bash
["luci-app-lucky"]="https://github.com/gdy666/luci-app-lucky main"
["luci-theme-argon"]="https://github.com/jerrykuku/luci-theme-argon 18.06"
```

### 配置需要提取子目录的仓库

如上游仓库是多项目结构，仅需其中一个子目录：

```bash
declare -A subdirs=(
  ["插件名"]="子目录路径"
)
```

例如：

```bash
["luci-app-lucky"]="luci-app-lucky"
["luci-app-store"]="luci-app-store"
```

这些子目录将被提取为根目录下的内容，其余全部忽略。

---

## 🕒 自动更新时间

通过 GitHub Actions 定时任务配置：

* **北京时间每日中午 12:00 自动运行**
* **可随时通过手动触发运行一次**

---

## 🚀 快速开始

1. Fork 本仓库
2. 编辑 `.github/workflows/sync.yml` 中的仓库列表和提取路径
3. 推送触发或等待定时任务执行
4. 所有同步内容将自动更新并提交
