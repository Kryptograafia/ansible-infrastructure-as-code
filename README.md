# Infrastructure as Code Project

## Overview

This is an Ansible-based infrastructure automation project that deploys a complete web application stack with monitoring, load balancing, and high availability features. The infrastructure is designed for the domain `kryptograafia.io` and includes multiple services distributed across three virtual machines.

## Architecture

The project deploys a multi-tier architecture with the following components:

### Infrastructure Components

- **3 Virtual Machines** (Kryptograafia-1, Kryptograafia-2, Kryptograafia-3)
- **Load Balancer**: HAProxy with Keepalived for high availability
- **Web Application**: Agama (Python-based web app) running in Docker containers
- **Database**: MySQL with master-slave replication
- **DNS**: BIND9 with primary/secondary configuration
- **Monitoring Stack**: Prometheus, Grafana, and InfluxDB
- **Web Server**: Nginx for static content
- **Backup System**: Automated backups using Duplicity

### Service Distribution

| Service | Kryptograafia-1 | Kryptograafia-2 | Kryptograafia-3 |
|---------|-----------------|-----------------|-----------------|
| Agama Web App | ✅ | ✅ | ❌ |
| Agama Client | ❌ | ❌ | ✅ |
| MySQL Database | ✅ | ✅ | ❌ |
| HAProxy | ✅ | ✅ | ❌ |
| Keepalived | ✅ | ✅ | ❌ |
| BIND9 DNS | ✅ | ✅ | ✅ |
| Prometheus | ❌ | ❌ | ✅ |
| Grafana | ❌ | ❌ | ✅ |
| InfluxDB | ❌ | ❌ | ✅ |
| Nginx | ✅ | ✅ | ✅ |

## Key Features

### High Availability
- **Load Balancing**: HAProxy distributes traffic across multiple Agama instances
- **Failover**: Keepalived provides automatic failover between HAProxy instances
- **Database Replication**: MySQL master-slave replication for data redundancy
- **DNS Redundancy**: Primary and secondary DNS servers

### Monitoring & Observability
- **Metrics Collection**: Prometheus scrapes metrics from various services
- **Visualization**: Grafana dashboards for system monitoring
- **Time Series Data**: InfluxDB stores system metrics and logs
- **Log Aggregation**: Centralized logging with Telegraf

### Backup & Recovery
- **Automated Backups**: Daily incremental and weekly full backups
- **Multiple Databases**: MySQL and InfluxDB backup coverage
- **RPO**: 24-hour recovery point objective
- **RTO**: 2-hour recovery time objective
- **30-day Retention**: Backups retained for 30 days

### Security
- **Encrypted Secrets**: Ansible Vault for sensitive configuration
- **Network Segmentation**: Restricted network access
- **Service Isolation**: Docker containers for application isolation

## Prerequisites

- Ansible 2.9+
- SSH access to target servers
- Python 3.x on target servers
- Docker (installed by Ansible)

## Quick Start

1. **Clone the repository**:
   ```bash
   git clone <repository-url>
   cd ica0002
   ```

2. **Configure inventory**:
   - Update `hosts` file with your server details
   - Ensure SSH access is configured

3. **Deploy infrastructure**:
   ```bash
   ansible-playbook infra.yaml
   ```

4. **Run pre-exam setup** (if needed):
   ```bash
   ./pre-exam.sh
   ```

## Configuration

### Variables
- **Global Variables**: `group_vars/all.yaml` contains all configuration
- **Encrypted Secrets**: Passwords and keys are stored in Ansible Vault
- **Network Configuration**: IP ranges and domain settings

### Key Configuration Files
- `infra.yaml`: Main playbook defining the deployment order
- `hosts`: Inventory file with server definitions
- `ansible.cfg`: Ansible configuration

## Services

### Agama Web Application
- **Purpose**: Main web application
- **Technology**: Python Flask application
- **Deployment**: Docker containers
- **Ports**: 8001-8003 (multiple instances)
- **Database**: MySQL backend

### Agama Client
- **Purpose**: Client application for system management
- **Location**: Kryptograafia-3
- **Service**: Systemd service

### MySQL Database
- **Purpose**: Primary data storage
- **Replication**: Master-slave setup
- **Backup**: Automated daily backups
- **Monitoring**: MySQL exporter for Prometheus

### HAProxy & Keepalived
- **Purpose**: Load balancing and high availability
- **Failover**: Automatic failover between instances
- **Health Checks**: Service health monitoring

### Monitoring Stack
- **Prometheus**: Metrics collection and storage
- **Grafana**: Dashboard visualization (port 3001)
- **InfluxDB**: Time series data storage
- **Telegraf**: Metrics collection agent

### DNS (BIND9)
- **Purpose**: Domain name resolution
- **Configuration**: Primary/secondary setup
- **Zone**: kryptograafia.io
- **Security**: TSIG keys for zone transfers

## Backup & Restore

### Backup Schedule
- **InfluxDB**: Daily at 21:15 UTC (incremental), Sunday 21:15 UTC (full)
- **MySQL**: Daily at 21:45 UTC (incremental), Sunday 21:45 UTC (full)

### Restore Procedures
Detailed restore procedures are documented in `backup_restore.md`:
- MySQL database restoration
- InfluxDB data restoration
- Verification procedures

## Monitoring Queries

Common Prometheus queries for monitoring:
- Memory consumption: `node_memory_MemTotal_bytes - node_memory_MemAvailable_bytes`
- CPU load: `avg_over_time(node_load1[5m])`

## Maintenance

### Regular Tasks
- Monitor backup success/failure
- Check service health via Grafana dashboards
- Review system logs for errors
- Update Ansible playbooks as needed

### Troubleshooting
- Check service status: `systemctl status <service-name>`
- View logs: `journalctl -u <service-name>`
- Verify DNS: `dig @<dns-server> kryptograafia.io`
- Test load balancer: `curl -H "Host: www.kryptograafia.io" <haproxy-ip>`

## Security Considerations

- All sensitive data is encrypted with Ansible Vault
- Network access is restricted to specific IP ranges
- Services run with minimal required privileges
- Regular security updates should be applied

## Contributing

When modifying the infrastructure:
1. Test changes in a staging environment
2. Update documentation
3. Ensure backup procedures remain functional
4. Verify monitoring continues to work
---

For detailed backup and restore procedures, see `backup_restore.md`.
For backup SLA information, see `backup_sla.md`.
