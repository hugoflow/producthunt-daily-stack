# 跨仓库 GitHub Pages 部署设置指南

## 仓库架构

- **源码仓库**：当前仓库（存放 Hugo 源码和内容）
- **部署仓库**：`AIBriefs/AIBriefs.github.io`（GitHub Pages 网站）
- **网站地址**：`https://AIBriefs.github.io`

## 配置步骤

### 1. 创建 Personal Access Token (PAT)

1. **快速生成 Token**：
   - 🔗 **直接访问**：[GitHub Personal Access Tokens 页面](https://github.com/settings/tokens)
   - 或者手动导航：GitHub Settings > Developer settings > Personal access tokens > Tokens (classic)
   - 点击 "Generate new token (classic)"

2. **配置 Token**：
   - **Token 名称**：`Hugo Deploy Token` 或 `AIBriefs Deploy`
   - **过期时间**：建议选择 `No expiration` 或较长时间（如 1 年）
   - **权限选择**：
     - ✅ `repo` (完整仓库访问权限) - **必需**
     - ✅ `workflow` (触发工作流) - 可选但建议勾选

3. **保存 Token**：
   - 点击 "Generate token"
   - ⚠️ **重要**：立即复制生成的 token（只显示一次）
   - 妥善保存，稍后配置 Secret 时需要用到

### 2. 配置源码仓库的 Secrets

1. **快速添加 Secret**：
   - 🔗 **直接访问当前仓库 Secrets 页面**：[Repository Secrets](../../settings/secrets/actions)
   - 或者手动导航：当前仓库 Settings > Secrets and variables > Actions
   - 点击 "New repository secret"

2. **配置 Secret**：
   - **Name**: `DEPLOY_TOKEN`
   - **Value**: 粘贴步骤 1 中生成的 Personal Access Token
   - 点击 "Add secret"

3. **验证配置**：
   - 确认 Secret 列表中显示 `DEPLOY_TOKEN`
   - Secret 值会被隐藏显示为 `***`

### 3. 确保目标仓库存在

1. **创建 AIBriefs.github.io 仓库**（如果还没有）：
   - 🔗 **快速创建**：[创建新仓库](https://github.com/new)
   - **仓库名**：必须是 `AIBriefs.github.io`
   - **可见性**：选择 Public（GitHub Pages 免费版要求）
   - 可以是空仓库，工作流会自动推送内容

### 4. 配置目标仓库的 GitHub Pages

1. **配置 GitHub Pages**：
   - 🔗 **直接访问 AIBriefs.github.io Pages 设置**：[https://github.com/AIBriefs/AIBriefs.github.io/settings/pages](https://github.com/AIBriefs/AIBriefs.github.io/settings/pages)
   - 或者手动导航：AIBriefs.github.io 仓库 Settings > Pages
   - **Source** 选择 "Deploy from a branch"
   - **Branch** 选择 "main"（或 "master"，取决于工作流配置）
   - 点击 "Save"

## 快速链接汇总

- 🔗 [生成 Personal Access Token](https://github.com/settings/tokens)
- 🔗 [配置当前仓库 Secrets](../../settings/secrets/actions)
- 🔗 [创建新仓库](https://github.com/new)
- 🔗 [AIBriefs.github.io Pages 设置](https://github.com/AIBriefs/AIBriefs.github.io/settings/pages)
- 🔗 [查看当前仓库 Actions](../../actions)

## 测试部署

1. **触发部署**：
   - 提交并推送代码到源码仓库的 `main` 或 `master` 分支
   - 🔗 或者 [手动触发工作流](../../actions/workflows/deploy.yml)

2. **监控部署**：
   - 🔗 [查看 Actions 运行状态](../../actions)
   - 检查是否有错误信息

3. **验证结果**：
   - 部署成功后，访问 [https://AIBriefs.github.io](https://AIBriefs.github.io) 查看网站
   - 检查 [AIBriefs.github.io 仓库](https://github.com/AIBriefs/AIBriefs.github.io) 是否有新的提交

## 常见问题排查

### 权限问题
- **Token 权限不足**：确保 PAT 有 `repo` 权限
- **Secret 配置错误**：检查 `DEPLOY_TOKEN` 是否正确设置

### 部署失败
- **目标仓库不存在**：确保 `AIBriefs.github.io` 仓库已创建
- **分支不匹配**：检查目标仓库的默认分支名称
- **Hugo 构建失败**：检查 Hugo 配置和内容文件

### 网站访问问题
- **404 错误**：确认 GitHub Pages 已在目标仓库中启用
- **样式丢失**：检查 baseURL 配置是否正确
- **内容不更新**：GitHub Pages 可能需要几分钟来更新

## 优势

这种跨仓库部署方式的优势：
- ✅ **源码分离**：源码和生成的网站文件分开管理
- ✅ **清洁部署**：目标仓库只包含静态文件
- ✅ **版本控制**：可以独立管理源码和部署历史
- ✅ **灵活性**：可以从多个源码仓库部署到同一个网站
