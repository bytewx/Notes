# Container Networking

Containers in the same user-defined network can reach each other by service name.

```
docker network create app-net
docker run -d --name db --network app-net mysql:8.4
docker run -d --name api --network app-net yourname/your-app:dev
# In api, connect to host 'db' (not 127.0.0.1)
```
