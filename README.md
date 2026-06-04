# React Application Deployment Guide (AWS EC2 + Nginx)

This guide explains how to deploy a React application on an AWS EC2 instance using Nginx.

---

## Repository

```bash
git clone https://github.com/<username>/<repository-name>.git
```

Example:

```bash
git clone https://github.com/khomesh/react-demo.git
```

---

## Prerequisites

- AWS Account
- EC2 Ubuntu Server
- SSH Key (.pem file)
- Git
- Node.js & npm
- Nginx

---

## Step 1: Connect to EC2

```bash
ssh -i your-key.pem ubuntu@YOUR_PUBLIC_IP
```

Example:

```bash
ssh -i react-app.pem ubuntu@13.233.xxx.xxx
```

---

## Step 2: Update Server

```bash
sudo apt update
sudo apt upgrade -y
```

---

## Step 3: Install Node.js

```bash
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt install -y nodejs
```

Verify installation:

```bash
node -v
npm -v
```

---

## Step 4: Install Git

```bash
sudo apt install git -y
```

Verify:

```bash
git --version
```

---

## Step 5: Clone Repository

```bash
git clone https://github.com/<username>/<repository-name>.git
cd <repository-name>
```

---

## Step 6: Install Dependencies

```bash
npm install
```

---

## Step 7: Create Production Build

```bash
npm run build
```

A build folder will be generated.

---

## Step 8: Install Nginx

```bash
sudo apt install nginx -y
```

Verify:

```bash
sudo systemctl status nginx
```

---

## Step 9: Copy Build Files

Remove default files:

```bash
sudo rm -rf /var/www/html/*
```

Copy React build:

```bash
sudo cp -r build/* /var/www/html/
```

---

## Step 10: Configure Nginx

Open configuration:

```bash
sudo nano /etc/nginx/sites-available/default
```

Replace content with:

```nginx
server {
    listen 80;
    server_name _;

    root /var/www/html;
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }
}
```

Save and exit.

---

## Step 11: Validate Configuration

```bash
sudo nginx -t
```

Expected Output:

```text
syntax is ok
test is successful
```

---

## Step 12: Restart Nginx

```bash
sudo systemctl restart nginx
sudo systemctl enable nginx
```

---

## Step 13: Configure AWS Security Group

Allow:

| Type | Port |
|--------|--------|
| SSH | 22 |
| HTTP | 80 |
| HTTPS | 443 |

---

## Step 14: Access Application

Open browser:

```text
http://YOUR_PUBLIC_IP
```

Example:

```text
http://13.233.xxx.xxx
```

Your React application should now be live.

---

# Deploying Future Changes

Pull latest code:

```bash
git pull origin main
```

Install packages:

```bash
npm install
```

Create production build:

```bash
npm run build
```

Replace old build:

```bash
sudo rm -rf /var/www/html/*
sudo cp -r build/* /var/www/html/
```

Restart nginx:

```bash
sudo systemctl restart nginx
```

---

# Useful Commands

Check nginx status:

```bash
sudo systemctl status nginx
```

Restart nginx:

```bash
sudo systemctl restart nginx
```

View nginx logs:

```bash
sudo tail -f /var/log/nginx/error.log
```

Check running ports:

```bash
sudo netstat -tulpn
```

---

# Troubleshooting

## Blank Page

```bash
npm run build
```

Verify build folder exists.

## React Routes Not Working

Make sure nginx contains:

```nginx
try_files $uri $uri/ /index.html;
```

## Nginx Not Starting

```bash
sudo nginx -t
```

Fix configuration errors and restart.

## Permission Issues

```bash
sudo chmod -R 755 /var/www/html
```

---

# Author

Khomesh Gajbhiye

Backend Engineer | Node.js Developer | AWS Cloud Engineer | AWS Certified Solutions Architect – Associate
