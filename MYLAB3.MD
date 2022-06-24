# Лабораторная работ № 2   
# Настройка административных ролей  
## Представлена следующая топология  
![](topology.png)
## Таблица IP-адресации.  
![](table.png)
# Цели     
## Часть 1. Настройка Основных Параметров Устройства    
* Подключите сеть, как показано в топологии.    
* Настройте базовую IP-адресацию для маршрутизаторов и ПК.  
* Настройте маршрутизацию OSPF.  
* Настройка хостов ПК.  
* Проверьте подключение между хостами и маршрутизаторами.  
## Часть 2. Настройка административных ролей    
* Создайте несколько представлений ролей и предоставьте различные привилегии.    
* Проверка и сопоставление видов.    
## Теоретическая часть  
Маршрутизатор является важнейшим компонентом любой сети. Он управляет перемещением данных в сеть и из нее, а также между устройствами внутри сети. Особенно важно защитить сетевые маршрутизаторы, поскольку отказ маршрутизирующего устройства может сделать недоступными отдельные участки сети или всю сеть. Контроль доступа к маршрутизаторам и включение отчетов о маршрутизаторах имеют решающее значение для сетевой безопасности и должны быть частью всеобъемлющей политики безопасности.
В этой лабораторной работе вы создадите сеть с несколькими маршрутизаторами и настроите маршрутизаторы и хосты. Вы настроите административные роли с различными уровнями привилегий.
Примечание: Маршрутизаторы, используемые в практических лабораториях, - это Cisco 4221 с Cisco IOS XE версии 16.9.6 (изображение universalk9). Коммутаторы, используемые в лабораториях, - это Cisco Catalyst 2960+ с Cisco IOS версии 15.2 (7) (изображение lanbasek9). Можно использовать другие маршрутизаторы, коммутаторы и версии Cisco IOS. В зависимости от модели и версии Cisco IOS доступные команды и выдаваемый результат могут отличаться от того, что показано в лабораторных условиях. Обратитесь к Сводной таблице интерфейса маршрутизатора в конце лабораторной работы для получения правильных идентификаторов интерфейса.  
Примечание: Прежде чем начать, убедитесь, что маршрутизаторы и коммутаторы были удалены и не имеют конфигураций запуска.  
## Необходимые Ресурсы  
* 3 маршрутизатора (Cisco 4221 с универсальным образом Cisco XE версии 16.9.6 или сопоставимым с лицензией на пакет технологий безопасности)  
* 2 коммутатора (Cisco 2960+ с изображением Cisco IOS версии 15.2 (7) lanbasek9 или аналогичным)  
* 2 ПК (ОС Windows с установленной программой эмуляции терминала, такой как PuTTY или Tera Term)  
* Консольные кабели для настройки сетевых устройств Cisco  
* Кабели Ethernet , как показано в топологии  
# Выполнение лабораторной работы № 2    
## Часть 1. Настройка Основных Параметров Устройства    
В этой части настройте топологию сети и настройте основные параметры, такие как IP-адреса интерфейса.    
### Шаг 1: Подключите кабель к сети.    
Подсоедините устройства, как показано на схеме топологии, и при необходимости подключите кабель.    
### Шаг 2: Настройте основные параметры для каждого маршрутизатора.    
Откройте окно конфигурации    
a. Консоль в маршрутизаторе и включите привилегированный режим EXEC.    
Router> enable    
Router# configure terminal    
Configure host names as shown in the topology.    
R1(config)# hostname R1    
Configure interface IP addresses as shown in the IP Addressing Table.    
R1(config)# interface g0/0/0    
R1(config-if)# ip address 10.1.1.1 255.255.255.0    
R1(config-if)# no shutdown   

