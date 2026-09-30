# High-Availability Database Infrastructure

A hands-on infrastructure lab for designing and validating a MariaDB primary-replica environment with infrastructure monitoring, automated backups, replication health checks, and controlled failure recovery testing.

The project focuses on **Linux infrastructure operations, database replication, observability, operational automation, and failure recovery validation** rather than fully automated database failover.

---

## Project Overview

This project was built to gain practical experience in operating a redundant database infrastructure and validating its behavior under controlled failure conditions.

The environment consists of dedicated database and monitoring servers connected through an internal VMware network.

### Key Objectives

* Build and configure a MariaDB primary-replica architecture
* Monitor database and host availability using Prometheus and Grafana
* Implement automated database backups using Bash and cron
* Develop replication health checks for operational validation
* Simulate infrastructure failures and validate recovery behavior
* Verify replication reconnection and synchronization after recovery
* Document deployment, monitoring, testing, and troubleshooting procedures

---

# Architecture

The environment consists of dedicated database and monitoring servers connected through an internal VMware network.

| Component               | Role                             |
| ----------------------- | -------------------------------- |
| Router                  | NAT and internal network routing |
| Primary Database Server | MariaDB write node               |
| Replica Database Server | MariaDB replication node         |
| Monitoring Server       | Prometheus and Grafana           |

## Architecture Diagram

![Architecture Diagram](docs/architecture-diagram.png)

---

# Core Features

* MariaDB primary-replica replication
* Replication synchronization validation
* Replication health checks
* Prometheus-based infrastructure monitoring
* Grafana dashboard visualization
* Grafana alert rule configuration
* Automated MariaDB database backups
* Cron-based backup scheduling
* Bash-based operational automation
* Controlled infrastructure failure testing
* Database service recovery validation
* VMware-based internal network configuration

---

# Monitoring and Observability

Prometheus and Grafana were integrated to provide infrastructure-level monitoring, visualization, and alerting.

## Monitoring Metrics

The monitoring environment tracks infrastructure metrics including:

* CPU utilization
* Memory utilization
* Disk utilization
* Network traffic
* Database server availability
* Node availability

## Dashboard

![Grafana Dashboard](docs/screenshots/grafana-dashboard-full.png)

## Alert Rules

![Grafana Alert Rules](docs/screenshots/grafana-dashboard-alert-rules.png)

---

# Database Replication

MariaDB primary-replica replication was configured to provide database redundancy and validate replication behavior during controlled infrastructure failures.

## Replication Configuration

* MariaDB Primary configured as the write node
* MariaDB Replica configured as the replication node
* Internal network connectivity validated
* Replication synchronization verified
* Replication status monitored through a custom health check script

## Healthy Replication State

![Replication Normal](docs/screenshots/replication-normal.png)

## Replication Failure State

![Replication Failed](docs/screenshots/replication-failed.png)

---

# Failure Detection and Alerting

Grafana alert rules were configured to detect infrastructure-level failures when a monitored node becomes unavailable.

## Alert Scenario

The following controlled failure scenario was used to validate monitoring and alert recovery:

1. Stop the Node Exporter service on the Primary Database Server
2. Prometheus detects the monitored target as unavailable
3. Grafana alert enters the `Pending` state
4. The alert transitions to the `Firing` state
5. Restoring Node Exporter returns the alert to the `Normal` state

## Alert Firing State

![Alert Firing](docs/screenshots/alert-firing.png)

## Alert History

![Alert History](docs/screenshots/alert-history-detail.png)

## Alert Recovery

![Alert Recovery](docs/screenshots/alert-recovered.png)

---

# Operational Automation

Operational tasks were automated using Bash shell scripts and cron.

## Automated Database Backup

A scheduled MariaDB backup process was implemented using Bash and cron.

### Backup Capabilities

* Automated MariaDB database dumps
* Cron-based scheduled execution
* Backup verification
* Script-based operational management
* Backup restoration testing

Detailed procedures are documented in:

