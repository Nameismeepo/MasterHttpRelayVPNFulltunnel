# MasterHttpRelayVPNFulltunnel

عالیه 👌  
من برات یک نسخه **تمیز، ساختارمند و مناسب README گیتهاب** آماده کردم (کاملاً Markdown استاندارد + تیترهای مرتب + توضیح حرفه‌ای‌تر). بعدش هم ترجمه کامل انگلیسیش رو گذاشتم که می‌تونی جدا یا دو زبانه استفاده کنی.

---

# ✅ نسخه فارسی (برای README)

```markdown
# MHRV Server Setup Guide

## مستندات راه‌اندازی سرور MHRV

### پیش‌نیازها
- یک سرور مجازی با سیستم‌عامل Ubuntu (ترجیحاً 22.04)
- یک دامنه یا ساب‌دامین (مانند DuckDNS) که رکورد A آن دقیقاً به IP سرور اشاره کند

---

## مرحله 1: نصب Docker

```bash
curl -fsSL https://get.docker.com | sudo sh
```

---

## مرحله 2: نصب وب‌سرور Caddy

```bash
sudo apt update && sudo apt install -y debian-keyring debian-archive-keyring apt-transport-https curl && curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/gpg.key' | sudo gpg --dearmor -o /usr/share/keyrings/caddy-stable-archive-keyring.gpg && curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/debian.deb.txt' | sudo tee /etc/apt/sources.list.d/caddy-stable.list && sudo apt update && sudo apt install caddy -y
```

---

## مرحله 3: باز کردن پورت‌ها

```bash
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

---

## مرحله 4: بهینه‌سازی تنظیمات شبکه

```bash
echo -e "net.ipv4.ip_local_port_range=10000 65535\nnet.ipv4.tcp_max_orphans=262144\nnet.ipv4.tcp_timestamps=1\nnet.ipv4.tcp_slow_start_after_idle=0\nnet.core.bpf_jit_enable=1" | sudo tee /etc/sysctl.d/99-mhr-extreme-plus.conf && sudo sysctl --system
```

---

## مرحله 5: پیکربندی Caddy

🔹 به جای `yourdomain.com` دامنه خود را قرار دهید.

```bash
echo -e "yourdomain.com {\n log {\n level ERROR\n }\n route /almas/* {\n uri strip_prefix /almas\n reverse_proxy 127.0.0.1:8888\n }\n}" | sudo tee /etc/caddy/Caddyfile && sudo mkdir -p /etc/systemd/system/caddy.service.d && echo -e "[Service]\nLimitNOFILE=1048576" | sudo tee /etc/systemd/system/caddy.service.d/override.conf && sudo systemctl daemon-reload && sudo systemctl restart caddy
```

---

## مرحله 6: اجرای هسته MHRV

🔹 به جای `change me` یک رمز قوی قرار دهید.

```bash
sudo docker rm -f mhrv-almas 2>/dev/null; sudo docker run -d --name mhrv-almas --network host --ulimit nofile=1048576:1048576 --restart unless-stopped -e PORT=8888 -e TUNNEL_AUTH_KEY="change me" -e RUST_LOG="error" ghcr.io/therealaleph/mhrv-tunnel-node:latest
```

---

## مرحله 7: تست سلامت سرور

```bash
curl -i -X POST https://yourdomain.com/almas/tunnel/batch -H "Authorization: Bearer change me" -H "Content-Type: application/json" -d '{"v":1,"ops":[]}'
```

✅ در صورت موفقیت باید کد `200` دریافت کنید.

---

## بروزرسانی سرور

```bash
sudo docker pull ghcr.io/therealaleph/mhrv-tunnel-node:latest
```

سپس کانتینر را مجدداً اجرا کنید.

---

# ☁️ راه‌اندازی Google Relay

## مرحله 1: Google Apps Script

1. وارد https://script.google.com شوید
2. New Project
3. محتوای `apps_script/Code.gs` را جایگذاری کنید
4. مقدار زیر را تغییر دهید:

```js
const AUTH_KEY = "CHANGE_ME_TO_A_STRONG_SECRET";
```

Deploy → New Deployment → Web App  
- Execute as: **Me**  
- Who has access: **Anyone**

✅ Deployment ID را ذخیره کنید.

---

## مرحله 2: دانلود پروژه

```bash
git clone https://github.com/masterking32/MasterHttpRelayVPN.git
cd MasterHttpRelayVPN
```

یا نسخه ZIP را دانلود کنید.

---

## مرحله 3: اجرا

