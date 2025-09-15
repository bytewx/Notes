# Healthcheck using cron

```
#!/bin/bash
URL="http://127.0.0.1:8081/health"
if ! curl -fsS --max-time 5 "$URL" > /dev/null; then
    echo "$(date) Go service is down, restarting..." >> /var/log/service-health.log
    systemctl restart service.service
else
    echo "$(date) Go service is up" >> /var/log/service-health.log
fi
```
