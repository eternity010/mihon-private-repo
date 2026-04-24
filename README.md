# Mihon 私用仓库

这个目录用于托管你自己的 Mihon 插件仓库文件：

- `repo.json`
- `index.json`
- `index.min.json`
- `apks/*.apk`

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

生成后会更新本目录下的索引和 `apks` 文件。

## 3) 发布到公网
把本目录内容发布到静态托管（推荐 GitHub Pages）。

发布后，Mihon 添加仓库 URL：

```text
https://<your-user>.github.io/<repo>/index.min.json
```

## 4) 每次更新插件
1. 先在 `extensions-source` 构建新版 APK。
2. 执行 `python .\tools\build_mihon_repo.py` 重新生成索引。
3. 提交并发布 `mihon-private-repo` 目录。

