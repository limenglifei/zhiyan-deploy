# 智研平台最新部署包

本包是可直接部署到 Nginx、Apache 或任意静态 Web 服务器的前后台静态站点。

## 文件清单

- `index.html`：前台产品介绍页，包含全国企业动态呼吸图、四大模型引擎与典型应用场景。
- `admin.html`：体验申请管理页面。
- `images/`：典型应用场景截图资源。
- `nationwide-map.png`：原始全国企业地图备份资源。
- `.nojekyll`：GitHub Pages 兼容文件。

## Nginx 部署（推荐）

1. 将压缩包解压到服务器目录，例如 `/var/www/zhiyan-deploy`。
2. 创建 Nginx 虚拟主机配置：

```nginx
server {
    listen 80;
    server_name your-domain.com;

    root /var/www/zhiyan-deploy;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~* \.(?:png|jpg|jpeg|gif|webp|svg|css|js)$ {
        expires 7d;
        add_header Cache-Control "public";
    }
}
```

3. 检查并重载配置：

```bash
sudo nginx -t
sudo systemctl reload nginx
```

## Apache 部署

将压缩包内容上传到站点根目录，例如 `/var/www/html/zhiyan-deploy`，然后确保 Apache 已启用静态文件访问即可。

```apache
<VirtualHost *:80>
    ServerName your-domain.com
    DocumentRoot /var/www/html/zhiyan-deploy

    <Directory /var/www/html/zhiyan-deploy>
        Require all granted
        Options -Indexes
    </Directory>
</VirtualHost>
```

## 访问地址

- 前台：`http://your-domain.com/`
- 管理页：`http://your-domain.com/admin.html`

## 重要说明

此版本是静态站点。前台留资和管理页数据目前存储于浏览器 `localStorage`；部署到本地服务器后，不同用户、不同浏览器或不同设备之间不会共享数据。`admin.html` 的登录校验也仍在浏览器端执行。

如需真正将前台留资汇总到后台并安全保护管理账号，需要额外部署 API 与数据库，再将前台、后台接入该 API。

## 当前更新

- 全国企业地图升级为内联 SVG 企业迁入迁出动态图，绿色流线表示迁入、红色流线表示迁出，并展示净迁入指标。
- 数据与模型底座新增：产业链构建及强弱分析模型、招商多因子评估模型、政策匹配引擎、企业尽调引擎。
- “典型业务场景”更新为“典型应用场景”。
- 典型应用场景图片保持原始比例，并使用延迟加载。