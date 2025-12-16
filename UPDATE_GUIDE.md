# n8n Update Guide

This guide walks you through updating your self-hosted n8n instance to the latest version.

## Current Versions (as of December 2024)

- **Stable**: 2.0.2
- **Beta**: 2.1.0

> **Note**: The `stable` version is recommended for production use. The `beta` version may be unstable.

---

## Updating n8n with Docker

### Option 1: Docker Compose (Recommended)

If you're running n8n with Docker Compose:

```bash
# Navigate to your n8n directory
cd /path/to/your/n8n

# Pull the latest image
docker compose pull

# Stop and remove the old container
docker compose down

# Start with the new image
docker compose up -d
```

### Option 2: Standalone Docker Container

If you're running n8n as a standalone Docker container:

```bash
# 1. Pull the latest stable image
docker pull docker.n8n.io/n8nio/n8n

# Or pull a specific version
docker pull docker.n8n.io/n8nio/n8n:2.0.2

# 2. Find your running container ID
docker ps -a

# 3. Stop the container
docker stop <container_id>

# 4. Remove the old container
docker rm <container_id>

# 5. Start a new container with the updated image
docker run -it --rm \
  --name n8n \
  -p 5678:5678 \
  -e GENERIC_TIMEZONE="America/Denver" \
  -e TZ="America/Denver" \
  -e N8N_ENFORCE_SETTINGS_FILE_PERMISSIONS=true \
  -e N8N_RUNNERS_ENABLED=true \
  -v n8n_data:/home/node/.n8n \
  docker.n8n.io/n8nio/n8n
```

> **Important**: Replace `America/Denver` with your timezone. See [timezone list](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones#List).

---

## Updating n8n with npm

If you installed n8n globally via npm:

```bash
# Update to the latest version
npm update -g n8n

# Or install a specific version
npm install -g n8n@2.0.2
```

---

## Pre-Update Checklist

1. **Backup your data**
   - If using SQLite: backup your `~/.n8n` directory
   - If using PostgreSQL: create a database backup

2. **Check the release notes**
   - Review [n8n releases](https://github.com/n8n-io/n8n/releases) for breaking changes
   - Pay attention to migration notes for major version updates

3. **Test in staging first** (if possible)
   - Run the new version in a test environment before production

---

## Post-Update Verification

After updating:

1. Access n8n at `http://localhost:5678` (or your configured URL)
2. Verify your workflows are intact
3. Test a few critical workflows
4. Check that credentials are still working

---

## Troubleshooting

### Container won't start after update

```bash
# Check container logs
docker logs n8n
```

### Data persistence issues

Ensure your volume is properly mounted:
```bash
docker volume inspect n8n_data
```

### Rolling back

If you need to revert to a previous version:

```bash
# Pull the specific older version
docker pull docker.n8n.io/n8nio/n8n:1.123.5

# Then restart with that image
```

---

## Resources

- [Official Docker Installation Docs](https://docs.n8n.io/hosting/installation/docker/)
- [n8n Releases](https://github.com/n8n-io/n8n/releases)
- [n8n Community Forum](https://community.n8n.io/)
- [n8n Hosting Repository](https://github.com/n8n-io/n8n-hosting) (Docker Compose examples)