R1(config)# interface g0/0/1    
R1(config-if)# ip address 192.168.1.1 255.255.255.0    
R1(config-if)# no shutdown    
б. Чтобы маршрутизатор не пытался перевести неправильно введенные команды, как если бы они были именами хостов, отключите поиск DNS.  R1 показан здесь в качестве примера.      
R1(config)# no ip domain-lookup  
### Шаг 3: Настройте маршрутизацию OSPF на маршрутизаторах.    
a. Используйте команду ospf маршрутизатора в режиме глобальной конфигурации, чтобы включить OSPF на R1.    
R1(config)# router ospf 1    
b. Настройте сетевые инструкции для сетей на R1. Используйте идентификатор области, равный 0.    
R1(config-router)# network 192.168.1.0 0.0.0.255 area 0    
R1(config-router)# network 10.1.1.0 0.0.0.3 area 0    
c. Настройте OSPF на R2 и R3.    
R2(config)# router ospf 1    
R2(config-router)# network 10.1.1.0 0.0.0.3 area 0     
R2(config-router)# network 10.2.2.0 0.0.0.3 area 0     
R3(config)# router ospf 1    
R3(config-router)# network 10.2.2.0 0.0.0.3 area 0    
R3(config-router)# network 192.168.3.0 0.0.0.255 area 0    
d. Выполните команду passive-interface , чтобы изменить интерфейс G0/0/1 на R1 и R3 на пассивный.    
R1(config)# router ospf 1    
R1(config-router)# passive-interface g0/0/1    
R3(config)# router ospf 1    
R3(config-router)# passive-interface g0/0/1    
### Шаг 4: Проверьте соседей OSPF и информацию о маршруте.    
a. Выполните команду show ip ospf neighbor, чтобы убедиться, что каждый маршрутизатор перечисляет другие маршрутизаторы в сети в    качестве соседей.  
R1#show ip ospf neighbor  


Neighbor ID     Pri   State           Dead Time   Address         Interface   
10.2.2.2          1   FULL/BDR        00:00:30    10.1.1.2        GigabitEthernet0/0/1    

R2#show ip ospf neighbor   


Neighbor ID     Pri   State           Dead Time   Address         Interface  
192.168.1.1       1   FULL/DR         00:00:38    10.1.1.1        GigabitEthernet0/0/0  
192.168.3.1       1   FULL/DR         00:00:38    10.2.2.1        GigabitEthernet0/0/1   

R3#show ip ospf neighbor  


Neighbor ID     Pri   State           Dead Time   Address         Interface  
10.2.2.2          1   FULL/BDR        00:00:36    10.2.2.2        GigabitEthernet0/0/0  

b. Выполните команду show ip route , чтобы убедиться, что все сети отображаются в таблице маршрутизации на всех маршрутизаторах.    
R1#show ip route  
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP  
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area  
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2  
       E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP  
       i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area  
       * - candidate default, U - per-user static route, o - ODR  
       P - periodic downloaded static route  

Gateway of last resort is not set  

     10.0.0.0/8 is variably subnetted, 3 subnets, 2 masks  
C       10.1.1.0/30 is directly connected, GigabitEthernet0/0/1  
L       10.1.1.1/32 is directly connected, GigabitEthernet0/0/1  
O       10.2.2.0/30 [110/2] via 10.1.1.2, 00:37:17, GigabitEthernet0/0/1  
     192.168.1.0/24 is variably subnetted, 2 subnets, 2 masks  
C       192.168.1.0/24 is directly connected, GigabitEthernet0/0/0  
L       192.168.1.1/32 is directly connected, GigabitEthernet0/0/0  
O    192.168.3.0/24 [110/3] via 10.1.1.2, 00:37:17, GigabitEthernet0/0/1    
### Шаг 5: Настройте параметры IP-адреса хоста ПК.      
Настройте статический IP-адрес, маску подсети и шлюз по умолчанию для PCA и PCC, как показано в таблице IP-адресации.     
C:\>ipconfig   

