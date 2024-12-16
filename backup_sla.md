# Backup SLA

## Coverage

We backup 2 services, where in each we backup 1 database:

- MySQL: "agama" database
- InfluxDB: "telegraf" database

## Schedule for backups

- InfluxDB FULL backups are created every Sunday at 21:35 EST
- InfluxDB INCREMENTAL backups are created every day from Monday to Saturday at 21:35 EST
It takes up to 2 hours to create and store the backup.

- MySQL FULL backups are created every Sunday at 22:00 EST
- MySQL INCREMENTAL backups are created every day from Monday to Saturday at 22:00 EST
It takes up to 2 hours to create and store the backup.

#Backup RPO (recovery point objective) is:
- 24 hours for InfluxDB
- 24 hours for MySQL

# Storage
MySQL and InfluxDB backups are uploaded to the backup server.

## Restore process
Service is recovered from the backup in case of an incident, and when service cannot be restored in any other way.

RTO (recovery time objective) is:
 - 2 hours for InfluxDB
 - 2 hours for MySQL

Detailed backup restore procedure is documented in (./backup_restore.md).
