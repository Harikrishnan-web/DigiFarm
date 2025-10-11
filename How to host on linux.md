---

## 🧰 **Prerequisites**

You need:

1. A **remote server** (e.g., AWS EC2, DigitalOcean Droplet, etc.)
2. Your **private SSH key (`.ppk`)** — for PuTTY authentication
3. **PuTTY** and **PuTTYgen** installed on your Windows system
4. An HTML/CSS project folder (e.g., `index.html`, `style.css`, etc.)

---

## ⚙️ **Step 1: Convert or Generate SSH Keys (if needed)**

If you already have a `.pem` key (e.g., from AWS), convert it to `.ppk` using **PuTTYgen**:

```bash
# Open PuTTYgen
Load → select your .pem file → Save private key → Save as mykey.ppk
```

Now you have your `mykey.ppk` file.

---

## 🔐 **Step 2: Connect to the Server via PuTTY**

1. Open **PuTTY**
2. In **Host Name (or IP address)** → enter your server’s public IP
   Example: `ec2-user@3.95.123.45` or `root@your_server_ip`
3. Go to **Connection → SSH → Auth → Credentials**

   * Under “Private key file for authentication”, browse and select `mykey.ppk`
4. (Optional) Save the session so you don’t re-enter this every time.
5. Click **Open**.

You’ll connect to your server shell.

---

## 🗂️ **Step 3: Prepare Web Server Directory**

Once connected:

```bash
# Switch to root (if not already)
sudo su

# Update system packages
apt update && apt upgrade -y   # (For Ubuntu/Debian)
# or
yum update -y                  # (For CentOS/RHEL)

# Install a simple web server (Apache)
apt install apache2 -y
# or
yum install httpd -y

# Start the web server
systemctl start apache2
systemctl enable apache2
# or for CentOS:
systemctl start httpd
systemctl enable httpd
```

Your web root directory is usually:

```
/var/www/html
```

---

## 📁 **Step 4: Upload Your Web Files**

### Option 1: Using **PSCP** (PuTTY Secure Copy)

Open **Command Prompt** on your Windows machine and run:

```bash
pscp -i "C:\path\to\mykey.ppk" -r "C:\path\to\your\website\*" username@your_server_ip:/var/www/html/
```

Example:

```bash
pscp -i "C:\Users\John\Desktop\mykey.ppk" -r "C:\Users\John\Documents\website\*" ubuntu@3.95.123.45:/var/www/html/
```

This copies your HTML/CSS files to the server’s web root.

---

## 🔧 **Step 5: Set Permissions**

Back in your PuTTY terminal:

```bash
sudo chown -R www-data:www-data /var/www/html
sudo chmod -R 755 /var/www/html
```

---

## 🌐 **Step 6: Access Your Website**

Open your browser and go to:

```
http://your_server_ip/
```

You should see your HTML page live 🎉

If not, check Apache status:

```bash
sudo systemctl status apache2
```

---

## 🔒 **Optional: Secure with Firewall Rules**

Allow HTTP traffic:

```bash
sudo ufw allow 'Apache'
sudo ufw enable
```

---

## ✅ **Summary of Commands**

```bash
# Update server
sudo apt update && sudo apt upgrade -y

# Install Apache
sudo apt install apache2 -y
sudo systemctl start apache2
sudo systemctl enable apache2

# Copy files from Windows (in CMD)
pscp -i "C:\path\to\mykey.ppk" -r "C:\path\to\website\*" user@server_ip:/var/www/html/

# Fix permissions
sudo chown -R www-data:www-data /var/www/html
sudo chmod -R 755 /var/www/html

# Visit your site
http://server_ip/
```

---

