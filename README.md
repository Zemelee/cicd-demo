# Vue3 + GitHub Actions CI/CD 部署演示

这是一个用于学习 CI/CD 自动化部署的 Vue3 项目。

## 项目结构

```
test/
├── .github/
│   └── workflows/
│       ├── deploy.yml          # Git 部署方式
│       └── deploy-scp.yml      # SFTP 部署方式
├── src/
│   ├── App.vue                 # 主组件
│   ├── main.js                 # 入口文件
│   └── style.css               # 样式文件
├── dist/                       # 构建输出（由构建生成）
├── index.html                  # HTML 模板
├── package.json                # 项目依赖
├── vite.config.js              # Vite 配置
└── .gitignore
```

## 快速开始

### 1. 本地开发

```bash
cd test
npm install
npm run dev
```

### 2. 本地构建

```bash
cd test
npm install
npm run build
```

## CI/CD 部署配置

### GitHub Secrets 配置

在 GitHub 仓库中设置以下 Secrets：

| Secret 名称 | 说明 |
|------------|------|
| `SERVER_HOST` | 服务器 IP 地址（如：`123.45.67.89`） |
| `SERVER_USERNAME` | SSH 用户名（如：`root`） |
| `SERVER_SSH_KEY` | SSH 私钥（生成方式见下方） |
| `SERVER_PORT` | SSH 端口（可选，默认 22） |

### 生成 SSH 密钥

```bash
# 生成 SSH 密钥对
ssh-keygen -t rsa -b 4096 -f github_actions_key -N ""

# 查看公钥（需要添加到 GitHub Secrets 的 SERVER_SSH_KEY 中）
cat github_actions_key.pub

# 将公钥添加到服务器
ssh-copy-id -i github_actions_key.pub root@sugarblack.top
```

### GitHub Actions 工作流

本项目提供两种部署方式：

1. **deploy.yml** - 使用 Git 拉取代码部署
2. **deploy-scp.yml** - 使用 SCP 传输构建产物部署（推荐）

## 服务器配置（CentOS）

### 1. 安装 Nginx

```bash
sudo yum install nginx -y
sudo systemctl start nginx
sudo systemctl enable nginx
```

### 2. 配置 Nginx

编辑 `/etc/nginx/conf.d/sugarblack.top.conf`：

```nginx
server {
    listen 83 ssl;
    server_name sugarblack.top;

    ssl_certificate /path/to/cert.pem;
    ssl_certificate_key /path/to/key.pem;

    root /var/www/sugarblack/dist;
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }

    # 禁用缓存以便测试
    add_header Cache-Control "no-cache";
}
```

### 3. 配置防火墙

```bash
sudo firewall-cmd --permanent --add-port=83/tcp
sudo firewall-cmd --reload
```

### 4. 设置部署目录

```bash
sudo mkdir -p /var/www/sugarblack
sudo chown -R $USER:$USER /var/www/sugarblack
```

## 部署流程

1. 将代码推送到 GitHub：
   ```bash
   git init
   git add .
   git commit -m "Initial commit"
   git branch -M main
   git remote add origin YOUR_GITHUB_REPO
   git push -u origin main
   ```

2. 在 GitHub Actions 中查看部署状态
3. 访问 https://sugarblack.top:83 查看部署结果

## 学习要点

- **GitHub Actions**：自动化的 CI/CD 工作流
- **Node.js 部署**：前端项目的构建和部署
- **SSH 密钥认证**：安全的服务器访问方式
- **Nginx 配置**：静态文件服务器配置
- **Vue3 + Vite**：现代化的前端开发工具链

## 资源

- [GitHub Actions 文档](https://docs.github.com/en/actions)
- [Vite 官方文档](https://vitejs.dev/)
- [Nginx 官方文档](https://nginx.org/en/docs/)
