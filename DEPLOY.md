# 智研平台部署包

## 文件清单

- `index.html` - 前台产品介绍页（已缩放 90%）
- `admin.html` - 后台留资管理页
- `nationwide-map.png` - 全国企业汇聚地图（526KB）
- `README.md` - 说明文档

## 部署步骤

### 方式一：Nginx/Apache 静态站点

1. 将所有文件上传到服务器 web 目录（如 `/var/www/html/zhiyan`）
2. 配置 Nginx：
   ```nginx
   server {
       listen 80;
       server_name your-domain.com;
       root /var/www/html/zhiyan;
       index index.html;
       
       location / {
           try_files $uri $uri/ =404;
       }
   }
   ```
3. 重启 Nginx：`sudo systemctl reload nginx`

### 方式二：Node.js 快速部署

1. 在部署目录创建 `server.js`：
   ```javascript
   const express = require('express');
   const app = express();
   app.use(express.static('.'));
   app.listen(8080, () => console.log('Server running on http://localhost:8080'));
   ```
2. 安装依赖：`npm install express`
3. 启动：`node server.js`

### 方式三：Python 快速测试

```bash
python -m http.server 8080
```

## 访问地址

- 前台页面：`http://your-server/` 或 `http://your-server/index.html`
- 后台管理：`http://your-server/admin.html`

## 数据说明

**留资表单数据存储在浏览器 localStorage**，不同访客的数据互相隔离。后台管理页只能查看当前浏览器提交的申请。

如需真正收集线索，建议接入：
- Formspree（免费额度 50 次/月）
- Tally（免费无限制）
- 自建后端 API

## 最新更新（2026-09-14）

- ✅ 数据更新：34 个省、6000 万企业、6 类维度
- ✅ 页面整体缩放 90%
- ✅ 汇聚全国优质企业模块移至数据底座上方
- ✅ 地图改用静态图片（nationwide-map.png）