FastEthernet0 Connection:(default port)    

   Connection-specific DNS Suffix..:     
   Link-local IPv6 Address.........: FE80::200:CFF:FE46:EE2E    
   IPv6 Address....................: ::    
   IPv4 Address....................: 192.168.1.3    
   Subnet Mask.....................: 255.255.255.0    
   Default Gateway.................: ::    
                                     192.168.1.1      
PC-C     
C:\>ipconfig          
FastEthernet0 Connection:(default port)           

   Connection-specific DNS Suffix..:         
   Link-local IPv6 Address.........: FE80::2D0:58FF:FEEE:866        
   IPv6 Address....................: ::        
   IPv4 Address....................: 192.168.3.3        
   Subnet Mask.....................: 255.255.255.0        
   Default Gateway.................: ::        
                                     192.168.3.1        
### Шаг 6: Проверьте подключение между ПC-A и PC-C.    
C:\>ping 192.168.3.3    

Pinging 192.168.3.3 with 32 bytes of data:   

Reply from 192.168.3.3: bytes=32 time<1ms TTL=125    
Reply from 192.168.3.3: bytes=32 time<1ms TTL=125   
Reply from 192.168.3.3: bytes=32 time<1ms TTL=125     
Reply from 192.168.3.3: bytes=32 time<1ms TTL=125     

Ping statistics for 192.168.3.3:    
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),    
Approximate round trip times in milli-seconds:    
    Minimum = 0ms, Maximum = 0ms, Average = 0ms      
a. Выполните поиск с R1 на R3.      
R1#ping 10.2.2.1    

Type escape sequence to abort.     
Sending 5, 100-byte ICMP Echos to 10.2.2.1, timeout is 2 seconds:    
!!!!!    
Success rate is 100 percent (5/5), round-trip min/avg/max = 0/0/0 ms    

Если запросы не завершились успешно, устраните неполадки в основных конфигурациях устройства, прежде чем продолжить.      
b. Выполните пинг с PC-A в локальной сети R1 на PCC в локальной сети R3.    
PC-A    
C:\>ping 192.168.1.1    

Pinging 192.168.1.1 with 32 bytes of data:    

Reply from 192.168.1.1: bytes=32 time<1ms TTL=255    
Reply from 192.168.1.1: bytes=32 time=21ms TTL=255    
Reply from 192.168.1.1: bytes=32 time<1ms TTL=255    
Reply from 192.168.1.1: bytes=32 time<1ms TTL=255    

Ping statistics for 192.168.1.1:    
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),    
Approximate round trip times in milli-seconds:    
    Minimum = 0ms, Maximum = 21ms, Average = 5ms        
PC-C    
C:\>ping 192.168.3.1    

Pinging 192.168.3.1 with 32 bytes of data:   

Reply from 192.168.3.1: bytes=32 time<1ms TTL=255    
Reply from 192.168.3.1: bytes=32 time<1ms TTL=255    
Reply from 192.168.3.1: bytes=32 time<1ms TTL=255    
Reply from 192.168.3.1: bytes=32 time<1ms TTL=255    

Ping statistics for 192.168.3.1:    
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),    
Approximate round trip times in milli-seconds:    
    Minimum = 0ms, Maximum = 0ms, Average = 0m     
        
Если запросы не завершились успешно, устраните неполадки в основных конфигурациях устройства, прежде чем продолжить.      
Примечание: Если вы можете выполнить пинг с ПК-A на ПК-C, вы продемонстрировали, что маршрутизация OSPF настроена и функционирует    правильно.     
Если вы не можете выполнить пинг, но интерфейсы устройств подключены, а IP-адреса указаны правильно, используйте show run, show ip ospf neighbor, и show ip route      
команды, помогающие выявить проблемы, связанные с протоколом маршрутизации.     

### Шаг 7: Сохраните базовую текущую конфигурацию для каждого маршрутизатора.      
Сохраните базовую текущую конфигурацию маршрутизаторов в виде текстовых файлов на вашем компьютере.      


