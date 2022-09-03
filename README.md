#25-homeWork. 34 - LDAP. Централизованная авторизация и аутентификация  
## Описание файлов в директории
logFileFull.log - полный лог выполнения  

Vagrant_folder - все что понадобится для поднятия ВМ и краткое описание файлов в ней  
Vagrantfile - вагрант файл  
provisioning - директория с файлами провижинга  
```
provisioning/
├── playbook.yml
└── roles
    ├── client
    └── server
```

## Описание как запустить виртуальную машину (кратко)
```
vagrant up
vagrant ssh client
[vagrant@ipaclient ~]$ sudo -i
[root@ipaclient ~]$ echo '123qweasd' | ssh admin@localhost
Пароль admin 123qweasd
```

## кратко о playbook
### server
Меняем имя ВМ
```
- name: Set a hostname
  hostname:
    name: freeipa.otus.local
    use: systemd
```
Устанавливаем ipa server передавая в параметрах нужные данные
```
- name: install ipa server
  shell:
    cmd: 'ipa-server-install -p 12345678 -a 123qweasd --domain="otus.local" --realm="OTUS.LOCAL" --hostname="freeipa.otus.local" --setup-dns --no-forwarders --no-reverse --mkhomedir --unattended'
```

### client
Меняем имя ВМ
```
- name: Set a hostname
  hostname:
    name: ipaclient.otus.local
    use: systemd
```
Устанавливаем ipa client передавая в параметрах нужные данные. И получаем билет от kerberos. Проверить выданные билет можно через klist.  
```
- name: install ipa-client
  shell:
    cmd: 'ipa-client-install --domain="otus.local" -p admin -w 123qweasd --mkhomedir --server="freeipa.otus.local" --realm="OTUS.LOCAL" --hostname="ipaclient.otus.local" --unattended'

- name: kinit
  shell:
    cmd: 'echo "123qweasd" | kinit admin'
```


📚Домашнее задание/проектная работа разработано(-на) для курса ["Administrator Linux. Professional"](https://otus.ru/lessons/linux-professional/)