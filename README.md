# High-Availability Database Infrastructure

A hands-on infrastructure lab for designing and validating a high-availability-oriented MariaDB environment using primary-replica replication, infrastructure monitoring, automated backups, health checks, and failure recovery testing.

## Key Objectives

* Build and configure a MariaDB primary-replica architecture
* Monitor database and host health using Prometheus and Grafana
* Implement automated database backups using Bash and cron
* Develop replication health checks for operational validation
* Simulate infrastructure failures and validate recovery behavior
* Document deployment, monitoring, testing, and troubleshooting procedures

---

# Architecture

The environment consists of dedicated database and monitoring servers connected through an internal VMware network.

* **Router** — NAT and internal network routing
* **Primary Database Server** — MariaDB write node
* **Replica Database Server** — MariaDB read replica
* **Monitoring Server** — Prometheus and Grafana

## Architecture Diagram

![Architecture Diagram](docs/architecture-diagram.png)

---

# Features

* MariaDB primary-replica replication
* Replication synchronization and health validation
* Prometheus-based infrastructure monitoring
* Grafana dashboard visualization
* Grafana alert rule configuration
* Automated MariaDB database backups
* Cron-based backup scheduling
* Replication health check automation
* Infrastructure failure and recovery testing
* VMware-based internal network configuration
* Bash-based operational automation

---

# Monitoring and Observability

Prometheus and Grafana were integrated to provide infrastructure visibility and alerting.

## Monitoring Metrics

The monitoring environment tracks infrastructure-level metrics including:

* CPU utilization
* Memory utilization
* Disk utilization
* Network traffic
* Database server availability
* Infrastructure health status

## Dashboard Preview

![Grafana Dashboard](docs/screenshots/grafana-dashboard-full.png)

## Alert Rules

![Grafana Alert Rules](docs/screenshots/grafana-dashboard-alert-rules.png)

---

# Database Replication

MariaDB primary-replica replication was configured to provide database redundancy and validate replication behavior during infrastructure failures.

## Replication Configuration

* MariaDB Primary configured as the write node
* MariaDB Replica configured as the replication node
* Internal network connectivity validated
* Replication synchronization verified
* Replication health monitored through custom scripts

## Healthy Replication State

![Replication Normal](docs/screenshots/replication-normal.png)

## Replication Failure State

![Replication Failed](docs/screenshots/replication-failed.png)

---

# Failure Detection and Alerting

Grafana alert rules were configured to detect infrastructure-level failures when a monitored node becomes unavailable.

## Alert Scenario

The following scenario was used to validate failure detection and alert recovery:

1. Stop the Node Exporter service on the Primary database server
2. Prometheus detects the monitored target as unavailable
3. Grafana alert enters the `Pending` state
4. The alert transitions to the `Firing` state
5. Restoring the service returns the alert to the `Normal` state

## Alert Firing State

![Alert Firing](docs/screenshots/alert-firing.png)

## Alert History

![Alert History](docs/screenshots/alert-history-detail.png)

## Alert Recovery

![Alert Recovery](docs/screenshots/alert-recovered.png)

---

# Infrastructure Automation

Operational tasks were automated using Bash shell scripts and cron.

## Automated Database Backup

A scheduled MariaDB backup process was implemented using Bash and cron.

### Backup Capabilities

* Automated MariaDB database dumps
* Cron-based scheduled execution
* Backup verification testing
* Script-based operational management

Detailed procedures are documented in:

* [`backup-automation.md`](docs/backup-automation.md)
* [`backup-restore.md`](docs/backup-restore.md)
* [`cron-backup.md`](docs/cron-backup.md)

---

## Replication Health Check

A custom health check script was implemented to validate the operational state of MariaDB replication.

### Health Check Capabilities

* `Slave_IO_Running` status verification
* `Slave_SQL_Running` status verification
* Replication state validation
* Operational health checking

The health check is used to identify replication issues and support infrastructure validation.

---

# Failure and Recovery Testing

Controlled failure scenarios were performed to validate the behavior of the database and monitoring environment.

## Replication Recovery Test

The Primary database service was intentionally interrupted to observe replication behavior and verify recovery after service restoration.

The test validates:

* Replication connection loss detection
* Database service recovery
* Replication reconnection
* Replica synchronization after recovery

> **Note:** This project validates replication recovery rather than implementing automatic Replica promotion or fully automated database failover.

Detailed test procedures are documented in:

* [`failover-test.md`](docs/failover-test.md)
* [`replication-test.md`](docs/replication-test.md)

---

# Technologies

| Category         | Technologies                             |
| ---------------- | ---------------------------------------- |
| Operating System | Ubuntu Server 24.04                      |
| Database         | MariaDB                                  |
| Monitoring       | Prometheus, Grafana                      |
| Virtualization   | VMware Workstation                       |
| Automation       | Bash, Cron                               |
| Networking       | Linux Networking, VMware Virtual Network |

---

# Project Structure

```text
ha-database-infrastructure/
├── database/
├── docs/
│   ├── screenshots/
│   │   ├── alert-firing.png
│   │   ├── alert-history-detail.png
│   │   ├── alert-recovered.png
│   │   ├── grafana-dashboard-alert-rules.png
│   │   ├── grafana-dashboard-full.png
│   │   ├── host-network-info.png
│   │   ├── replication-failed.png
│   │   └── replication-normal.png
│   ├── architecture-diagram.png
│   ├── alerting.md
│   ├── backup-automation.md
│   ├── backup-restore.md
│   ├── cron-backup.md
│   ├── failover-test.md
│   ├── master-setup.md
│   ├── monitoring-setup.md
│   ├── replication-test.md
│   └── slave-setup.md
├── moni
```
