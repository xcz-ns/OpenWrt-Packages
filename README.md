# 上游仓库同步自动化（含子目录提取）

本项目使用 **GitHub Actions** 自动同步多个上游 OpenWrt 插件仓库，支持跨多个分支（如 `18.06`、`24.10`）自动提取子目录并更新至当前仓库，便于定制 OpenWrt 插件集成。

------

## ✨ 功能特点

✅ **支持多分支同步**（通过矩阵策略一次同步多个目标分支）

✅ **自动提取仓库中的子目录**（只保留需要的部分）

✅ **每日定时同步**（北京时间中午 12:00 自动运行）

✅ **支持手动触发或代码变更触发**

✅ **自动记录上游 commit，避免重复拉取**

✅ **增量更新，仅同步发生变化的仓库**

✅ **同步失败自动重试**

✅ **自动提交改动并推送**

✅ **自动清理旧的工作流运行记录**

------

## 🧩 使用方式

### 配置同步源

在 `.github/workflows/sync.yml` 中使用 matrix 配置各分支同步内容。

每个分支配置如下两部分：

#### 1. `repos`：需同步的仓库及分支（可省略分支）

格式为 JSON 格式的键值对，例：

```json
{
  "插件名": "仓库链接 分支（可选）"
}
```

示例：

```json
{
  "luci-theme-argon": "https://github.com/jerrykuku/luci-theme-argon 18.06",
  "luci-app-lucky": "https://github.com/gdy666/luci-app-lucky"
}
```

#### 2. `subdirs`：仅提取的子目录路径（可选）

同样为 JSON 格式，指定仓库中要提取的子目录，提取后将替换为根目录下的同名目录。

示例：

```json
{
  "luci-app-lucky": "luci-app-lucky",
  "luci-app-store": "luci-app-store",
  "unishare": "network/services/unishare"
}
```

------

## 📦 同步记录机制

自动记录每个仓库最新的 commit，避免重复拉取。首次运行时会强制拉取所有仓库，后续仅增量同步发生变化的部分。

------

## ⏱ 自动更新时间

通过 GitHub Actions 定时任务实现：

🕛 **每日北京时间中午 12:00 自动执行**

🖱 可在 Actions 页面中手动点击触发

📝 触发 `.github/workflows/sync.yml` 文件的修改也可启动流程

------

## 🚀 快速开始

1. Fork 本仓库
2. 编辑 `.github/workflows/sync.yml` 中 `matrix` 配置的仓库列表和提取路径
3. 推送修改，或等待自动定时触发
4. 所有变更将自动提交至对应分支

------

## 🧹 自动清理历史运行记录

每次同步完成后，会自动删除旧的 workflow 执行记录，仅保留最新记录，保持 Actions 页面整洁。
