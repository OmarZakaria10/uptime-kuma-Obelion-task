# 🚀 Deployment Setup Guide (Beginner Friendly)

This guide shows you how to set up **automatic deployment** for Uptime Kuma to your DigitalOcean server.

## 📋 What This Does

Every time you push code to the `main` branch on GitHub:
1. ✅ GitHub Actions automatically runs
2. ✅ Copies your `docker-compose.yml` to the server
3. ✅ Updates Uptime Kuma to the latest version
4. ✅ Restarts the container
5. ✅ Verifies it's working

**No manual SSH needed!** Everything is automated.

---

## 🔧 Step 1: Get Your Server IP Address

From your Terraform outputs:

```bash
cd /media/omar/01DADC72FB780420/Projects/Obelion-task/my-terraform
terraform output frontend_public_ip
```

**Example output:** `157.245.119.32`

📝 **Write this down** - you'll need it in Step 3.

---

## 🔑 Step 2: Get Your SSH Private Key

You need the **private key** that can access your DigitalOcean server.

```bash
# Display your SSH private key
cat ~/.ssh/id_rsa
```

You'll see something like:
```
-----BEGIN OPENSSH PRIVATE KEY-----
b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAABlwAAAAdzc2gtcn
NhAAAAAwEAAQAAAYEA1xM5...
(many more lines)
...
-----END OPENSSH PRIVATE KEY-----
```

📝 **Copy the ENTIRE content** (including the `-----BEGIN` and `-----END` lines).

---

## 🔐 Step 3: Add Secrets to GitHub

Go to your GitHub repository:

1. Click **Settings** (top menu)
2. Click **Secrets and variables** → **Actions** (left sidebar)
3. Click **New repository secret** (green button)

Add these **2 secrets**:

### Secret 1: `SSH_PRIVATE_KEY`

- **Name:** `SSH_PRIVATE_KEY`
- **Value:** Paste the entire private key from Step 2
- Click **Add secret**

### Secret 2: `FRONTEND_HOST`

- **Name:** `FRONTEND_HOST`
- **Value:** Your server IP (e.g., `157.245.119.32`)
- Click **Add secret**

---

## ✅ Step 4: Test the Deployment

### Option A: Push a Change

1. Make a small change to any file (e.g., edit README.md)
2. Commit and push to `main` branch:

```bash
git add .
git commit -m "Test deployment"
git push origin main
```

3. Go to **Actions** tab on GitHub
4. Watch your deployment run! 🎉

### Option B: Manual Trigger

1. Go to **Actions** tab on GitHub
2. Click **Deploy Uptime Kuma to DigitalOcean**
3. Click **Run workflow** → **Run workflow**

---

## 📊 What You'll See

In the GitHub Actions log, you'll see:

```
📥 Checkout Code ... ✅
🔐 Setup SSH Key ... ✅
📤 Copy Docker Compose File ... ✅
🚀 Deploy Uptime Kuma ... 
   🔄 Updating Uptime Kuma...
   ✅ Container Status:
   NAME            IMAGE                    STATUS
   uptime-kuma     louislam/uptime-kuma:1   Up 3 seconds (healthy)
   🎉 Deployment completed!
✅ Verify Deployment ... 
   ✅ Uptime Kuma is responding!
   🌐 Access it at: http://157.245.119.32
```

---

## 🎯 How to Update Uptime Kuma Configuration

Just edit `compose.yaml` in this repo and push:

```bash
# Edit the file
nano compose.yaml

# Make your changes (e.g., change ports, add environment variables)

# Commit and push
git add compose.yaml
git commit -m "Update Uptime Kuma config"
git push origin main
```

GitHub Actions will automatically deploy the new configuration! 🚀

---

## 🐛 Troubleshooting

### Problem: "Permission denied (publickey)"

**Solution:** Your `SSH_PRIVATE_KEY` secret is wrong or missing.

1. Make sure you copied the **entire** private key (including BEGIN/END lines)
2. Check it has the right format (should start with `-----BEGIN OPENSSH PRIVATE KEY-----`)
3. Delete the secret and re-add it

### Problem: "Connection refused"

**Solution:** Your `FRONTEND_HOST` IP is wrong.

1. Check the IP with: `terraform output frontend_public_ip`
2. Update the `FRONTEND_HOST` secret with the correct IP

### Problem: "Container is not running"

**Solution:** SSH to the server and check logs:

```bash
ssh root@<your-server-ip>
cd /opt/uptime-kuma
docker compose logs
```

---

## 🎓 Group B Requirements Checklist

This deployment satisfies **Obelion Assessment Group B**:

- ✅ **CI/CD Pipeline:** GitHub Actions auto-deploys on push to `main`
- ✅ **Automated Deployment:** No manual steps needed
- ✅ **Basic Monitoring:** Verifies deployment worked
- ✅ **Documentation:** This guide explains everything
- ✅ **Beginner-Friendly:** Simple, clear, step-by-step

---

## 🔄 Next Steps

After setting this up:

1. ✅ Configure Uptime Kuma monitors (login at `http://<your-ip>`)
2. ✅ Add monitors for your backend server
3. ✅ Set up email/Slack notifications
4. ✅ Test the CI/CD by making changes and pushing

**You're all set!** 🎉