Эти текстовые файлы можно использовать для восстановления конфигураций позже в лаборатории.      
Закрыть окно настройки      

## Часть 2. Настройка административных ролей      
В этой части лаборатории вы будете:      
* Создайте несколько административных ролей или представлений на маршрутизаторах R1 и R3.     
* Предоставьте каждому виду различные привилегии.      
* Проверьте и сравните виды.      
Функция доступа CLI на основе ролей позволяет сетевому администратору определять представления,       
которые представляют собой набор операционных команд и возможностей настройки,       
которые обеспечивают выборочный или частичный доступ к командам Cisco IOS EXEC и режима конфигурации (config).        
Представления ограничивают доступ пользователей к интерфейсу командной строки Cisco IOS и информации о конфигурации.       
Представление может определять, какие команды принимаются и какая информация о конфигурации отображается.      
Примечание: Выполняйте все задачи как на R1, так и на R3. Процедуры и выходные данные для R1 показаны здесь.      
Если администратор хочет настроить другое представление системы, система должна находиться в корневом представлении.      
Когда система находится в режиме root view, пользователь имеет те же права доступа, что и пользователь с привилегиями 15-го уровня,     
но пользователь root view также может настроить новое представление и добавлять или удалять команды из представления.       
Когда вы находитесь в представлении CLI, у вас есть доступ только к командам, которые были добавлены в это представление      пользователем корневого представления.      
### Шаг 1: Включите AAA на маршрутизаторе R1.    
Открыть окно конфигурации     
Чтобы определить представления, включите AAA на маршрутизаторе.      
R1# configure terminal     
R1(config)# aaa new-model      
### Шаг 2: Настройте пароль привилегированного режима EXEC.      
Для доступа к корневому представлению требуется пароль привилегированного режима EXEC. В этом примере используется пароль cisco12345.      
R1(config)# enable secret cisco12345       
R1# exit      
### Шаг 3: Включите корневой(root) просмотр.      
Используйте команду включить просмотр, чтобы включить корневой(root) просмотр.    
R1# enable view      
Password: cisco12345     

R1#%PARSER-6-VIEW_SWITCH: successfully set to view 'root'.    

### Шаг 4: Создайте представление admin 1, установите пароль и назначьте привилегии.      
a. Пользователь admin1 - это пользователь верхнего уровня ниже root, которому разрешен доступ к этому маршрутизатору.       
Он обладает наибольшим авторитетом. Пользователь admin1 может использовать все команды show, config и debug.      
Используйте следующую команду, чтобы создать представление admin 1 в корневом представлении.      
R1# configure terminal      
R1(config)# parser view admin1      
R1(config-view)#%PARSER-6-VIEW_CREATED: view 'admin1' successfully created.    

R1(config-view)#      
Примечание: Чтобы удалить представление, используйте команду no parser view viewname.      
b. Свяжите представление admin1 с зашифрованным паролем.     
R1(config-view)# secret admin1pass      
R1(config-view)#      
c. Просмотрите команды, которые можно настроить в представлении admin1. Использовать команды? команда для просмотра доступных команд.   
Ниже приведен частичный список доступных команд.     
R1(config-view)#commands ?    
  configure  Global configuration mode    
  exec       Exec mode    
  interface  Interface configuration mode    
  line       Line configuration mode    
  router     Router configuration mode    
  
Просмвтриваем список команд которые доступны в представлении admin1(после его создания)    
R1#?    
Exec commands:    
  disable     Turn off privileged commands    
  enable      Turn on privileged commands    
  exit        Exit from the EXEC    
  logout      Exit from the EXEC    
  

d. Добавьте все команды config, show и debug в представление admin 1, а затем выйдите из режима просмотра конфигурации.      
R1(config-view)# commands exec include all show      
R1(config-view)# commands exec include all config terminal      
R1(config-view)# commands exec include all debug      
R1(config-view)# end      
e. Проверьте представление admin1.      
  R1# enable view admin1      
  Password: admin1pass    
  
  R1#%PARSER-6-VIEW_SWITCH: successfully set to view 'admin1'.    
  
  R1# show parser view      
