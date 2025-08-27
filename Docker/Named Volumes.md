# Named Volumes

```
# Create a volume, then mount it (e.g., for DB)
docker volume create db-data
docker run -d --name db -v db-data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=root mysql:8.4
```
