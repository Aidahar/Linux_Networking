## 1.1 Сети и маски

Определить и записать в отчёт:


### адрес сети 192.167.38.54/13
  
  ![ip_address](screen/ip_addr.png)

### перевод маски 255.255.255.0 в префиксную и двоичную запись.

  ![mask](screen/mask.png)
  префиксная форма = 24

  двоичная запись 11111111.11111111.11111111.00000000

### /15 в обычную и двоичную. 

  ![mask_15](screen/mask_15.png)
  
  обычная = 255.254.0.0
  
  двоичная 11111111.11111110.00000000.00000000

### 11111111.11111111.11111111.11110000 в обычную и префиксную

  ipcalc не принимает адрес в двоичной форме, поэтому считаем ручками

  обычная = 255.255.255.240

  префиксная = 28

### 3) минимальный и максимальный хост в сети *12.167.38.4* при масках: */8* 
  ![ws-1-maxmin](screen/ws-1_maxmin.png)
  минимальный 12.0.0.1
  максимальный 12.255.255.254
### *11111111.11111111.00000000.00000000*
  ![ws-1-maxmin_2](screen/ws-1_maxmin_2.png)
  минимальный 12.167.0.1
  максимальный 12.167.255.254
### *255.255.254.0* 
  ![ws-1-maxmin_3](screen/ws-1_maxmin_3.png)
  минимальный 12.167.38.1
  максимальный 12.167.39.254
### */4*
  ![ws-1-maxmin_3](screen/ws-1_maxmin_4.png)
  минимальный 0.0.0.1
  максимальный 15.255.255.254

## 1.2 localhost
###  Определить и записать в отчёт, можно ли обратиться к приложению, работающему на localhost, со следующими IP: 

*194.34.23.100* - нельзя, *127.0.0.2* - можно, *127.1.0.1* - можно, *128.0.0.1* - нельзя.

## 1.3 Диапазоны и сегменты сетей

### 1. какие из перечисленных IP можно использовать в качестве публичного, а какие только в качестве частных: 
*10.0.0.45* - частный
*134.43.0.2* - публичный 
*192.168.4.2* - частный
*172.20.250.4* - частный
*172.0.2.1* - публичный
*192.172.0.1* - публичный
*172.68.0.2* - публичный
*172.16.255.255* - частный
*10.10.10.10* - частный
*192.169.168.1* - публичный
  ![public](screen/Public_1.png)
  ![public](screen/Public_2.png)
  ![public](screen/Public_3.png)
  ![public](screen/Public_4.png)
Частный диапазоны:
3 сегмента IP-адресов включают:
A: 10.0.0.0~10.255.255.255, 
B: 172.16.0.0~172.31.255.255
C: 192.168.0.0~192.168.255.255.

### 2. какие из перечисленных IP адресов шлюза возможны у сети *10.10.0.0/18*: 

*10.0.0.1*, *10.10.100.1* - нет 

*10.10.1.255*, *10.10.0.2*, *10.10.10.10* - да 

## Part 2. Статическая маршрутизация между двумя машинами

### Отобразить существющие интерфейсы:
#### Существующие интерфейсы ws1
  ![ws1_ip_a](screen/ws1_ip_a.png)

#### Существующие интерфейсы ws2
  ![ws2_ip_a](screen/ws2_ip_a.png)

### Описать сетевые интерфейсы:
#### ws1 - *192.168.100.10*, маска */16*
  ![ws1-etc](screen/ws1_netplan.png)

#### ws2 - *172.24.116.8*, маска */12*
  ![ws2-etc](screen/ws2_netplan.png)
### Выполнить команду `netplan apply` для перезапуска сервиса сети
  ![ws1-apply](screen/ws1_apply.png)
  ![ws2-apply](screen/ws2_apply.png)

## 2.1. Добавление статического маршрута вручную
  ![ws1-ip-r](screen/ws1_ip_r.png)
  ![ws2-ip-r](screen/ws2_ip_r.png)

## 2.2. Добавление статического маршрута с сохранением
  
  ![ws1-r_static](screen/ws1_r_static.png)
  ![ws2-r_static](screen/ws2_r_static.png)