Current view is 'admin1'      
f. Изучите команды, доступные в представлении admin1.      
R1#?    
Exec commands:    
  configure   Enter configuration mode    
  debug       Debugging functions (see also 'undebug')    
  disable     Turn off privileged commands    
  enable      Turn on privileged commands    
  exit        Exit from the EXEC    
  logout      Exit from the EXEC    
  show        Show running system information    
Примечание: Может быть доступно больше команд EXEC, чем отображается. Это зависит от вашего устройства и используемого образа IOS.   
g. Изучите команды отображения, доступные в представлении admin1.    
R1#show ?    
  aaa                Show AAA values    
  access-lists       List access lists    
  arp                Arp table    
  cdp                CDP information    
  class-map          Show QoS Class Map    
  clock              Display the system clock    
  controllers        Interface controllers status    
  crypto             Encryption module    
  debugging          State of each debugging option    
  dhcp               Dynamic Host Configuration Protocol status    
  dot11              IEEE 802.11 show information    
  file               Show filesystem information    
  flash:             display information about flash: file system    
  flow               Flow information    
  frame-relay        Frame-Relay information    
  history            Display the session command history    
  hosts              IP domain-name, lookup style, nameservers, and host table    
  interfaces         Interface status and configuration    
  ip                 IP information    
  ipv6               IPv6 information   
  license            Show license information   
  line               TTY line information   
 --More--     
### Шаг 5: Создайте представление admin 2, установите пароль и назначьте привилегии.      
a. Пользователь admin2 является младшим администратором, проходящим обучение, которому разрешено просматривать все конфигурации, но     не разрешается настраивать маршрутизаторы или использовать команды отладки.      
b. Используйте команду включить просмотр, чтобы включить корневой просмотр, и введите секретный пароль включения cisco12345.      
R1# enable view    
Password: cisco12345     
  c. Используйте следующую команду для создания представления admin 2.     
  R1# configure terminal    
  R1(config)# parser view admin2     
  d. Свяжите представление admin2 с паролем.      
  R1(config-view)# secret admin2pass      
  e. Добавьте все команды отображения в представление, а затем выйдите из режима настройки представления.      
  R1(config-view)# commands exec include all show      
  R1(config-view)# end      
  f. Проверьте представление admin2.      
  R1# enable view admin2    
  Password: admin2pass    
  R1# show parser view    
  Current view is ‘admin2’   
  g. Изучите команды, доступные в представлении admin2.    
  R1#?  
Exec commands:  
  disable     Turn off privileged commands  
  enable      Turn on privileged commands  
  exit        Exit from the EXEC  
  logout      Exit from the EXEC  
  show        Show running system information   
Примечание: Может быть доступно больше команд EXEC, чем отображается. Это зависит от вашего устройства и используемого образа IOS.    
## Вопрос:      
##Чего не хватает в списке команд admin 2, который присутствует в командах admin 1?   

Команд конфигурирования и отладки.  

### Шаг 6: Создайте техническое представление, установите пароль и назначьте привилегии.    
a. Технический пользователь обычно устанавливает устройства конечного пользователя и кабели. Техническим пользователям разрешено     использовать только выбранные команды   показа.  
b. Используйте команду включить просмотр, чтобы включить корневой просмотр, и введите секретный пароль включения cisco12345.    
R1# enable view    
Password: cisco12345    
c. Используйте следующую команду для создания технического представления.    
R1(config)# parser view tech   

R1(config-view)#%PARSER-6-VIEW_CREATED: view 'tech' successfully created.  

