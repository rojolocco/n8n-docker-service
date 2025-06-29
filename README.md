# n8n Docker Service

This repository contains a Docker Compose configuration for running [n8n](https://n8n.io/), a powerful workflow automation tool that allows you to connect various services and automate tasks without writing code.

## Overview

n8n (pronounced n-eight-n) is a free and open fair-code licensed workflow automation tool. With a node-based approach, you can build, configure, and execute workflows that integrate with various third-party services.

This setup:

- Uses the official n8n Docker image
- Persists data using Docker volumes
- Connects to a PostgreSQL database
- Runs behind a reverse proxy (Caddy)
- Includes health checks and resource limits

## Prerequisites

- [Docker](https://docs.docker.com/get-docker/) and [Docker Compose](https://docs.docker.com/compose/install/)
- PostgreSQL database (configured separately or using Docker)
- Caddy or another reverse proxy for HTTPS (optional but recommended for production)

## Setup Instructions

### 1. Clone this repository

```bash
git clone https://github.com/yourusername/n8n-docker-service.git
cd n8n-docker-service
```

### 2. Configure environment variables

Copy the example environment file and edit it with your settings:

```bash
cp .env.example .env
```

Edit the `.env` file and set:

- PostgreSQL connection details
- n8n authentication settings
- Encryption keys (use strong, unique values)
- Webhook URL (your public domain for n8n)

### 3. Start the service

```bash
docker-compose up -d
```

This will start n8n in detached mode. The service will automatically restart if it crashes or if the server reboots.

## Configuration Options

### Environment Variables

The following environment variables can be configured in the `.env` file:

#### Database Configuration

- `POSTGRES_USER`: PostgreSQL username
- `POSTGRES_PASSWORD`: PostgreSQL password
- `POSTGRES_DB`: PostgreSQL database name
- `DB_TYPE`: Database type (default: postgresdb)
- `DB_POSTGRESDB_HOST`: PostgreSQL host
- `DB_POSTGRESDB_PORT`: PostgreSQL port
- `DB_POSTGRESDB_DATABASE`: PostgreSQL database name
- `DB_POSTGRESDB_USER`: PostgreSQL username
- `DB_POSTGRESDB_PASSWORD`: PostgreSQL password

#### n8n Configuration

- `N8N_BASIC_AUTH_ACTIVE`: Enable basic authentication (true/false)
- `N8N_ENFORCE_SETTINGS_FILE_PERMISSIONS`: Enforce permissions on settings file
- `N8N_RUNNERS_ENABLED`: Enable workflow runners
- `N8N_ENCRYPTION_KEY`: Encryption key for sensitive data
- `N8N_USER_MANAGEMENT_JWT_SECRET`: JWT secret for user management
- `WEBHOOK_URL`: Public URL for n8n webhooks

### Resource Limits

The service is configured with the following resource limits:

- Memory: 1GB
- CPU: 1 core

You can adjust these limits in the `docker-compose.yaml` file if needed.

## Accessing n8n

Once the service is running, you can access n8n at:

- If using a reverse proxy: <https://your-domain.com>
- If running locally without a proxy: <http://localhost:5678>

## Maintenance

### Viewing Logs

```bash
docker-compose logs -f n8n
```

### Updating n8n

To update to the latest version of n8n:

1. Update the image version in `docker-compose.yaml`
2. Pull the new image and restart the service:

```bash
docker-compose pull
docker-compose up -d
```

### Backup and Restore

n8n data is stored in the `n8n_data` Docker volume. To back up this data:

```bash
docker run --rm -v n8n_data:/data -v $(pwd):/backup alpine tar -czf /backup/n8n-backup.tar.gz -C /data .
```

To restore from a backup:

```bash
docker run --rm -v n8n_data:/data -v $(pwd):/backup alpine sh -c "rm -rf /data/* && tar -xzf /backup/n8n-backup.tar.gz -C /data"
```

## Troubleshooting

### Health Checks

The service includes a health check that verifies n8n is running properly. If the service is unhealthy:

1. Check the logs: `docker-compose logs -f n8n`
2. Verify the database connection
3. Check if the service has enough resources

### Common Issues

- **Database Connection Errors**: Verify PostgreSQL credentials and ensure the database is accessible
- **Webhook Issues**: Ensure your `WEBHOOK_URL` is correctly set and publicly accessible
- **Permission Issues**: Check that the Docker volumes have the correct permissions

## License

This project is licensed under the terms of the [LICENSE](LICENSE) file included in this repository.

## Additional Resources

- [n8n Documentation](https://docs.n8n.io/)
- [n8n Community Forum](https://community.n8n.io/)
- [n8n GitHub Repository](https://github.com/n8n-io/n8n)