### Пропинговать соединение между машинами

  ![ws1-static-ping](screen/ws1_static_ping.png)
  ![ws2-static-ping](screen/ws2_static_ping.png)

## Part 3. Утилита iperf3
### 3.1. Скорость соединения
 
8 Mbps = 1 MB/s 

100 MB/s = 800 000 Kbps

1 Gbps = 1 000 Mbps 

## 3.2. Утилита iperf3
### Измерить скорость соединения между ws1 и ws2:

  ![ws1_iperf3](screen/ws1_iperf3_serv.png)
  ![ws2_iperf3](screen/ws2_iperf3.png)
  ![ws1_iperf3](screen/ws1_iperf_client.png)
  ![ws2_iperf3](screen/ws2_iperf3_serv.png)

## Part 4. Сетевой экран
### 4.1. Утилита iptables

Создать файл /etc/firewall.sh, имитирующий фаерволл, на ws1 и ws2:
  ![ws1_iptables](screen/ws-1_iptables.png)
  ![ws2_iptables](screen/ws-2_iptables.png)

  #### Разница между подходами в том, что в случае запрещающего правила с начала есть возможность потерять удаленный доступ к серверу. (Применяется если есть консольный доступ к серверу).
  #### В случае разрешающего, правила новые правила необходимо добавлять в цепочку перед запрещающим правилом.

## 4.2. Утилита nmap

Командой ping найти машину, которая не "пингуется", после чего утилитой nmap показать, что хост машины запущен:
  ![ws1_nmap](screen/ws1_nmap.png)
  ![ws2_nmap](screen/ws2_nmap.png)
  ![ws1_nmap](screen/ws-1_nmap.png)

#### Сохраняем дампы
  ![ws1_damp](screen/ws1_damp.png)
  ![ws2_damp](screen/ws2_damp.png)

## Part 5. Статическая маршрутизация сети
### 5.1. Настройка адресов машин
#### Настроить конфигурации машин в etc/netplan/00-installer-config.yaml согласно сети на рисунке.
  ![ws11_netplan](screen/ws11_netplan.png)
  ![r1_netplan](screen/r1_netplan.png)
  ![r2_netplan](screen/r2_netplan.png)
  ![ws21_netplan](screen/ws21_netplan.png)
  ![ws22_netplan](screen/ws22_netplan.png)

### Перезапустить сервис сети. Если ошибок нет, то командой ip -4 a проверить, что адрес машины задан верно. Также пропинговать ws22 с ws21. Аналогично пропинговать r1 с ws11.
  ![ws11_ping](screen/ws11_ping.png)
  ![r1_ping](screen/r1_ping.png)
  ![r2_ping](screen/r2_ping.png)
  ![ws21_ping](screen/ws21_ping.png)
  ![ws22_ping](screen/ws22_ping.png)

## 5.2. Включение переадресации IP-адресов.
  ![r1_sysctl](screen/r1_sysctl.png)
  ![r2_sysctl](screen/r2_sysctl.png)
  ![r1_sysctl_conf](screen/r1_sysctl_conf.png)
  ![r2_sysctl_conf](screen/r2_sysctl_conf.png)

## 5.3. Установка маршрута по-умолчанию
  ![ws11_gateway](screen/ws11_ip_router.png)
  ![ws21_gateway](screen/ws21_ip_default.png)
  ![ws22_gateway](screen/ws22_ip_default.png)

#### Вызвать ip r и показать, что добавился маршрут в таблицу маршрутизации
  ![ws11_ip_r](screen/ws11_ip_r.png)
  ![ws21_ip_r](screen/ws21_ip_r.png)
  ![ws22_ip_r](screen/ws22_ip_r.png)
  ![r1_ip_r](screen/r1_ip_r.png)
  ![r2_ip_r](screen/r2_ip_r.png)
dhcpd.png
#### Пропинговать с ws11 роутер r2 и показать на r2, что пинг доходит. 
  ![ws11_tcpdump](screen/ws11_tcpdump.png)
  ![r2_tcpdump](screen/r2_tcpdump.png)

## 5.4. Добавление статических маршрутов

### Добавить в роутеры r1 и r2 статические маршруты в файле конфигураций. 
  ![r1_routes](screen/r1_routes.png)
  ![r2_routes](screen/r2_routes.png)

