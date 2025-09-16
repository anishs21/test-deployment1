# React Frontend Deployment on Nginx (Sub-path `/test-deployment1/`)

```bash
# 1️⃣ Create Deployment Directory
sudo mkdir -p /var/www/html/test-deployment1

# Change ownership and give permission
sudo chown -R $USER:www-data /var/www/html/test-deployment1
sudo chmod -R 755 /var/www/html/test-deployment1

# 2️⃣ Configure Nginx
sudo nano /etc/nginx/sites-available/test-deployment1

# Paste the following inside the file:
# --------------------------------------------------------
server {
    listen 80;
    server_name _;  

    root /var/www/html;
    index index.html;

    location /test-deployment1/ {
        try_files $uri /test-deployment1/index.html;
    }
}
# --------------------------------------------------------

# Enable the site
sudo ln -s /etc/nginx/sites-available/test-deployment1 /etc/nginx/sites-enabled/

# Test and reload Nginx
sudo nginx -t
sudo systemctl reload nginx

# 3️⃣ Update React App for Sub-path
# Edit package.json and add homepage
# --------------------------------------------------------
{
  "name": "counter-game",
  "version": "0.1.0",
  "private": true,
  "homepage": "/test-deployment1",
  "dependencies": {
    "@testing-library/jest-dom": "^5.16.5",
    "@testing-library/react": "^13.4.0",
    "@testing-library/user-event": "^13.5.0",
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "react-scripts": "5.0.1",
    "web-vitals": "^2.1.4"
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  }
}
# --------------------------------------------------------

# 4️⃣ Build React App
npm install
npm run build

# 5️⃣ Copy Build Files to Server
# Option 1: Manual copy
sudo cp -r build/* /var/www/html/test-deployment1/
sudo chown -R $USER:www-data /var/www/html/test-deployment1
sudo chmod -R 755 /var/www/html/test-deployment1

# Option 2: Using GitHub Actions / CI/CD
# --------------------------------------------------------
- name: Copy frontend build to EC2
  uses: appleboy/scp-action@master
  with:
    host: ${{ secrets.EC2_HOST }}
    username: ${{ secrets.EC2_USER }}
    key: ${{ secrets.EC2_KEY }}
    source: "build/*"
    target: "/var/www/html/test-deployment1/"
    strip_components: 1
# --------------------------------------------------------

# 6️⃣ Reload Nginx
sudo nginx -t
sudo systemctl reload nginx

# ✅ Your app is now available at:
# http://<IP>/test-deployment1/

# 7️⃣ Notes / Common Issues
# - 404 on static files (/static/js/...) → check "homepage" in package.json
# - Permissions denied → ensure www-data or your user has 755 permissions
# - Nginx location mismatch → use try_files $uri /test-deployment1/index.html for React SPA