* [`backup-automation.md`](docs/backup-automation.md)
* [`backup-restore.md`](docs/backup-restore.md)
* [`cron-backup.md`](docs/cron-backup.md)

---

## Replication Health Check

A custom health check script was implemented to validate the operational state of MariaDB replication.

### Health Check

The script validates replication status including:

* `Slave_IO_Running`
* `Slave_SQL_Running`
* Replication state
* Replication operational status

The health check provides a simple operational validation mechanism for identifying replication issues.

> The `Slave_*` field names are MariaDB replication status fields and are retained as-is.

---

# Failure and Recovery Testing

Controlled failure scenarios were performed to validate monitoring, database recovery, and replication behavior.

## Primary Database Service Recovery Test

The Primary database service was intentionally interrupted to observe replication behavior and verify recovery after service restoration.

### Test Flow

```text
Primary Database
      │
      │ MariaDB service stopped
      ▼
Replication connection lost
      │
      ▼
Prometheus detects infrastructure failure
      │
      ▼
Grafana alert triggered
      │
      │ MariaDB service restored
      ▼
Replication reconnects
      │
      ▼
Replica synchronization verified
```

The test validates:

* Replication connection loss detection
* Database service recovery
* Replication reconnection
* Replica synchronization after recovery
* Monitoring alert recovery

> **Scope:** This project validates replication recovery and monitoring behavior. It does **not** implement automatic Replica promotion, automatic role switching, or fully automated database failover.

Detailed test procedures are documented in:

* [`failure-recovery-test.md`](docs/failure-recovery-test.md)
* [`replication-test.md`](docs/replication-test.md)

---

# Backup and Restore Validation

Database backup and restoration procedures were tested as part of the operational workflow.

The validation covered:

* MariaDB database dump generation
* Scheduled backup execution
* Backup file verification
* Database restoration
* Restored data verification
* MariaDB service availability after restoration

Detailed procedures:

* [`backup-automation.md`](docs/backup-automation.md)
* [`backup-restore.md`](docs/backup-restore.md)
* [`cron-backup.md`](docs/cron-backup.md)

---

# Validation Results

| Validation Area                        | Result |
| -------------------------------------- | ------ |
| MariaDB Primary-Replica Replication    | Passed |
| Replication Synchronization            | Passed |
| Replication Health Validation          | Passed |
| Prometheus Target Failure Detection    | Passed |
| Grafana Alert Triggering               | Passed |
| Grafana Alert Recovery                 | Passed |
| Primary Database Service Recovery      | Passed |
| Replication Reconnection               | Passed |
| Replica Synchronization After Recovery | Passed |
| Automated Database Backup              | Passed |
| Database Restore Validation            | Passed |

These tests were performed as controlled infrastructure-lab scenarios and are intended to demonstrate operational understanding rather than production-grade HA certification.

---

# Technologies

| Category         | Technologies                             |
| ---------------- | ---------------------------------------- |
| Operating System | Ubuntu Server 24.04                      |
| Database         | MariaDB                                  |
| Monitoring       | Prometheus, Grafana, Node Exporter       |
| Virtualization   | VMware Workstation                       |
| Automation       | Bash, Cron                               |
| Networking       | Linux Networking, VMware Virtual Network |

---

# Project Structure

```text
ha-database-infrastructure/
├── database/
│   └── ...
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
├── gns3/
│   └── ...
├── monitoring/
│   └── ...
├── scripts/
│   └── ...
├── storage/
│   └── ...
├── vmware/
│   └── ...
├── .gitattributes
└── README.md
```

---

# Project Scope

This project focuses on the following infrastructure capabilities:

**Linux Infrastructure → Database Replication → Monitoring → Alerting → Backup → Failure Testing → Recovery Validation**

The project intentionally does not claim automatic database failover or production-grade high-availability orchestration.

The primary objective is to demonstrate practical experience with infrastructure deployment, operational monitoring, failure diagnosis, backup procedures, and recovery validation.
