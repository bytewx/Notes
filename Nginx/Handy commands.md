# Handy commands

```
# Enable site
sudo ln -s /etc/nginx/sites-available/foo.conf /etc/nginx/sites-enabled/
# Disable site
sudo rm /etc/nginx/sites-enabled/foo.conf
# Restart/reload
sudo systemctl restart nginx
sudo systemctl reload nginx
# Firewall (Ubuntu ufw)
sudo ufw allow 'Nginx Full'
```
