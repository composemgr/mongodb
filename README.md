## 👋 Welcome to mongodb 🚀

MongoDB - NoSQL document database

## 📋 Description

MongoDB is a source-available cross-platform document-oriented database program. Classified as a NoSQL database, MongoDB uses JSON-like documents with optional schemas.

## 🚀 Services

- **db**: MongoDB 7 (`mongo:7`)

## 📦 Installation

### Using curl
```shell
curl -q -LSsf "https://raw.githubusercontent.com/composemgr/mongodb/main/docker-compose.yaml" | docker compose -f - up -d
```

### Using git
```shell
git clone "https://github.com/composemgr/mongodb" ~/.local/srv/docker/mongodb
cd ~/.local/srv/docker/mongodb
docker compose up -d
```

### Using composemgr
```shell
composemgr install mongodb
```

## 🔧 Configuration

### Environment Variables

```shell
TZ=America/New_York
MONGO_INITDB_ROOT_USERNAME=admin
MONGO_INITDB_ROOT_PASSWORD=changeme_admin_password
MONGO_INITDB_DATABASE=admin
```

## 🌐 Access

- **MongoDB**: localhost:27017 (from Docker host)
- **Connection String**: `mongodb://admin:password@localhost:27017/admin`

## 📂 Volumes

- `./rootfs/db/mongodb/mongodb` - Database files

## 🔐 Security

- Enable authentication (enabled by default)
- Create database-specific users
- Use role-based access control
- Enable TLS for production
- Regular security updates

### Create Database User
```javascript
use myapp
db.createUser({
  user: "myapp",
  pwd: "secure_password",
  roles: [{ role: "readWrite", db: "myapp" }]
})
```

## 🔍 Logging

```shell
docker compose logs -f db
```

## 🛠️ Management

### Connect to Mongo shell
```shell
docker compose exec db mongosh -u admin -p password
```

### Common Commands
```javascript
// Show databases
show dbs

// Use database
use myapp

// Show collections
show collections

// Find documents
db.collection.find()
```

## 🔄 Backup & Restore

### Backup
```shell
docker compose exec db mongodump -u admin -p password --out=/dump
docker compose cp db:/dump ./backup-$(date +%Y%m%d)
```

### Restore
```shell
docker compose cp ./backup-YYYYMMDD db:/dump
docker compose exec db mongorestore -u admin -p password /dump
```

## 📋 Requirements

- Docker Engine 20.10+
- Docker Compose V2+
- 1GB+ RAM recommended

## 🤝 Author

🤖 casjay: [Github](https://github.com/casjay) 🤖  
🦄 composemgr: [Github](https://github.com/composemgr) 🦄
