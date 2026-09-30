# Failover Scenario Test

## Overview

This document describes the failover and recovery test performed on the MariaDB replication environment.

The objective was to verify:
- Replication failure detection
- Infrastructure monitoring alerts
- Replication recovery process

---

## Test Environment

Components:
- DB Master
- DB Slave
- Prometheus
- Grafana

---

## Failure Simulation

MariaDB service was stopped on the master server.

### Command

```bash
sudo systemctl stop mariadb
```

---

## Monitoring Result

Grafana alert status changed:

```text
Normal → Pending → Alerting
```

Prometheus detected the target failure successfully.

---

## Replication Status During Failure

Replication status check:

```sql
SHOW SLAVE STATUS\G
```

Observed result:

```text
Slave_IO_Running: Connecting
Slave_SQL_Running: Yes
Last_IO_Error: Connection refused
```

This confirmed that the slave server lost connectivity to the master database.

---

## Recovery Process

MariaDB service restored on master server.

### Command

```bash
sudo systemctl start mariadb
```

Replication restarted successfully.

---

## Recovery Verification

Final replication status:

```text
Slave_IO_Running: Yes
Slave_SQL_Running: Yes
Seconds_Behind_Master: 0
```

Grafana alert status returned to:

```text
Normal
```

---

## Result

The failover test successfully verified:
- Infrastructure failure detection
- Prometheus monitoring functionality
- Grafana alerting
- Replication recovery process
- Database synchronization restoration