Windows:
```
start.bat
```

Linux / macOS:
```bash
chmod +x start.sh
./start.sh
```

---

## مرحله 4: تنظیم مرورگر

| Field | Value |
|-------|-------|
| Proxy Type | HTTP |
| Address | 127.0.0.1 |
| Port | 8085 |
| SOCKS5 Port | 1080 |

Telegram:
```
https://t.me/socks?server=127.0.0.1&port=1080
```
```

---

# ✅ English Version (Professional GitHub Ready)

```markdown
# MHRV Server Setup Guide

## Prerequisites

- A VPS running Ubuntu (recommended: 22.04)
- A domain or subdomain (e.g., DuckDNS) with its A record pointing to your server IP

---

## Step 1: Install Docker

```bash
curl -fsSL https://get.docker.com | sudo sh
```

---

## Step 2: Install Caddy Web Server

```bash
sudo apt update && sudo apt install -y debian-keyring debian-archive-keyring apt-transport-https curl && curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/gpg.key' | sudo gpg --dearmor -o /usr/share/keyrings/caddy-stable-archive-keyring.gpg && curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/debian.deb.txt' | sudo tee /etc/apt/sources.list.d/caddy-stable.list && sudo apt update && sudo apt install caddy -y
```

---

## Step 3: Open Required Ports

```bash
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

---

## Step 4: Optimize Kernel Network Settings

```bash
echo -e "net.ipv4.ip_local_port_range=10000 65535\nnet.ipv4.tcp_max_orphans=262144\nnet.ipv4.tcp_timestamps=1\nnet.ipv4.tcp_slow_start_after_idle=0\nnet.core.bpf_jit_enable=1" | sudo tee /etc/sysctl.d/99-mhr-extreme-plus.conf && sudo sysctl --system
```

---

## Step 5: Configure Caddy

Replace `yourdomain.com` with your actual domain:

```bash
echo -e "yourdomain.com {\n log {\n level ERROR\n }\n route /almas/* {\n uri strip_prefix /almas\n reverse_proxy 127.0.0.1:8888\n }\n}" | sudo tee /etc/caddy/Caddyfile && sudo mkdir -p /etc/systemd/system/caddy.service.d && echo -e "[Service]\nLimitNOFILE=1048576" | sudo tee /etc/systemd/system/caddy.service.d/override.conf && sudo systemctl daemon-reload && sudo systemctl restart caddy
```

---

## Step 6: Run MHRV Core

Replace `change me` with a strong secret key:

```bash
sudo docker rm -f mhrv-almas 2>/dev/null; sudo docker run -d --name mhrv-almas --network host --ulimit nofile=1048576:1048576 --restart unless-stopped -e PORT=8888 -e TUNNEL_AUTH_KEY="change me" -e RUST_LOG="error" ghcr.io/therealaleph/mhrv-tunnel-node:latest
```

---

## Step 7: Health Check

```bash
curl -i -X POST https://yourdomain.com/almas/tunnel/batch -H "Authorization: Bearer change me" -H "Content-Type: application/json" -d '{"v":1,"ops":[]}'
```

✅ Expected result: HTTP `200`

---

# ☁️ Google Relay Setup

## Step 1: Deploy on Google Apps Script

1. Go to https://script.google.com
2. Create a new project
3. Paste contents of `apps_script/Code.gs`
4. Change:

```js
const AUTH_KEY = "CHANGE_ME_TO_A_STRONG_SECRET";
```

Deploy → New Deployment → Web App  
- Execute as: **Me**  
- Who has access: **Anyone**

Save your **Deployment ID**.

---

## Step 2: Download Project

```bash
git clone https://github.com/masterking32/MasterHttpRelayVPN.git
cd MasterHttpRelayVPN
```

---

## Step 3: Run

Windows:
```
start.bat
```

Linux/macOS:
```bash
chmod +x start.sh
./start.sh
```

---

## Step 4: Configure Browser

| Field | Value |
|-------|-------|
| Proxy Type | HTTP |
| Address | 127.0.0.1 |
| Port | 8085 |
| SOCKS5 Port | 1080 |

Telegram:
```
https://t.me/socks?server=127.0.0.1&port=1080
```
```

---

اگر بخواهی می‌توانم یک نسخه **خیلی حرفه‌ای‌تر با Badge، Table of Contents، Warning Box، Troubleshooting و بخش امنیتی** هم برات طراحی کنم که پروژه‌ات کاملاً استاندارد اوپن‌سورس دیده شود.
