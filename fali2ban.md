# 1. Учимся работать с fali2ban
1. Устанавливаем
```sh
[root@localhost]# yum install epel-release
[root@localhost]# yum update
[root@localhost]# yum install fail2ban fail2ban-systemd
```

2. Делаем локальный конфиг
```sh
[root@localhost]# cp -pf /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
```

3. Конфигурируем
```
[DEFAULT]
ignoreip = 127.0.0.1/8
ignorecommand =
bantime = 3600
findtime = 600
maxretry = 5

# Override /etc/fail2ban/jail.d/00-firewalld.conf:
banaction = iptables-multiport

[nginx-http-auth]
enabled = true
```

`ignoreip` — используется для установки списка IP-адресов, которые не будут забанены. Список IP-адресов следует указывать через пробел.
`bantime` — время блокировки, в секундах.
`maxretry` — количество попыток перед перед блокировкой.
`findtime` — время, на протяжении которого рассчитывается количество попыток перед баном (maxretry).

Т.е. в конфиге по-умолчанию прописано, что пользователь будет забанен на 10 минут, если в течение 10 минут будет совершено 5 неудачных попыток.

4. Защита SSH соединения
Cоздаем файл для защиты ssh:
```sh
[root@localhost]# nano /etc/fail2ban/jail.d/sshd.local
```

В него пишем:
```
[sshd]
enabled = true
port = ssh
action = firewallcmd-ipset
logpath = %(sshd_log)s
maxretry = 5
bantime = 86400
```

`enable` = true — проверка ssh активна.
`action` используется для получения IP-адреса, который необходимо заблокировать, используя фильтр, доступный в /etc/fail2ban/action.d/firewallcmd-ipset.conf.
`logpath` — путь, где хранится файл журнала. Этот файл журнала сканируется Fail2Ban.
`maxretry` — лимита для неудачных входов в систему.
`bantime` — время блокировки (24 часа).


5. Добавляем в автозапуск
```sh
[root@localhost]# systemctl enable fail2ban
[root@localhost]# systemctl start fail2ban
```

6. Проверяем статус
```sh
[root@localhost]# fail2ban-client status
Status
|- Number of jail: 1
`- Jail list: sshd
```

7. РАЗБЛОКИРОВКА адреса
```sh
[root@localhost]# fail2ban-client set sshd unbanip IPADDRESS
```
