#How to restore MySQL backups
#Steps:
1.) Install and configure infrastructure with Ansible:

    ansible-playbook infra.yaml

2.) Download the backup, run the command as the backup user (--force is used to overwrite the directory if there is already a backup in that directory)

    sudo -u backup duplicity --no-encryption --force restore rsync://Kryptograafia@backup.kryptograafia.io./mysql /home/backup/restore/mysql

3.) Restore MySQL data from the downloaded backup as user root:

    sudo mysql agama < /home/backup/restore/mysql/agama.sql

#How to check if the backup is correct
1.) Enter the MySQL client
    sudo mysql

2.) Select the database
    use agama;

3.) Select all items from the database (works in our lab environment as we dont have a lot of data, would not be the case in a  real business)
    select * from item;

#From the table look if the data contained in there is the data from the backup (you can compare to /home/backup/mysql/agama.sql)

#How to restore InfluxDB backups
#Steps:
1.) Install and configure infrastructure with Ansible:

    ansible-playbook infra.yaml

2.) Download the backup, run the command as the backup user (--force is used to overwrite the directory if there is already a backup in that directory)

    sudo -u backup duplicity --no-encryption --force restore rsync://Kryptograafia@backup.kryptograafia.io./influxdb /home/backup/restore/influxdb
 
    sudo service telegraf stop

    sudo influx -execute 'DROP DATABASE telegraf'
 
    sudo influxd restore -portable -db telegraf /home/backup/restore/influxdb
