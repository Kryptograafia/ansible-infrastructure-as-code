# Backup SLA

## Coverage

We back up services that satisfy at least one of these criteria:
 - are primary source of truth for particular data
 - contain customer and/or client data
 - are not feasible (or very costly) to restore by other means

Services that are backed up:
 - InxfluxDB
 - MySQL
 - Grafana


## Schedule

InfluxDB backups are created every 24 hours; it takes up to 3 hours to create and store the backup.

MySQL backups are created every 12 hours; it takes up to 2 hours to create and store the backup.

Grafana backups are created every 24 hours; it takes up to 2 hours to create and store the backup.

All backups are started automatically by the backup automation service.

Backup RPO (recovery point objective) is:
 - 24 hours for InfluxDB
 - 12 hours for MySQL
 - 24 hours for Grafana


## Storage

MySQL and InfluxDB backups are uploaded to the backup server.

Grafana is mirrored to the internal Git server.

Backup data from both servers will be synchronized to encrypted AWS S3 bucket in future (work in progress).


## Retention

InfluxDB backups are stored for 30 days; _____ versions (recovery points) are available to restore.

MySQL backups are stored for 30 days; _____ versions are available to restore.

Grafana backups are stored for 7 days; _____ versions are available to restore.


## Usability checks

InfluxDB backups are verified every _____ by _____.

MySQL backups are verified every _____ by _____.

Grafana backups are verified every _____ by _____.


## Restore process

Service is recovered from the backup in case of an incident, and when service cannot be restored in any other way.

RTO (recovery time objective) is:
 - 2 hours for InfluxDB
 - 3 hours for MySQL
 - 2 hours for Grafana

Detailed backup restore procedure is documented in the [backup_restore.md](./backup_restore.md).
