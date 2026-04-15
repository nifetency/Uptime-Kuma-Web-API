# Uptime-Kuma-Web-API

## A REST API wrapper for [Uptime Kuma](https://github.com/louislam/uptime-kuma) using [Uptime-Kuma-API](https://github.com/lucasheld/uptime-kuma-api)

---

## EndPoints:

![Alt text](./images/1.png)
![Alt text](./images/2.png)

## How to use it:

---

### Environment Variables :

#### Required
You have to define these ENV VARS in order to connect to your KUMA server.

    KUMA_SERVER: The URL of your Uptime Kuma instance. ex: https://uptime.example.com
    KUMA_USERNAME: The username of your Uptime Kuma user
    KUMA_PASSWORD: The password of your Uptime Kuma user
    ADMIN_PASSWORD: An admin password to access the API

#### Optional
Additional configuration variables available

    ACCESS_TOKEN_EXPIRATION: Minutes the access token should be valid. Defaults to 8 days.
    SECRET_KEY: A secret value to encode JWTs with

#### Note:

You have to define your ADMIN_PASSWORD or you wont be able to connect to your rest api.

You will connect with those credentials:

    username = admin
    password = <ADMIN_PASSWORD>

### Features:

- Multi user Kuma api ( without privilege YET!!) with a small SQLite db
- Easy to use REST API with most of the Uptime-Kuma features
- Swagger Docs
- Dockerized [UptimeKuma_RestAPI Image](https://hub.docker.com/repository/docker/medaziz11/uptimekuma_restapi)
- Multi-architecture support (amd64, arm64)



## Installation

###  Using Docker Compose 

```yaml
version: "3.9"
services:
  kuma:
    container_name: uptime-kuma
    image: louislam/uptime-kuma:latest
    ports:
      - "3001:3001"
    restart: always
    volumes:
      - uptime-kuma:/app/data

  api:
    container_name: backend
    image: medaziz11/uptimekuma_restapi
    volumes:
      - api:/db
    restart: always
    environment:
      - KUMA_SERVER=http://kuma:3001
      - KUMA_USERNAME=test
      - KUMA_PASSWORD=123test.
      - ADMIN_PASSWORD=admin
    depends_on:
      - kuma
    ports:
      - "8000:8000"

volumes:
  uptime-kuma:
  api:
```

### In order for the example to work: You have to run kuma first then create your kuma username and password then re-run the compose file.

### Example CURL Script:

---

TOKEN=$(curl -X -L 'POST' -H 'Content-Type: application/x-www-form-urlencoded' --data 'username=admin&password=admin' http://127.0.0.1:8000/login/access-token/ | jq -r ".access_token")

```bash
curl -X GET \
  -H 'Accept: application/json' \
  -H "Authorization: Bearer ${TOKEN}" \
  http://127.0.0.1:8000/monitors/
```

---

## API Documentation

The API provides comprehensive endpoint documentation through interactive Swagger UI. After starting the application:

1. Navigate to `http://localhost:8000/docs`
2. You can test endpoints directly in the UI
3. Authorization is handled through the "Authorize" button at the top

### Common Endpoints

- `POST /login/access-token` - Get authentication token
- `GET /monitors/` - List all monitors
- `POST /monitors/` - Create a new monitor
- `GET /status-page` - Get status page information

For a complete list of endpoints, refer to the Swagger documentation or the endpoint images above.



## Related Projects

- [Uptime Kuma](https://github.com/nifetency/Uptime-Kuma-Web-API) 
