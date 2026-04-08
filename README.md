# DAN custom install script

这是一个可通过 `curl | bash` 方式安装的自定义 `install.sh`。

相比原版安装脚本，这个版本额外支持在安装时直接写入邮箱域名配置：

- `--mail-domain DOMAIN`
- `--mail-domain-options CSV`
- `--enabled-email-domains CSV`

## 一次性安装示例

```bash
curl -fsSL https://raw.githubusercontent.com/YOUR_GITHUB_USERNAME/YOUR_REPO_NAME/main/install.sh | bash -s -- \
  --install-dir "$HOME/dan-runtime" \
  --background \
  --cpa-base-url 'https://cpa.example.com/' \
  --cpa-token 'replace-me' \
  --mail-api-url 'https://mail.example.com/' \
  --mail-api-key 'replace-me' \
  --mail-domain 'yourdomain.com' \
  --threads 20
```

## 多个域名示例

```bash
curl -fsSL https://raw.githubusercontent.com/YOUR_GITHUB_USERNAME/YOUR_REPO_NAME/main/install.sh | bash -s -- \
  --install-dir "$HOME/dan-runtime" \
  --background \
  --cpa-base-url 'https://cpa.example.com/' \
  --cpa-token 'replace-me' \
  --mail-api-url 'https://mail.example.com/' \
  --mail-api-key 'replace-me' \
  --mail-domain 'a.com' \
  --mail-domain 'b.com' \
  --threads 20
```

## 分开控制可选域名与启用域名

```bash
curl -fsSL https://raw.githubusercontent.com/YOUR_GITHUB_USERNAME/YOUR_REPO_NAME/main/install.sh | bash -s -- \
  --install-dir "$HOME/dan-runtime" \
  --background \
  --cpa-base-url 'https://cpa.example.com/' \
  --cpa-token 'replace-me' \
  --mail-api-url 'https://mail.example.com/' \
  --mail-api-key 'replace-me' \
  --mail-domain-options 'a.com,b.com' \
  --enabled-email-domains 'a.com' \
  --threads 20
```

## 说明

- 仓库需要是 **public**，这样 `raw.githubusercontent.com` 才能匿名下载。
- 上传后建议文件名保持为 `install.sh`。
- 如果你后续继续改脚本，只需要提交到 `main` 分支，`raw` 地址不变。
