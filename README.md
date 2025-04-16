# Monitoring System Setup

This repository contains a complete monitoring system setup using Docker Compose, including Prometheus, Grafana, Alertmanager, Node Exporter, Blackbox Exporter, and a test Nginx service.

## Components

### Core Monitoring Components
- **Prometheus**: Metrics collection and storage
- **Grafana**: Visualization and dashboards
- **Alertmanager**: Alert handling and notifications
- **Node Exporter**: System metrics collection
- **Blackbox Exporter**: HTTP endpoint monitoring
- **Nginx**: Test service for monitoring

### Alert Integration
- **Slack Integration**: Real-time alerts in Slack channels
- **Telegram Integration**: Real-time alerts in Telegram groups

## Prerequisites

- Docker
- Docker Compose
- Slack workspace and webhook URL
- Telegram bot token and chat ID

## System Architecture

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│  Prometheus │◄────┤ Node Exporter     │
└──────┬──────┘     └─────────────┘     └─────────────┘
       │
       ▼
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│ Alertmanager│◄────┤ Blackbox    │◄────┤   Nginx     │
└──────┬──────┘     │ Exporter    │     │  Service    │
       │            └─────────────┘     └─────────────┘
       │
       ▼
┌─────────────┐     ┌─────────────┐
│   Slack     │     │  Telegram   │
└─────────────┘     └─────────────┘
```

## Setup Instructions

1. Clone this repository
2. Configure alert integrations:
   - Update Slack webhook URL in `alertmanager/config.yml`
   - Telegram bot token and chat ID are already configured
3. Start all services:
```bash
docker-compose up -d
```

## Service Access Points

| Service | URL | Port | Description |
|---------|-----|------|-------------|
| Prometheus | http://localhost:9090 | 9090 | Metrics and alerts |
| Grafana | http://localhost:3000 | 3000 | Dashboards |
| Alertmanager | http://localhost:9093 | 9093 | Alert management |
| Node Exporter | http://localhost:9100 | 9100 | System metrics |
| Blackbox Exporter | http://localhost:9115 | 9115 | HTTP probes |
| Nginx Service | http://localhost:8080 | 8080 | Test endpoints |

## Nginx Test Endpoints

The Nginx service includes the following test endpoints:
- `/` - Main page
- `/health` - Health check endpoint (returns 200)
- `/error` - Error endpoint (returns 502)

## Monitoring Features

### System Monitoring
- CPU usage monitoring
- Memory usage tracking
- Disk space monitoring
- Network metrics collection
- System load monitoring

### API Monitoring
- HTTP status code monitoring
- Response time tracking
- Service availability checks
- SSL certificate monitoring
- Custom endpoint monitoring

### Alert Configuration
- High CPU usage (>80%)
- High memory usage (>85%)
- High disk usage (>85%)
- HTTP probe failures
- Non-200 HTTP status codes
- Service availability alerts

## Alert Channels

### Slack Integration
- Channel: #alerts
- Alert format includes:
  - Alert name and description
  - Status and severity
  - Instance details
  - Timestamp
  - Color-coded messages

### Telegram Integration
- Group chat notifications
- Emoji-enhanced messages
- Markdown formatting
- Real-time alerts
- Resolved notifications

## Grafana Dashboards

Recommended dashboards to import:
1. Node Exporter Full (ID: 1860)
   - System metrics visualization
   - Resource usage trends
   - Performance indicators

2. Blackbox Exporter (ID: 7587)
   - HTTP endpoint monitoring
   - Response time analysis
   - Availability metrics

## Maintenance

### Starting Services
```bash
docker-compose up -d
```

### Stopping Services
```bash
docker-compose down
```

### Checking Logs
```bash
docker-compose logs -f [service_name]
```

### Updating Configuration
1. Edit configuration files
2. Restart affected services:
```bash
docker-compose restart [service_name]
```

## Troubleshooting

### Common Issues

1. **Prometheus Targets**
   - Check: http://localhost:9090/targets
   - Verify all targets are UP
   - Check scrape configurations

2. **Alertmanager Status**
   - Check: http://localhost:9093/#/status
   - Verify alert routing
   - Check notification channels

3. **Nginx Endpoints**
   - Test health endpoint: http://localhost:8080/health
   - Test error endpoint: http://localhost:8080/error
   - Verify response codes

4. **Grafana Access**
   - Default credentials: admin/admin
   - Check datasource connections
   - Verify dashboard imports

### Log Inspection
```bash
# Check specific service logs
docker-compose logs -f prometheus
docker-compose logs -f alertmanager
docker-compose logs -f nginx

# Check all services
docker-compose logs -f
```

## Security Considerations

1. **Access Control**
   - Change default Grafana credentials
   - Secure Prometheus endpoints
   - Protect Alertmanager access

2. **Network Security**
   - Use internal Docker network
   - Limit exposed ports
   - Implement firewall rules

3. **Alert Configuration**
   - Secure webhook URLs
   - Protect bot tokens
   - Use secure channels

## Support

For issues and support:
1. Check the troubleshooting section
2. Review service logs
3. Verify configuration files
4. Test individual components 