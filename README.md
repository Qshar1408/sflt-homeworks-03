# Домашнее задание к занятию 3 «Резервное копирование»
## Грибанов Антон. FOPS-31

### Задание 1
- Составьте команду rsync, которая позволяет создавать зеркальную копию домашней директории пользователя в директорию `/tmp/backup`
- Необходимо исключить из синхронизации все директории, начинающиеся с точки (скрытые)
- Необходимо сделать так, чтобы rsync подсчитывал хэш-суммы для всех файлов, даже если их время модификации и размер идентичны в источнике и приемнике.
- На проверку направить скриншот с командой и результатом ее выполнения

#### Команда для бэкапа:
```bash
rsync -a --checksum --verbose --delete --progress --exclude '.*' /home/qshar/ /tmp/backup
```
![Rsync_001](https://github.com/Qshar1408/sflt-homeworks-03/blob/main/img/sflt03_001.png)
![Rsync_002](https://github.com/Qshar1408/sflt-homeworks-03/blob/main/img/sflt03_002.png)
![Rsync_003](https://github.com/Qshar1408/sflt-homeworks-03/blob/main/img/sflt03_003.png)
![Rsync_004](https://github.com/Qshar1408/sflt-homeworks-03/blob/main/img/sflt03_004.png)
![Rsync_005](https://github.com/Qshar1408/sflt-homeworks-03/blob/main/img/sflt03_005.png)

### Задание 2
- Написать скрипт и настроить задачу на регулярное резервное копирование домашней директории пользователя с помощью rsync и cron.
- Резервная копия должна быть полностью зеркальной
- Резервная копия должна создаваться раз в день, в системном логе должна появляться запись об успешном или неуспешном выполнении операции
- Резервная копия размещается локально, в директории `/tmp/backup`
- На проверку направить файл crontab и скриншот с результатом работы утилиты.

#### Файл crontab: [crontab](https://github.com/Qshar1408/sflt-homeworks-03/blob/main/files/crontab)
```bash
# Edit this file to introduce tasks to be run by cron.
# 
# Each task to run has to be defined through a single line
# indicating with different fields when the task will be run
# and what command to run for the task
# 
# To define the time you can provide concrete values for
# minute (m), hour (h), day of month (dom), month (mon),
# and day of week (dow) or use '*' in these fields (for 'any').
# 
# Notice that tasks will be started based on the cron's system
# daemon's notion of time and timezones.
# 
# Output of the crontab jobs (including errors) is sent through
# email to the user the crontab file belongs to (unless redirected).
# 
# For example, you can run a backup of all your user accounts
# at 5 a.m every week with:
# 0 5 * * 1 tar -zcf /var/backups/home.tgz /home/
# 
# For more information see the manual pages of crontab(5) and cron(8)
# 
# m h  dom mon dow   command
30 12 * * * /bin/bash /home/qshar/every_day_backup_gribanov.sh /home/qshar/ /tmp/backup

```

#### Файл crontab: [every_day_backup_gribanov.sh](https://github.com/Qshar1408/sflt-homeworks-03/blob/main/files/every_day_backup_gribanov.sh)
```bash
#!/bin/bash
copy=$1
paste=$2

if [ ! -d $copy ]; then
	echo -e "\033[31m----- | $(date +"%Y-%m-%d | %H:%M:%S") | Бэкап выполнен неудачно! | Исходный каталог не существует. | -----\033[0m" >> /var/log/rsync-cron-$(date +"%Y-%m-%d").log
elif [ ! -d $paste ]; then
	echo -e "\033[31m----- | $(date +"%Y-%m-%d | %H:%M:%S") | Бэкап выполнен неудачно! | Целевой каталог не существует. | -----\033[0m" >> /var/log/rsync-cron-$(date +"%Y-%m-%d").log
else
	rsync -a --checksum --verbose --delete --progress --exclude '.*' $copy $paste >> /var/log/rsync-cron-$(date +"%Y-%m-%d").log
        echo -e "\033[32m----- | $(date +"%Y-%m-%d | %H:%M:%S") | Бэкап выполнен удачно! | -----\033[0m" >> /var/log/rsync-cron-$(date +"%Y-%m-%d").log
fi

```

![Rsync_006](https://github.com/Qshar1408/sflt-homeworks-03/blob/main/img/sflt03_006.png)
![Rsync_007](https://github.com/Qshar1408/sflt-homeworks-03/blob/main/img/sflt03_007.png)

## Проверка работоспособности скрипта:

![Rsync_008](https://github.com/Qshar1408/sflt-homeworks-03/blob/main/img/sflt03_008.png)
![Rsync_009](https://github.com/Qshar1408/sflt-homeworks-03/blob/main/img/sflt03_009.png)
![Rsync_010](https://github.com/Qshar1408/sflt-homeworks-03/blob/main/img/sflt03_010.png)
![Rsync_011](https://github.com/Qshar1408/sflt-homeworks-03/blob/main/img/sflt03_011.png)
![Rsync_012](https://github.com/Qshar1408/sflt-homeworks-03/blob/main/img/sflt03_012.png)
