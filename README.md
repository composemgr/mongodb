## 👋 Welcome to mongodb 🚀

Flexible and scalable document-oriented database

## 📋 Description

MongoDB is a document database that stores JSON-like documents. This stack
ships a single `mongo` service with authentication enabled and its data
directory on a bind mount.

Unlike the other stacks in this repo, MongoDB has no web interface. The
default file publishes the MongoDB wire protocol on its native `27017` port so
other containers or a client on the host can reach it; the traefik and tunnel
variants publish nothing, because there is no HTTP surface to route.

## 🚀 Services

This stack has no web service. The only service is the database itself:

- **db**: mongo:latest (MongoDB, published on `27017`)

### Infrastructure Components

- **db**: MongoDB database

## 📦 Installation

### Option 1: Quick Install
```bash
curl -q -LSsf "https://raw.githubusercontent.com/composemgr/mongodb/main/docker-compose.yaml" -o compose.yml
```

### Option 2: Git Clone
```bash
git clone "https://github.com/composemgr/mongodb" ~/.local/srv/docker/mongodb
cd ~/.local/srv/docker/mongodb
cp default.env .env
docker compose --env-file .env up -d
```

### Option 3: Using composemgr
```bash
composemgr install mongodb
```

## 🔧 Configuration

### Environment Variables

```shell
TZ=America/New_York
BASE_DOMAIN_NAME=example.com
BASE_HOST_NAME=mongodb.example.com
DB_ADMIN_NAME=administrator
DB_ADMIN_PASS=replace-with-a-random-password
```

See `docker-compose.yaml` for complete list of configurable options.

### Environment Files

`default.env.sample` holds the full set of variables and is copied to `.env`
before starting the stack:

```bash
cp default.env .env
docker compose --env-file .env up -d
```

`app.env.sample` holds the app-specific overrides and is copied to `app.env`:

```bash
cp app.env.sample app.env
```

`composemgr up` applies `app.env` and `default.env` automatically. Raw
`docker compose` commands need `--env-file .env` to pick up the same values.

### Reverse Proxy

Three standalone compose files ship in this repo:

| File | Use |
|------|-----|
| `docker-compose.yaml` | Default, MongoDB published on `27017` |
| `docker-compose.traefik.yaml` | Internal only, no published port |
| `docker-compose.tunnel.yaml` | Internal only, no published port |

```bash
docker compose -f docker-compose.traefik.yaml --env-file .env up -d
```

The traefik and tunnel variants carry no route or cloudflare labels,
because the MongoDB wire protocol is not HTTP and cannot be proxied that
way. They exist so the database can be started with no host port at all,
for stacks where another container reaches it over the shared network.

## 🌐 Access

There is no web interface. MongoDB is reached over its wire protocol:

- **MongoDB**: `172.17.0.1:27017` (default)

```bash
mongosh "mongodb://${DB_ADMIN_NAME}:${DB_ADMIN_PASS}@172.17.0.1:27017/admin"
```

As an infrastructure project, MongoDB publishes its native `27017` port on
the host; the assigned `64259` slot is not used.

## 📂 Volumes

- `./volumes/db/mongodb/mongodb` - Database data

## 🔐 Security

- Change `DB_ADMIN_PASS` before the first start. The values are only applied
  on an empty data directory, so changing them later requires editing the
  `admin.system.users` collection directly
- The port is published on the Docker host IP only, not on every interface,
  but MongoDB is unencrypted, so treat the host as a trusted boundary
- Prefer the traefik or tunnel variant for deployments where no host port is
  needed at all
- Configure TLS and replica-set authentication before using this outside a
  private network
- Regularly update Docker images for security patches
- Backup your data regularly

## 🔍 Logging

```shell
docker compose --env-file .env logs -f db
```

## 🛠️ Management

```bash
# Start services
docker compose --env-file .env up -d

# Stop services
docker compose --env-file .env down

# Update to latest images
docker compose --env-file .env pull && docker compose --env-file .env up -d

# View logs
docker compose --env-file .env logs -f

# Restart services
docker compose --env-file .env restart

# Open a shell
docker compose --env-file .env exec db mongosh
```

## 📋 Requirements

- Docker Engine 20.10+
- Docker Compose V2+

## 🤝 Author

🤖 casjay: [Github](https://github.com/casjay) 🤖  
🦄 composemgr: [Github](https://github.com/composemgr) 🦄
