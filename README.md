## 👋 Welcome to autorestic 🚀

High-level backup tool, a configuration wrapper around restic

## 📋 Description

autorestic is a CLI wrapper around restic that drives backups from a single
`.autorestic.yml` file. This stack runs `autorestic cron`, which executes the
backups on the schedules defined in that file.

## 🚀 Services

- **autorestic**: cupcakearmy/autorestic:latest

## 📦 Installation

### Option 1: Quick Install
```bash
curl -q -LSsf "https://raw.githubusercontent.com/composemgr/autorestic/main/docker-compose.yaml" -o compose.yml
```

### Option 2: Git Clone
```bash
git clone "https://github.com/composemgr/autorestic" ~/.local/srv/docker/autorestic
cd ~/.local/srv/docker/autorestic
cp default.env .env
docker compose --env-file .env up -d
```

Create `./volumes/config/autorestic/.autorestic.yml` before starting; the
container exits until the file exists.

### Option 3: Using composemgr
```bash
composemgr install autorestic
```

## 🔧 Configuration

### Environment Variables

```shell
TZ=America/New_York
```

See `docker-compose.yaml` for the complete list of configurable options.

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
| `docker-compose.yaml` | Default, no web port - CLI tool only |
| `docker-compose.traefik.yaml` | Behind an existing external `traefik` network |
| `docker-compose.tunnel.yaml` | Behind an existing external `cloudflare` tunnel network |

```bash
docker compose -f docker-compose.traefik.yaml --env-file .env up -d
```

## 🌐 Access

autorestic is a CLI tool, not a web application. It publishes no port and has no
web interface.

```bash
docker compose --env-file .env exec autorestic autorestic -c /root/.autorestic/.autorestic.yml check
```

## 📂 Volumes

- `./volumes/config/autorestic` - autorestic configuration (`.autorestic.yml`)
- `./volumes/data/autorestic` - Data to back up, mounted at `/backup`

## 🔐 Security

- Change all default passwords before deploying to production
- Use strong secrets for all repository passwords in `.autorestic.yml`
- Regularly update Docker images for security patches
- Backup your data regularly

## 🔍 Logging

```shell
docker compose --env-file .env logs -f autorestic
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
```

## 📋 Requirements

- Docker Engine 20.10+
- Docker Compose V2+

## 🤝 Author

🤖 casjay: [Github](https://github.com/casjay) 🤖  
🦄 composemgr: [Github](https://github.com/composemgr) 🦄
