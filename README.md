# automation-infra

A comprehensive Docker Compose setup for running n8n workflow automation platform with supporting services including PostgreSQL, Redis, and Evolution API.

## 🏗️ Architecture

This infrastructure setup provides a scalable and production-ready environment for n8n with the following components:

- **n8n API**: Main n8n application server
- **n8n Worker**: Background job processor for workflow executions
- **n8n Webhook**: Dedicated webhook handler for incoming requests
- **PostgreSQL**: Primary database for n8n data persistence
- **Redis**: Queue management and caching layer
- **Evolution API**: WhatsApp Business API integration

## 📋 Prerequisites

- Docker Engine 20.10+
- Docker Compose 2.0+
- At least 4GB RAM available
- Ports 5432, 6379, 5678, and 8080 available

## 🚀 Quick Start

1. **Clone the repository**
   ```bash
   git clone <[repository-url](https://github.com/gmorang/automation-infra)>
   cd n8n-infra
   ```

2. **Set up environment variables**
   
   Copy the example environment file and configure your variables:
   
   ```bash
   cp .env.example .env
   ```
   
   Edit the `.env` file with your specific configuration values. Make sure to:
   - Set strong passwords for PostgreSQL and Redis
   - Configure your encryption key for n8n
   - Update host and port settings as needed

3. **Start the services**
   ```bash
   docker-compose up -d
   ```

4. **Access n8n**
   
   Open your browser and navigate to `http://localhost:5678`

## 🔧 Service Configuration

### n8n Services

The n8n platform is deployed with three specialized services:

- **n8n_api**: Main application server handling the web interface and API
- **n8n_worker**: Processes workflow executions in the background
- **n8n_webhook**: Handles incoming webhook requests

### Database Services

- **PostgreSQL 17.4**: Primary database with health checks
- **Redis 8.0.1**: Queue management and caching with authentication

### External Services

- **Evolution API**: WhatsApp Business API integration for messaging workflows

## 📁 Project Structure

```
n8n-infra/
├── docker-compose.yml          # Main orchestration file
├── databases/
│   ├── postgres/
│   │   ├── docker-compose.yml  # PostgreSQL service definition
│   │   └── postgres.env        # PostgreSQL environment variables
│   └── redis/
│       ├── docker-compose.yml  # Redis service definition
│       └── redis.env           # Redis environment variables
└── services/
    ├── n8n/
    │   ├── docker-compose.yml  # n8n services definition
    │   └── n8n.env            # n8n environment variables
    └── evolution/
        ├── docker-compose.yml  # Evolution API service definition
        └── evolution.env       # Evolution API environment variables
```

## 🔒 Security Considerations

- Change all default passwords in the `.env` file
- Use strong encryption keys for `N8N_ENCRYPTION_KEY`
- Enable `N8N_SECURE_COOKIE` for production deployments
- Configure proper firewall rules for exposed ports
- Regularly update Docker images to latest versions

## 📊 Monitoring

All services include health checks:

- **n8n API**: HTTP health check on `/healthz`
- **PostgreSQL**: `pg_isready` command check
- **Redis**: `redis-cli ping` command check
- **Evolution API**: HTTP health check on `/health`

## 🛠️ Management Commands

### View logs
```bash
# All services
docker-compose logs -f

# Specific service
docker-compose logs -f n8n_api
```

### Restart services
```bash
# All services
docker-compose restart

# Specific service
docker-compose restart n8n_api
```

### Scale workers
```bash
docker-compose up -d --scale n8n_worker=3
```

### Backup database
```bash
docker-compose exec postgres pg_dump -U n8n n8n > backup.sql
```

## 🔄 Updates

To update n8n to the latest version:

1. Update `N8N_VERSION` in your `.env` file
2. Pull the new image: `docker-compose pull`
3. Restart services: `docker-compose up -d`

## 🐛 Troubleshooting

### Common Issues

1. **Port conflicts**: Ensure ports 5432, 6379, 5678, and 8080 are available
2. **Memory issues**: Increase Docker memory allocation to at least 4GB
3. **Database connection**: Verify PostgreSQL credentials in `.env` file
4. **Permission errors**: Ensure Docker has proper permissions for volume mounts

### Health Check Failures

If health checks fail:

```bash
# Check service status
docker-compose ps

# View detailed logs
docker-compose logs <service-name>

# Restart specific service
docker-compose restart <service-name>
```

## 📝 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Submit a pull request

## 📞 Support

For issues and questions:

- Check the [n8n documentation](https://docs.n8n.io/)
- Review Docker Compose logs for error details
- Open an issue in this repository

---

**Note**: This setup is designed for development and small-scale production use. For large-scale production deployments, consider additional security measures, load balancing, and monitoring solutions.
