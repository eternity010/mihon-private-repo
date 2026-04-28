# Mihon 私用仓库

这个目录用于托管你自己的 Mihon 插件仓库文件：

- `repo.json`
- `index.json`
- `index.min.json`
- `apk/tachiyomi-*.apk`（放在 `apk` 子目录）

## 0) 关键字段规范（必须遵守）

`index.json` / `index.min.json` 的 `apk` 字段只能写文件名，不要带 `apk/` 前缀。

正确示例：

```json
{ "apk": "tachiyomi-zh.manxiaosi-v1.4.15-release.apk" }
```

错误示例：

```json
{ "apk": "apk/tachiyomi-zh.manxiaosi-v1.4.15-release.apk" }
```

原因：Mihon 会按 `{repoUrl}/apk/{apk}` 自动拼接下载地址。

## 1) 首次配置
编辑 `repo_config.json`：

- `meta.name`：仓库显示名称
- `meta.website`：仓库主页（通常与 `baseUrl` 一致）
- `meta.signingKeyFingerprint`：你的签名证书 SHA-256 指纹
- `baseUrl`：公网访问前缀（例如 GitHub Pages）

## 2) 生成仓库索引
在 `D:\vue_project\BKcomic` 下执行：

```powershell
python .\tools\build_mihon_repo.py
```

生成后会更新本目录下的索引和 APK 文件。

建议在发布前做一次快速检查：

```powershell
python -m json.tool .\mihon-private-repo\index.json > $null
python -m json.tool .\mihon-private-repo\index.min.json > $null
```

## 3) 发布到公网
把本目录内容发布到静态托管（推荐 GitHub Pages）。

发布后，Mihon 建议添加仓库 URL：

```text
https://<your-user>.github.io/<repo>/repo.json
```

## 4) 每次更新插件
1. 先在 `extensions-source` 构建新版 APK。
2. 执行 `python .\tools\build_mihon_repo.py` 重新生成索引。
3. 提交并发布 `mihon-private-repo` 目录。

发布后建议验证：

```powershell
curl -I "https://<your-user>.github.io/<repo>/index.min.json"
curl -I "https://<your-user>.github.io/<repo>/apk/<apk-file-name>"
```

## 5) 一键更新脚本
在 `D:\vue_project\BKcomic` 执行：

```powershell
.\update_repo.ps1
```

只重建并提交、不推送：

```powershell
.\update_repo.ps1 -NoPush
```
