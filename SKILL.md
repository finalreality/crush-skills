---
name: git-release
description: 发布版本前补充ChangeLog和Readme
license: Complete terms in LICENSE.txt
---

# Git Release Skill

## 功能描述

标准化的Git版本发布流程，包括：
1. 更新 CHANGELOG.md 版本更新说明
2. 更新 README.md 版本信息
3. 提交所有更改
4. 创建 Git tag
5. 推送到远程仓库

## 触发条件

当用户要求发布新版本时自动激活。

## 使用方法

### 基本发布

用户说"发布新版本"或"打tag"时，执行完整发布流程：

1. 首先读取当前 CHANGELOG.md 和 README.md 了解现有内容
2. 根据本次更新内容，更新两个文件的版本信息
3. 提交更改并创建 tag
4. 自动检查并推送到远程

### 版本号规则

- 主版本号.次版本号.修订号 (如 v1.0.0, v1.1.0, v2.0.0)
- 主版本号：重大功能变更或不兼容的API更改
- 次版本号：新功能向后兼容
- 修订号：Bug修复和小的改进

## 发布流程

### 1. 更新 CHANGELOG.md

在文件顶部添加新版本块：

```markdown
## v{x.y.z} ({日期})

### 主要更新

#### 新增功能
- 功能1
- 功能2

#### 修复问题
- 问题1

#### 技术改进
- 改进1

### 文件变更

#### 新增文件
- 文件路径 - 说明

#### 修改文件
- 文件路径 - 说明
```

### 2. 更新 README.md

- 更新版本号（如果显示）
- 更新默认账户信息（如有变更）
- 更新功能特性列表

### 3. 提交并创建 Tag

```bash
git add -A
git commit -m "chore: 准备发布 v{x.y.z}

- 更新 CHANGELOG.md
- 更新 README.md
"

git tag -a v{x.y.z} -m "v{x.y.z} 版本说明
主要功能：
- 功能1
- 功能2

💘 Generated with Crush

Assisted-by: MINIMAX-2.7 via Crush <crush@charm.land>
"
```

### 4. 推送到远程

发布完成后自动检查并执行推送：

1. **检查远程跟踪分支**：
   ```bash
   git rev-parse --abbrev-ref HEAD  # 获取当前分支名
   git config --get branch.{分支名}.remote  # 检查是否有远程跟踪
   ```

2. **如果有远程跟踪分支**：
   自动推送到远程（代码和tag）：
   ```bash
   git push origin {分支名}
   git push origin v{x.y.z}
   ```

3. **如果没有远程跟踪分支**：
   提示用户：
   > 检测到当前分支没有设置远程跟踪分支。请输入远程仓库地址（或直接回车跳过）：
   > 例如：git@github.com:user/repo.git 或 https://github.com/user/repo.git

   - **用户输入地址**：
     ```bash
     # 添加名为 origin 的远程仓库
     git remote add origin {用户输入的地址}
     # 推送到远程
     git push -u origin {分支名}
     git push origin v{x.y.z}
     ```

   - **用户直接回车（跳过）**：
     显示：`已跳过推送步骤，如需推送请手动执行：git push origin {分支名} && git push origin v{x.y.z}`

## 输出示例

有远程跟踪分支时：
```
已创建 v1.0.0 tag

版本更新：
- 新增多语言支持
- 新增大数据看板
- 新增测评报告管理
- 完善用户管理功能

提交: abc1234
Tag: v1.0.0

正在推送到远程...
已推送到 origin/{分支名}
已推送 tag v1.0.0
```

无远程跟踪分支时：
```
已创建 v1.0.0 tag

版本更新：
...

提交: abc1234
Tag: v1.0.0

⚠️ 当前分支没有远程跟踪分支
请输入远程仓库地址（或直接回车跳过）:
> git@github.com:user/repo.git

正在添加远程仓库...
已添加远程仓库 origin
正在推送到远程...
已推送到 origin/{分支名}
已推送 tag v1.0.0
```

## 注意事项

1. 确保所有更改已提交后再创建 tag
2. Tag 消息应简洁说明主要功能
3. CHANGELOG.md 中的日期使用 YYYY-MM-DD 格式
4. 更新 README.md 时保持格式一致
5. 推送前检查远程跟踪，避免推送到错误的仓库