# HomeGenii 部署指南

本指南提供两种部署方案,您可以根据需求选择:

## 📋 目录

- [方案一: 静态部署(推荐用于演示)](#方案一-静态部署)
- [方案二: 完整后端部署](#方案二-完整后端部署)

---

## 🌟 方案一: 静态部署(推荐用于演示)

**优点**: 免费、快速、无需维护服务器
**适用场景**: 学术演示、审稿人访问

### 选项 A: GitHub Pages (免费)

#### 1. 准备工作

```bash
# 创建新的 GitHub 仓库
git init
git add .
git commit -m "Initial commit"
```

#### 2. 项目结构

```
homegenii/
├── index.html (您的网页文件)
└── README.md
```

#### 3. 推送到 GitHub

```bash
# 在 GitHub 上创建新仓库后
git remote add origin https://github.com/YOUR_USERNAME/homegenii.git
git branch -M main
git push -u origin main
```

#### 4. 启用 GitHub Pages

1. 进入仓库的 Settings
2. 点击左侧 "Pages"
3. Source 选择 "Deploy from a branch"
4. Branch 选择 "main" 和 "/ (root)"
5. 点击 Save

#### 5. 访问地址

几分钟后,您的网站将在以下地址可访问:

```
https://YOUR_USERNAME.github.io/homegenii/
```

---

### 选项 B: Vercel (免费,推荐)

#### 1. 安装 Vercel CLI

```bash
npm install -g vercel
```

#### 2. 部署

```bash
# 在项目目录下运行
vercel

# 按照提示操作:
# - Set up and deploy? Y
# - Which scope? 选择您的账户
# - Link to existing project? N
# - What's your project's name? homegenii
# - In which directory is your code located? ./
```

#### 3. 配置 vercel.json (可选)

创建 `vercel.json` 文件:

```json
{
  "version": 2,
  "builds": [
    {
      "src": "index.html",
      "use": "@vercel/static"
    }
  ],
  "routes": [
    {
      "src": "/(.*)",
      "dest": "/index.html"
    }
  ]
}
```

#### 4. 生产部署

```bash
vercel --prod
```

#### 5. 访问地址

Vercel 会提供一个类似这样的地址:

```
https://homegenii.vercel.app
```

---

### 选项 C: Netlify (免费)

#### 1. 通过网页部署

1. 访问 https://app.netlify.com
2. 注册/登录账户
3. 点击 "Add new site" → "Deploy manually"
4. 将包含 index.html 的文件夹拖拽到上传区域
5. 等待部署完成

#### 2. 通过 CLI 部署

```bash
# 安装 Netlify CLI
npm install -g netlify-cli

# 登录
netlify login

# 部署
netlify deploy

# 生产部署
netlify deploy --prod
```

#### 3. 访问地址

```
https://random-name.netlify.app
```

---

## 🚀 方案二: 完整后端部署

**优点**: 支持文件上传、API 调用、完整功能
**适用场景**: 需要实际功能演示

### 准备工作

#### 1. 项目结构

```
homegenii/
├── server.js           (后端服务器)
├── package.json        (依赖配置)
├── .gitignore         (Git 忽略文件)
├── public/            (静态文件目录)
│   └── index.html     (您的网页文件)
├── uploads/           (上传文件目录,自动创建)
└── README.md
```

#### 2. 创建目录并放置文件

```bash
mkdir homegenii
cd homegenii

# 创建 public 目录并放置 HTML 文件
mkdir public
# 将您的 index.html 移动到 public 目录

# 创建 server.js, package.json 等文件(已提供)
```

---

### 部署到云服务器

#### 选项 A: Railway (免费套餐,推荐)

1. **注册 Railway**
   - 访问 https://railway.app
   - 使用 GitHub 账户登录

2. **准备代码**

```bash
git init
git add .
git commit -m "Initial commit"
git push origin main
```

3. **部署**

   - 在 Railway 仪表板点击 "New Project"
   - 选择 "Deploy from GitHub repo"
   - 选择您的仓库
   - Railway 会自动检测 Node.js 项目并部署

4. **配置环境变量**

   - 在项目设置中添加:

   ```
   PORT=3000
   NODE_ENV=production
   ```

5. **获取访问地址**

   - Railway 会自动生成一个公开 URL
   - 类似: https://homegenii-production.up.railway.app

---

#### 选项 B: Render (免费)

1. **注册 Render**

   - 访问 https://render.com
   - 使用 GitHub 账户登录

2. **创建 Web Service**

   - 点击 "New +" → "Web Service"
   - 连接您的 GitHub 仓库
   - 填写配置:
     - Name: `homegenii`
     - Environment: `Node`
     - Build Command: `npm install`
     - Start Command: `npm start`

3. **部署**

   - 点击 "Create Web Service"
   - 等待部署完成

4. **访问地址**

   ```
   https://homegenii.onrender.com
   ```

---

#### 选项 C: 自己的 VPS (阿里云/腾讯云等)

1. **连接服务器**

```bash
ssh root@your-server-ip
```

2. **安装 Node.js**

```bash
# 使用 NodeSource 安装 Node.js 18
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt-get install -y nodejs

# 验证安装
node --version
npm --version
```

3. **上传代码**

```bash
# 方法1: 使用 Git
cd /var/www
git clone https://github.com/YOUR_USERNAME/homegenii.git
cd homegenii

# 方法2: 使用 SCP
# 在本地运行:
scp -r ./homegenii root@your-server-ip:/var/www/
```

4. **安装依赖**

```bash
npm install
```

5. **使用 PM2 管理进程**

```bash
# 安装 PM2
sudo npm install -g pm2

# 启动应用
pm2 start server.js --name homegenii

# 设置开机自启
pm2 startup
pm2 save
```

6. **配置 Nginx 反向代理**

```bash
# 安装 Nginx
sudo apt-get install nginx

# 创建配置文件
sudo nano /etc/nginx/sites-available/homegenii
```

添加以下配置:

```nginx
server {
    listen 80;
    server_name your-domain.com;  # 替换为您的域名或 IP

    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```

```bash
# 启用配置
sudo ln -s /etc/nginx/sites-available/homegenii /etc/nginx/sites-enabled/

# 测试配置
sudo nginx -t

# 重启 Nginx
sudo systemctl restart nginx
```

7. **配置 HTTPS (可选但推荐)**

```bash
# 安装 Certbot
sudo apt-get install certbot python3-certbot-nginx

# 获取 SSL 证书
sudo certbot --nginx -d your-domain.com

# 自动续期
sudo certbot renew --dry-run
```

8. **防火墙设置**

```bash
# 允许 HTTP 和 HTTPS
sudo ufw allow 'Nginx Full'
sudo ufw allow ssh
sudo ufw enable
```

9. **查看应用状态**

```bash
# 查看 PM2 状态
pm2 status

# 查看日志
pm2 logs homegenii

# 重启应用
pm2 restart homegenii
```

---

## 🔧 本地测试

在部署前,建议先在本地测试:

```bash
# 安装依赖
npm install

# 启动开发服务器
npm start

# 访问
http://localhost:3000
```

---

## 📝 部署后配置

### 更新 HTML 文件中的 API 端点

如果使用方案二(完整后端),需要修改 HTML 文件中的 API 调用地址。

在 `public/index.html` 中找到需要调用 API 的地方,添加:

```javascript
const API_BASE_URL = window.location.origin;

// 示例: 上传文件
fetch(`${API_BASE_URL}/api/upload/floorplan`, {
    method: 'POST',
    body: formData
});
```

---

## 🔗 在 GitHub 添加链接

在您的 GitHub 仓库中:

1. **README.md 中添加**

```markdown
## 🌐 在线演示

访问地址: [HomeGenii Demo](https://your-deployment-url.com)
```

2. **仓库 About 设置**
   - 点击仓库右上角的 ⚙️ (设置图标)
   - 在 Website 输入框填入您的部署地址
   - 点击保存

---

## 🐛 常见问题

### Q1: GitHub Pages 不显示样式?

**A**: 检查资源路径是否正确,使用相对路径或完整 URL。

### Q2: 文件上传功能不工作?

**A**: 静态部署不支持文件上传,需要使用方案二(完整后端)。

### Q3: 如何查看服务器日志?

**A**: 

- Railway/Render: 在控制台的 Logs 标签
- VPS: 使用 `pm2 logs homegenii`

### Q4: 如何更新部署?

**A**:

- GitHub Pages: 推送新代码到 GitHub

- Vercel/Netlify: 推送代码会自动部署

- Railway/Render: 推送代码会自动部署

- VPS: 

  ```bash
  cd /var/www/homegenii
  git pull
  pm2 restart homegenii
  ```

---

## 🎯 推荐方案

| 场景         | 推荐方案               | 原因                   |
| ------------ | ---------------------- | ---------------------- |
| 纯演示展示   | Vercel 或 GitHub Pages | 免费、快速、易用       |
| 需要文件上传 | Railway 或 Render      | 免费套餐充足、自动部署 |
| 完整功能     | 自己的 VPS             | 完全控制、可定制       |
| 学术论文审稿 | Vercel                 | 最稳定、最快速         |

---

## 📧 技术支持

如有问题,请查看:

- [Express.js 文档](https://expressjs.com/)
- [Vercel 文档](https://vercel.com/docs)
- [Railway 文档](https://docs.railway.app/)

---

**祝您部署顺利! 🎉**