### Вызвать ip r и показать таблицы с маршрутами на обоих роутерах. 
  ![r1_ip_r](screen/r1_ip_r_routing.png)
  ![r2_ip_r](screen/r2_ip_r_routing.png)

### Запустить команды на ws11:
ip r list 10.10.0.0/[маска сети] и ip r list 0.0.0.0/0
  ![ws11_routing](screen/ws11_ip_routing.png)
  ![ws11_routing_zero](screen/ws11_ip_routing_zero.png)

IP 0.0.0.0 - немаршрутизируемый адрес ipv4, который используется как адрес по умолчанию или адрес-заполнитель. Приоритет сначала на заданный маршрут, если его нет то маршрутизация идет через 0.0.0.0

## 5.5. Построение списка маршрутизаторов

### Запустить на r1 команду дампа:
tcpdump -tnv -i eth0
  ![r1_tcpdump](screen/r1_tcpdump.png)

### Построить список маршрутизаторов на пути от ws11 до ws21:
  ![ws11_traceroute](screen/ws11_traceroute.png)
  ![r1_traceroute](screen/r1_tcpdump.png)

Программа трасероут отправляет по 3 UDP пакета с TTL=1  и смотрит адрес ответившего узла, и так дальше увеличивая TTL на единицу пока не достигнет конечного узла.

## 5.6. Использование протокола ICMP при маршрутизации

### Запустить на r1 перехват сетевого трафика, проходящего через eth0 с помощью команды:
tcpdump -n -i eth0 icmp

  ![r1_tcpdump_ping](screen/r1_tcpdump_icmp.png)

### Пропинговать с ws11 несуществующий IP (например, 10.30.0.111) с помощью команды:
ping -c 1 10.30.0.111

  ![ws11_ping_wrong](screen/ws11_ping_wrong.png)

#### Дампы
  ![ws11_damp](screen/ws11_damp_5.png)
  ![ws21_damp](screen/ws21_damp_5.png)
  ![ws22_damp](screen/ws22_damp_5.png)
  ![r1_damp](screen/r1_damp_5.png)
  ![r2_damp](screen/r2_damp_5.png)

## Part 6. Динамическая настройка IP с помощью DHCP
### Для r2 настроить в файле /etc/dhcp/dhcpd.conf конфигурацию службы DHCP:  
#### указать адрес маршрутизатора по-умолчанию, DNS-сервер и адрес внутренней сети.
  ![r2_dhcp_config](screen/r2_dhcp_conf.png)
#### Перезагрузить службу DHCP командой systemctl restart isc-dhcp-server.
  ![r2_dhcp_restart](screen/r2_dhcp_restart.png)

#### в файле resolv.conf прописать nameserver 8.8.8.8.
  
  ![r2_resolv](screen/r2_resolv.png)

#### Машину ws21 перезагрузить при помощи reboot и через ip a показать, что она получила адрес. 
  
  ![ws21_dhcp](screen/ws21_dhcp.png)
  ![ws22_dhcp](screen/ws22_dhcp.png)

#### Также пропинговать ws22 с ws21.
  
  ![ws21_ping](screen/ws21_ping_ws22.png)

## Установить DHCP на машину r1

  ![r1_macaddress](screen/r1_macaddress.png)
  ![r1_resolv](screen/r1_resolv_conf.png)

#### Указать MAC адрес у ws11

  ![ws11_macaddress](screen/ws11_macaddress.png)

#### Показать адрес до

  ![ws11_befor_static](screen/ws11_before_static.png)

#### Показать после
  
  ![ws11_static_ip](screen/ws11_static_mac_ip.png)

#### Получить айпи по DHCP

  ![ws21_dhclient](screen/ws21_dhclient.png)

#### Команды которыми пользовался: sudo dhclient -r сбрасывает текущий айпи, вызов команды sudo dhclient делает обращение к серверу и получает айпи.

#### Дампы
  ![ws11_damp_6](screen/ws11_damp_6.png)
  ![ws21_damp_6](screen/ws21_damp_6.png)
  ![ws22_damp_6](screen/ws22_damp_6.png)
  ![r1_damp_6](screen/r1_damp_6.png)
  ![r2_damp_6](screen/r2_damp_6.png)

