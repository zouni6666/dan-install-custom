# DAN custom install script

这是一个可通过 `curl | bash` 方式安装的自定义 `install.sh`。

相比原版安装脚本，这个版本额外支持在安装时直接写入邮箱域名配置：

- `--mail-domain DOMAIN`
- `--mail-domain-options CSV`
- `--enabled-email-domains CSV`
- `--domains-api-url URL`

默认情况下，脚本会继续使用内置的域名列表接口，不再根据 `--cpa-base-url`
自动拼接 `/v0/management/domains`。这样自建 CPA 面板即使没有这个接口，也不会因为
404 导致安装阶段出问题。

如果你自己维护了兼容的域名列表接口，或者已经在反代上补了
`/v0/management/domains`，再显式传 `--domains-api-url` 即可。

注意：`dan-web` 的较新运行时版本也会把 `cpa_base_url` 规范化到
`/v0/management` 后再请求一次 `GET /domains`。如果你使用的是自建 CPA，
最稳妥的做法仍然是在同域名上提供一个兼容接口，返回格式至少包含：

```json
{"domains":["a.example.com","b.example.com"]}
```

例如 Nginx 可以直接这样补：

```nginx
location = /v0/management/domains {
    default_type application/json;
    return 200 '{"domains":["a.example.com","b.example.com"]}';
}
```

如果你不想补这个接口，也可以在安装阶段直接把
`mail_domain_options` / `enabled_email_domains` 预写进配置，运行时会优先使用已有域名。

## 一次性安装示例

```bash
curl -fsSL https://raw.githubusercontent.com/zouni6666/dan-install-custom/main/install.sh | bash -s -- \
  --install-dir "$HOME/dan-runtime" \
  --background \
  --cpa-base-url 'https://cpa.example.com/' \
  --cpa-token 'replace-me' \
  --mail-api-url 'https://mail.example.com/' \
  --mail-api-key 'replace-me' \
  --mail-domain 'yourdomain.com' \
  --threads 20
```

## 自定义 domains API 示例

```bash
curl -fsSL https://raw.githubusercontent.com/zouni6666/dan-install-custom/main/install.sh | bash -s -- \
  --install-dir "$HOME/dan-runtime" \
  --background \
  --cpa-base-url 'https://cpa.example.com/' \
  --cpa-token 'replace-me' \
  --domains-api-url 'https://cpa.example.com/v0/management/domains' \
  --mail-api-url 'https://mail.example.com/' \
  --mail-api-key 'replace-me' \
  --threads 20
```

## 多个域名示例

```bash
curl -fsSL https://raw.githubusercontent.com/zouni6666/dan-install-custom/main/install.sh | bash -s -- \
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
curl -fsSL https://raw.githubusercontent.com/zouni6666/dan-install-custom/main/install.sh | bash -s -- \
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