d. Свяжите техническое представление с паролем.    
R1(config-view)# secret techpasswd    
e. Добавьте следующие команды отображения в представление, а затем выйдите из режима настройки представления.    
R1(config-view)# commands exec include show version    
R1(config-view)# commands exec include show interfaces    
R1(config-view)# commands exec include show ip interface brief   
R1(config-view)# commands exec include show parser view   
R1(config-view)# end   
f. Проверьте техническое представление.    
R1# enable view tech    
Password: techpasswd  

R1#%PARSER-6-VIEW_SWITCH: successfully set to view 'tech'.  

R1# show parser view  
Current view is ‘tech’   
g. Изучите команды, доступные в техническом представлении.    
R1#?  
Exec commands:  
  disable     Turn off privileged commands  
  enable      Turn on privileged commands  
  exit        Exit from the EXEC  
  logout      Exit from the EXEC  
  show        Show running system information  
Примечание: Может быть доступно больше команд EXEC, чем отображается. Это зависит от вашего устройства и используемого образа IOS.    
* h. Изучите команды отображения, доступные в техническом представлении.  *
R1#show ?  
  interfaces         Interface status and configuration  
  ip                 IP information  
  parser             Show parser commands  
  version            System hardware and software status  
Примечание: Может быть доступно больше команд EXEC, чем отображается. Это зависит от вашего устройства и используемого образа IOS.  
*i. Выполните краткую команду показать ip-интерфейс.*  
R1#show ip interface  
GigabitEthernet0/0/0 is up, line protocol is up (connected)  
  Internet address is 192.168.1.1/24  
  Broadcast address is 255.255.255.255  
  Address determined by setup command  
  MTU is 1500 bytes  
  Helper address is not set  
  Directed broadcast forwarding is disabled  
  Outgoing access list is not set  
  Inbound  access list is not set  
  Proxy ARP is enabled  
  Security level is default  
  Split horizon is enabled  
  ICMP redirects are always sent  
  ICMP unreachables are always sent  
  ICMP mask replies are never sent  
  IP fast switching is disabled  
  IP fast switching on the same interface is disabled  
  IP Flow switching is disabled  
  IP Fast switching turbo vector  
  IP multicast fast switching is disabled  
  IP multicast distributed fast switching is disabled  
  Router Discovery is disabled  
 --More--  
 
## Вопрос:      
## Смогли ли вы сделать это как технический пользователь? Объяснять.    
## Введите свои ответы здесь.    
Да.Данная команда доступна для технического пользователя.(Представление Tech)   

*j. Выполните команду показать ip-маршрут.*  

R1#show ip route  
           ^
% Invalid input detected at '^' marker.  
## Вопрос:   
## Смогли ли вы сделать это как технический пользователь?    
## Введите свои ответы здесь.  
Данная команда не доступная техническому пользователю.(Представление Tech)  

*k. Вернитесь к корневому просмотру с помощью команды включить просмотр. *   
R1# enable view        
Пароль: cisco12345        
*l. Выполните команду show run, чтобы просмотреть созданные вами представления.*    

parser view tech  
 secret 5 $1$mERr$QMUdu4f7Qgk/Gy0quE8Eh0  
 commands exec include show  
 commands exec include show interfaces  
 commands exec include show ip  
 commands exec include show ip interface  
 commands exec include show ip interface brief  
 commands exec include show parser  
 commands exec include show parser view  
 commands exec include show version             

## Вопрос:         
## Для технического просмотра, почему перечислены команды show и show ip, а также show ip interface и show ip interface brief?      
## Введите свои ответы здесь.  
Для доступа к командам  
show ?  
show ip ?  
show ip interface ?  
show ip interface brief  
*m. Настройте те же административные роли на маршрутизаторе R3.*     
         
### Шаг 7: Сохраните конфигурацию на маршрутизаторах R1 и R3.    
Сохраните текущую конфигурацию в конфигурации запуска из привилегированной командной строки EXEC.   
R1#copy running-config startup-config  
Destination filename [startup-config]?   
Building configuration...  
[OK]  
  