## Part 7. NAT
#### r1 изменяем конфиг
  
  ![r1_apache_conf](screen/r1_apache_conf.png)

#### ws22 изменяем конфиг 
  
  ![ws22_apache_conf](screen/ws22_apache2_conf.png)

#### r1 запуск сервера

  ![r1_apache_start](screen/r1_apache2_start.png)

#### ws22 запуск сервера

  ![ws22_apache_start](screen/ws22_apache2_start.png)

#### Правила для iptables2

  ![r2_appache_iptables](screen/r2_firewall_apache2.png)

#### Пропинговать ws22 с r1

  ![r1_ping_ws22](screen/r1_ping_ws22.png)

#### Добавить новое правило для ICMP
  ![r2_nat_rules](screen/r2_nat_rules.png)
  ![r2_firewall_icmp](screen/r2_firewall_icmp.png)

#### Пропинговать ws22 с r1

  ![r1_ping_icmp](screen/r1_ping_ws22_icmp.png)

#### Добавить еще одно правило разрешающее маршрутизацию пакетов ICMP

  ![r2_nat-New_rule](screen/r2_nat_new_rule.png)

#### проверить соединение между r1 и ws22

  ![r1_nat_ping](screen/r1_nat_ping_ws22.png)

#### добавить еще два правила для SNAT и DNAT

  ![r2_nat_last_rules](screen/r2_nat_last_rule.png)

##### Проверить соединение по TCP для **SNAT**, для этого с ws22 подключиться к серверу Apache на r1 командой:
  ![ws22_telnet](screen/ws22_telnet.png)

##### Проверить соединение по TCP для **DNAT**, для этого с r1 подключиться к серверу Apache на ws22 командой `telnet` (обращаться по адресу r2 и порту 8080)

  ![r1_telnet](screen/r1_telnet.png)

#### Дампы
  ![ws11_nat_damp](screen/ws11_nat_damp.png)
  ![ws21_nat_damp](screen/ws21_nat_damp.png)
  ![ws22_nat_damp](screen/ws22_nat_damp.png)
  ![r1_nat_damp](screen/r1_nat_damp.png)
  ![r2_nat_damp](screen/r2_nat_damp.png)

## Part 8. Дополнительно. Знакомство с **SSH Tunnels**

##### Запустить на r2 фаервол с правилами из Части 7
  ![r2_firewall_8](screen/r2_firewall_8.png)

##### Запустить веб-сервер **Apache** на ws22 только на localhost (то есть в файле */etc/apache2/ports.conf* изменить строку `Listen 80` на `Listen localhost:80`)
  
  ![ws22_localhost](screen/ws22_localhost.png)

##### Воспользоваться *Local TCP forwarding* с ws21 до ws22, чтобы получить доступ к веб-серверу на ws22 с ws21

  ![ws21_ssh](screen/ws21_sshd.png)

##### Открываем второй терминал на машине ws21 логинимся и проверяем телнетом

  ![ws21_telnet](screen/ws21_tcpforwarding.png)

##### Воспользоваться *Remote TCP forwarding* c ws11 до ws22, чтобы получить доступ к веб-серверу на ws22 с ws11:

  ![ws11_tcpforwarding](screen/ws11_tcpforwarding.png)
  ![ws11_telnet](screen/ws11_telnet.png)

#####  Описать команды которыми пользовался:  
Для *Local TCP forwarding*: ssh -L [local_port](выбираем порт):localhost:[local_port](наш порт 80) [remote_ip](10.20.0.20)

Для *Remote TCP forwarding*: ssh -R [remote_port](выбираем порт):localhost:[local_port](наш порт 80) [remote_ip](10.20.0.20)

второй терминал открывается командой alt+F2 логинимся и телнетом проверяем соединение

#### Дампы
  ![ws11_damp_8](screen/ws11_damp_8.png)
  ![ws21_damp_8](screen/ws21_damp_8.png)
  ![ws22_damp_8](screen/ws22_damp_8.png)
  ![r1_damp_8](screen/r1_damp_8.png)
  ![r2_damp_8](screen/r2_damp_8.png)