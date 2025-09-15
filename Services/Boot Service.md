# Boot Service

- Create service as systemd unit:
```
sudo nano /etc/systemd/system/news.service
```
```
[Unit]
Description=Go Service
After=network.target

[Service]
User=www-data
Group=www-data
WorkingDirectory=/var/www/service
ExecStart=/var/www/service/service
Restart=always
RestartSec=5
Environment="PORT=8081"

[Install]
WantedBy=multi-user.target
```
- Enabling service:
```
sudo systemctl daemon-reload
sudo systemctl enable news.service
sudo systemctl start news.service
```
- Status:
```
systemctl status news.service
```
