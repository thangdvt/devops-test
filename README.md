# Monitoring System with Prometheus, Grafana, and Alertmanager

This project sets up a complete monitoring system using Prometheus, Grafana, and Alertmanager with integrations for Slack and Telegram notifications.

## Table of Contents
- [System Components](#system-components)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
  - [Prometheus Configuration](#prometheus-configuration)
  - [Grafana Configuration](#grafana-configuration)
  - [Alertmanager Configuration](#alertmanager-configuration)
- [Slack Integration](#slack-integration)
- [Telegram Integration](#telegram-integration)
- [Usage](#usage)
- [Troubleshooting](#troubleshooting)

## System Components

- **Prometheus**: Time-series database and monitoring system
- **Grafana**: Visualization and dashboard platform
- **Alertmanager**: Handles alerts from Prometheus and routes them to different receivers
- **Node Exporter**: Collects system metrics
- **Blackbox Exporter**: Probes endpoints over HTTP, HTTPS, DNS, TCP, and ICMP

## Prerequisites

- Docker and Docker Compose installed
- Access to Slack workspace (for Slack integration)
- Telegram bot token (for Telegram integration)

## Installation

1. Clone the repository:
```bash
git clone https://github.com/thangdvt/devops-test.git
cd devops-test
```

2. Start the services:
```bash
docker-compose up -d
```

3. Verify the services are running:
```bash
docker-compose ps
```

## Configuration

### Prometheus Configuration

The Prometheus configuration is located in `prometheus/prometheus.yml`. It includes:
- Global scrape intervals
- Alertmanager configuration
- Job configurations for Node Exporter and Blackbox Exporter

### Grafana Configuration

Grafana is configured with:
- Pre-configured dashboards for Node Exporter and Blackbox Exporter
- Prometheus as the default datasource
- Alerting rules

### Alertmanager Configuration

Alertmanager configuration is in `alertmanager/alertmanager.yml`. It includes:
- Global settings
- Route configuration
- Receiver configurations for Slack and Telegram

## Slack Integration

### Creating Slack Webhook
url guide: https://api.slack.com/messaging/webhooks

1. Go to your Slack workspace
2. Click on your workspace name → Settings & administration → Manage apps
3. Search for "Incoming Webhooks" and click "Add to Slack"
4. Choose a channel for notifications
5. Copy the Webhook URL

### Configuring Alertmanager for Slack

Add the following to `alertmanager/alertmanager.yml`:

```yaml
receivers:
  - name: 'slack'
    slack_configs:
      - api_url: 'YOUR_SLACK_WEBHOOK_URL'
        channel: '#alerts'
        send_resolved: true
        title: '{{ template "slack.default.title" . }}'
        text: '{{ template "slack.default.text" . }}'
```

## Telegram Integration

### Creating Telegram Bot
url for telegram bot: https://core.telegram.org/bots/tutorial

1. Open Telegram and search for "@BotFather"
2. Start a chat and send `/newbot`
3. Follow the instructions to create your bot
4. Copy the bot token provided by BotFather

### Getting Chat ID

1. Start a chat with your bot
2. Send a message to the bot
3. Visit `https://api.telegram.org/bot<YOUR_BOT_TOKEN>/getUpdates`
4. Look for the "chat" object and note the "id" value

### Configuring Alertmanager for Telegram

Add the following to `alertmanager/alertmanager.yml`:

```yaml
receivers:
  - name: 'telegram'
    telegram_configs:
      - bot_token: 'YOUR_BOT_TOKEN'
        chat_id: YOUR_CHAT_ID
        send_resolved: true
        message: '{{ template "telegram.default.message" . }}'
```

## Usage

### Accessing Services

- **Grafana**: http://localhost:3000
  - Default credentials: admin/admin
- **Prometheus**: http://localhost:9090
- **Alertmanager**: http://localhost:9093
- **Node Exporter**: http://localhost:9100
- **Blackbox Exporter**: http://localhost:9115

### Viewing Metrics

1. Log in to Grafana
2. Navigate to Dashboards
3. Select either:
   - Node Exporter Dashboard (ID: 11074)
   - Blackbox Exporter Dashboard (ID: 13659)
4. when any issue or alert will send to chanel in slack though webhook and group chat in telegram though API.
![image](https://github.com/user-attachments/assets/bff0c67b-6b87-43f9-bcfc-d76625b45847)
![image](https://github.com/user-attachments/assets/8f679cbf-ec55-4537-9890-d43d8c1da112)
### Managing Alerts

1. Access Alertmanager at http://localhost:9093
2. View active alerts and their status
3. Configure silence periods if needed
## Troubleshooting

### Common Issues

1. **Services not starting**
   - Check Docker logs: `docker-compose logs`
   - Verify port availability
   - Check configuration files for syntax errors

2. **No data in Grafana**
   - Verify Prometheus is scraping targets
   - Check datasource configuration in Grafana
   - Ensure metrics are being collected

3. **Alerts not working**
   - Verify Alertmanager configuration
   - Check webhook URLs and tokens
   - Test alert rules in Prometheus

### Logs

View logs for specific services:
```bash
docker-compose logs prometheus
docker-compose logs grafana
docker-compose logs alertmanager
```

### Restarting Services

To restart all services:
```bash
docker-compose restart
```

To restart a specific service:
```bash
docker-compose restart <service-name>
``` 
