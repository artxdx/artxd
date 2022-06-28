# Лабораторная работ № 6   
# Настройка безопасности STP
## Представлена следующая топология   
![](topology6.jpg)  
## Таблица IP-адресации.  
![](table6.jpg)  
# Цели  
## Часть 1: Настройка Основных параметров Коммутатора  
 Постройте топологию.  
 Настройте имя хоста, IP-адрес и пароли доступа.  
## Часть 2. Настройка защищенных магистральных портов( Secure Trunks Ports)  
 Настройте режим магистрального порта.  
 Измените собственную VLAN(native) для магистральных портов.  
 Проверьте конфигурацию магистрали(trunk).  
 Отключите транкинг.  
## Часть 3: Защита От STP-Атак  
 Включите PortFast и BPDU guard.  
 Проверьте защиту BPDU guard.  
 Включите root guard.  
 Включить loop guard.  
## Часть 4: Настройка безопасности портов и отключение неиспользуемых портов  
 Настройте и проверьте безопасность портов.  
 Отключите неиспользуемые порты.  
 Переместите порты из VLAN 1 по умолчанию в альтернативную VLAN.  
 Настройте пограничную функцию PVLAN на порту.    
## Теоретическая часть   
Инфраструктура уровня 2 состоит в основном из взаимосвязанных коммутаторов Ethernet. Большинство устройств конечного пользователя, таких как компьютеры, принтеры, IP-телефоны и другие хосты, подключаются к сети через коммутаторы доступа уровня 2. В результате коммутаторы могут представлять угрозу безопасности сети. Подобно маршрутизаторам, коммутаторы подвергаются атакам со стороны злонамеренных внутренних пользователей. Программное обеспечение коммутатора Cisco IOS предоставляет множество функций безопасности, специфичных для функций и протоколов коммутатора.  
В этой лабораторной работе вы настроите различные меры защиты коммутатора, включая защиту портов доступа и функции протокола связующего дерева (STP), такие как защита BPDU и защита root.  
Примечание: Маршрутизаторы, используемые в практических лабораториях CCNA, - это Cisco 4221 с Cisco IOS XE версии 16.9.6 (изображение universalk9). В лабораториях используются коммутаторы Cisco Catalyst 2960+ с Cisco IOS версии 15.2 (7) (изображение lanbasek9). Можно использовать другие маршрутизаторы, коммутаторы и версии Cisco IOS. В зависимости от модели и версии Cisco IOS доступные команды и выдаваемый результат могут отличаться от того, что показано в лабораторных условиях. Правильные идентификаторы интерфейсов приведены в Сводной таблице интерфейса маршрутизатора в конце лабораторной работы.  
Примечание: Прежде чем начать, убедитесь, что маршрутизаторы и коммутаторы были удалены и не имеют конфигураций запуска.   
## Необходимые Ресурсы   
Необходимые Ресурсы  
 1 Маршрутизатор (Cisco 4221 с универсальным образом Cisco XE версии 16.9.6 или сопоставимым с лицензией на пакет технологий безопасности)  
 2 коммутатора (Cisco 2960+ с изображением Cisco IOS версии 15.2(7) lanbasek9 или аналогичным)  
 2 ПК (ОС Windows с установленной программой эмуляции терминала, такой как PuTTY или Tera Term)   
 Консольные кабели для настройки сетевых устройств Cisco  
 Кабели Ethernet , как показано в топологии 
# Выполнение лабораторной работы № 6  
Для выполнения данной лабораторной работы 
Данная топология была собрана на лабораторном(программном) стенде EVE-NG.В дальнейшем настройка сетевых устройств будет осуществляться согласно схемы собранной на стенде.   
![](eve.jpg)
## Часть 1: Настройка Основных параметров Коммутатора
В части 1 вы настроите топологию сети и настроите основные параметры, такие как имена хостов, IP-адреса и пароли доступа к устройствам.
### Шаг 1: Подключите сеть, как показано в топологии.
Подсоедините устройства, как показано на схеме топологии, и при необходимости подключите кабель.
### Шаг 2: Настройте основные параметры для маршрутизатора и каждого коммутатора.
Выполните все задачи на R1, S1 и S2. Процедура для S1 показана здесь в качестве примера.
Открыть окно конфигурации
Настройте имена хостов, как показано в топологии.
Настройте IP-адреса интерфейса, как показано в таблице IP-адресации. Следующая конфигурация отображает интерфейс управления VLAN 1 на S1:   
S1(config)# interface vlan 1  
S1(config-if)# ip address 192.168.1.2 255.255.255.0  
S1(config-if)# no shutdown  
Предотвратите попытки маршрутизатора или коммутатора перевести неправильно введенные команды, отключив поиск DNS.    
S1 показан здесь в качестве примера  
S1(config)# no ip domain-lookup
HTTP-доступ к коммутатору включен по умолчанию. Предотвратите доступ по протоколу HTTP, отключив HTTP-сервер и HTTP secure server.   
S1(config)# no ip http server
S1(config)# no ip http secure-server
### Примечание: Коммутатор должен иметь криптографический образ IOS для поддержки команды ip http secure-server. HTTP-доступ к маршрутизатору по умолчанию отключен.  
Настройте параметр enable secret password.  
S1(config)# enable algorithm-type scrypt secret cisco12345  
Настройте пароль консоли.  
S1(config)# line console 0  
S1(config-line)# password ciscoconpass  
S1(config-line)# exec-timeout 5 0  
S1(config-line)# login    
S1(config-line)# logging synchronous    
### Шаг 3: Настройте параметры IP-адреса хоста ПК.  
Настройте статический IP-адрес, маску подсети и шлюз по умолчанию для PCA и PCB, как показано в таблице адресации.  
### Шаг 4: Проверьте базовое сетевое подключение.     
a. Отправьте Ping-запрос от PCA и PCB к интерфейсу R1 G0/0/1 по IP-адресу 192.168.1.1.  
b. Отправьте Ping-запрос от PC-A к PC-B.  
### Шаг 5: Сохраните основные конфигурации для маршрутизатора и обоих коммутаторов.  
Сохраните текущую конфигурацию в конфигурации запуска из приглашения привилегированного режима EXEC.  
S1# copy running-config startup-config  
## Часть 2. Настройка Защищенных Магистральных портов  
В этой части вы настроите магистральные порты, измените собственную VLAN для магистральных портов и проверите конфигурацию магистрали.  
Защита магистральных портов может помочь предотвратить атаки с перескакиванием VLAN. Лучший способ предотвратить базовую атаку с перескакиванием VLAN - это явно отключить транкинг на всех портах, кроме портов, которые специально требуют транкинга. На требуемых портах транкинга отключите переговоры DTP (auto trunking) и включите транкинг вручную. Если транкинг для интерфейса не требуется, настройте порт в качестве порта доступа. Это отключает транкинг в интерфейсе.  
Примечание: Задачи должны выполняться на S1 или S2, как указано.  
### Шаг 1: Настройте S1 в качестве корневого коммутатора.  
Для целей этой вкладки S2 в настоящее время является корневым мостом. Вы настроите S1 в качестве корневого моста, изменив уровень приоритета идентификатора моста.  
a. Из консоли на S1 войдите в режим глобальной конфигурации.    
b. Приоритет по умолчанию для S1 и S2 равен 32769 (32768 + 1 с расширением системного идентификатора). Установите приоритет S1 равным 0, чтобы он стал корневым коммутатором.  
S1(config)# spanning-tree vlan 1 priority 0
S1(config)# exit
### Примечание: Вы также можете использовать команду spanning-tree vlan 1 root primary, чтобы сделать S1 корневым коммутатором для VLAN 1.  
c. Выполните команду show spanning-tree , чтобы убедиться, что S1 является корневым мостом, чтобы увидеть используемые порты и их статус.  
S1# show spanning-tree  

VLAN0001  
  Spanning tree enabled protocol ieee  
  Root ID    Priority    1  
             Address     001d.4635.0c80   
             This bridge is the root  
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec  

  Bridge ID  Priority    1      (priority 0 sys-id-ext 1)  
             Address     001d.4635.0c80  
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec  
             Aging Time 300  

Interface        Role Sts Cost      Prio.Nbr Type  
---------------- ---- --- --------- -------- --------------------------------  
Fa0/1            Desg FWD 19        128.1    P2p  
Fa0/5            Desg FWD 19        128.5    P2p   
Fa0/6            Desg FWD 19        128.6    P2p  
### Вопрос:  
### Каков приоритет S1?  
### Какие порты используются и каков их статус?   

### Шаг 2: Настройте магистральные порты на S1 и S2.  
a. Настройте порт F0/1 на S1 в качестве магистрального порта.   
S1(config)# interface f0/1  
S1(config-if)# switchport mode trunk  
### Примечание: При выполнении этой лабораторной работы с коммутатором 3560 пользователь должен сначала ввести команду switchport trunk encapsulation dot1q.    
b. Настройте порт F0/1 на S2 в качестве магистрального порта.  
S2(config)# interface f0/1  
S2(config-if)# switchport mode trunk  
c. Убедитесь, что порт S1 F0/1 находится в режиме транкинга, с помощью команды show interfaces trunk.  
S1# show interfaces trunk  

Port        Mode         Encapsulation  Status        Native vlan  
Fa0/1       on           802.1q         trunking      1  

Port        Vlans allowed on trunk  
Fa0/1       1-4094  

Port        Vlans allowed and active in management domain  
Fa0/1       1  

Port        Vlans in spanning tree forwarding state and not pruned  
Fa0/1       1  

### Шаг 3: Измените собственную VLAN для магистральных портов на S1 и S2.  
a. Изменение собственной VLAN для магистральных портов на неиспользуемую VLAN помогает предотвратить атаки с перескакиванием VLAN.    
Вопрос:  
Из выходных данных команды show interfaces trunk на предыдущем шаге, какова текущая собственная VLAN для интерфейса магистрали S1 F0/1?    
b. Установите для собственной VLAN на магистральном интерфейсе S1 F0/1 значение неиспользуемой VLAN 99.  
S1(config)# interface f0/1  
S1(config-if)# switchport trunk native vlan 99  
S1(config-if)# end  
c. Через короткий промежуток времени должно появиться следующее сообщение:  
02:16:28: %CDP-4-NATIVE_VLAN_MISMATCH: Native VLAN mismatch discovered on FastEthernet0/1 (99), with S2 FastEthernet0/1 (1).  
Что означает это сообщение?  
d. Установите для собственной VLAN на магистральном интерфейсе S2 F0/1 значение VLAN 99.  
S2(config)# interface f0/1  
S2(config-if)# switchport trunk native vlan 99  
S2(config-if)# end  
### Шаг 4: Предотвратите использование DTP на S1 и S2.
Установка для магистрального порта значения nonegotiate также помогает уменьшить переключение VLAN, отключив генерацию кадров DTP.
S1(config)# interface f0/1   
S1(config-if)# switchport nonegotiate  

S2(config)# interface f0/1  
S2(config-if)# switchport nonegotiate  
Шаг 5: Проверьте конфигурацию транкинга на порту F0/1.  
S1# show interfaces f0/1 trunk  

Port        Mode         Encapsulation  Status        Native vlan  
Fa0/1       on           802.1q         trunking      99  
  
Port        Vlans allowed on trunk  
Fa0/1       1-4094  

Port        Vlans allowed and active in management domain  
Fa0/1       1  
  
Port        Vlans in spanning tree forwarding state and not pruned  
Fa0/1       1  

S1# show interfaces f0/1 switchport  

Name: Fa0/1  
Switchport: Enabled  
Administrative Mode: trunk  
Operational Mode: trunk  
Administrative Trunking Encapsulation: dot1q  
Operational Trunking Encapsulation: dot1q  
Negotiation of Trunking: Off  
Access Mode VLAN: 1 (default)  
Trunking Native Mode VLAN: 99 (Inactive)  
Administrative Native VLAN tagging: enabled  
Voice VLAN: none  
Administrative private-vlan host-association: none  
Administrative private-vlan mapping: none  
Administrative private-vlan trunk native VLAN: none  
Administrative private-vlan trunk Native VLAN tagging: enabled 
Administrative private-vlan trunk encapsulation: dot1q 
Administrative private-vlan trunk normal VLANs: none  
Administrative private-vlan trunk private VLANs: none 
Operational private-vlan: none 
Trunking VLANs Enabled: ALL    
Pruning VLANs Enabled: 2-1001   
Capture Mode Disabled   
Capture VLANs Allowed: ALL  

Protected: false  
Unknown unicast blocked: disabled  
Unknown multicast blocked: disabled  
Appliance trust: none  

### Шаг 6: Проверьте конфигурацию с помощью команды show run.  
Используйте команду show run для отображения текущей конфигурации, начиная с первой строки, содержащей текстовую строку “0/1”.  

S1# show run | begin 0/1  
interface FastEthernet0/1  
 switchport trunk native vlan 99  
 switchport mode trunk  
 switchport nonegotiate  
  
<output omitted>  
### Шаг 7: Отключите транкинг на портах доступа S1.  
a. На S1 настройте F0/5, порт, к которому подключен R1, только в режиме доступа.  
S1(config)# interface f0/5  
S1(config-if)# switchport mode access  
b. На S1 настройте F0/6, порт, к которому подключен PCA, только в режиме доступа.    
S1(config)# interface f0/6  
S1(config-if)# switchport mode access  
### Шаг 8: Отключите транкинг на портах доступа S2.
На S2 настройте F0/18, порт, к которому подключена PCB, только в режиме доступа.  
S2(config)# interface f0/18  
S2(config-if)# switchport mode access  
## Часть 3: Защита От STP-Атак
Сетевые злоумышленники надеются подделать свою систему или мошеннический коммутатор, который они добавляют в сеть, в качестве корневого моста в топологии, манипулируя параметрами корневого моста STP. Если порт, настроенный с помощью PortFast, получает BPDU, STP может перевести порт в состояние блокировки с помощью функции, называемой BPDU guard.  
Топология имеет только два коммутатора и не имеет избыточных путей, но STP все еще активен. В этой части вы включите функции безопасности коммутаторов, которые могут помочь уменьшить вероятность того, что злоумышленник манипулирует коммутаторами с помощью методов, связанных с STP.   
### Шаг 1: Включите portfast.  
PortFast настраивается на портах доступа, которые подключаются к одной рабочей станции или серверу, что позволяет им быстрее становиться активными         
a. Включите PortFast на порту доступа S1 F0/5.  
S1(config)# interface f0/5
S1(config-if)# spanning-tree portfast

%Warning: portfast should only be enabled on ports connected to a single host. Connecting hubs, concentrators, switches, bridges, etc... to this interface when portfast is enabled, can cause temporary bridging loops. Use with CAUTION   

%Portfast has been configured on FastEthernet0/5 but will only  
 have effect when the interface is in a non-trunking mode.  
b. Включите PortFast на порту доступа S1 F0/6.  
S1(config)# interface f0/6  
S1(config-if)# spanning-tree portfast  
c. Включите PortFast на портах доступа S2 F0/18.  
S2(config)# interface f0/18  
S2(config-if)# spanning-tree portfast  
### Шаг 2: Включите защиту BPDU.  
BPDU guard - это функция, которая может помочь предотвратить несанкционированные коммутаторы и подмену портов доступа.  
a. Включите защиту BPDU на порту коммутатора F0/6.         
S1(config)# interface f0/6
S1(config-if)# spanning-tree bpduguard enable
S2(config)# interface f0/18
S2(config-if)# spanning-tree bpduguard enable
Примечание: PortFast и BPDU guard также можно включить глобально с помощью команд spanning-tree portfast default и spanning-tree portfast bpduguard в режиме глобальной конфигурации. 
Примечание: Защита BPDU может быть включена на всех портах доступа, для которых включена функция PortFast. Эти порты никогда не должны получать BPDU. BPDU guard лучше всего развертывать на портах, ориентированных на пользователя, чтобы предотвратить несанкционированное расширение сети коммутатора злоумышленником. Если порт включен с помощью BPDU guard и получает BPDU, он отключен и должен быть повторно включен вручную. На порту можно настроить тайм-аут, допускающий ошибку, чтобы он мог автоматически восстанавливаться по истечении указанного периода времени.   
b. Убедитесь, что защита BPDU настроена с помощью команды show spanning-tree interface f0/6 detail на S1.      
S1# show spanning-tree interface f0/6 detail  
    Port 6 (FastEthernet0/6) of VLAN0001 is designated forwarding  
   Port path cost 19, Port priority 128, Port Identifier 128.6.  
   Designated root has priority 1, address 001d.4635.0c80  
   Designated bridge has priority 1, address 001d.4635.0c80  
   Designated port id is 128.6, designated path cost 0  
   Timers: message age 0, forward delay 0, hold 0  
   Number of transitions to forwarding state: 1  
   The port is in the portfast mode  
   Link type is point-to-point by default  
   Bpdu guard is enabled  
   BPDU: sent 3349, received 0  

### Шаг 3: Включите root guard.
Root guard - это еще один вариант предотвращения несанкционированных переключений и подмены. Защита Root может быть включена на всех портах коммутатора, которые не являются корневыми портами. Обычно он включен только на портах, подключаемых к пограничным коммутаторам, где никогда не должен приниматься улучшенный BPDU. Каждый коммутатор должен иметь только один корневой порт, который является наилучшим путем к корневому коммутатору.
a. Следующая команда настраивает root guard на интерфейсе S2 G0/1. Обычно это делается, если к этому порту подключен другой коммутатор. Root guard лучше всего развертывать на портах, которые подключаются к коммутаторам, которые не должны быть корневым мостом. В лабораторной топологии S1 F0/1 был бы наиболее логичным кандидатом на root guard. Однако S2 G0/1 показан здесь в качестве примера, поскольку гигабитные порты чаще используются для межпереключательных соединений.       

       
       
       
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
C:\ipconfig          
FastEthernet0 Connection:(default port)           

   Connection-specific DNS Suffix..:         
   Link-local IPv6 Address.........: FE80::2D0:58FF:FEEE:866        
   IPv6 Address....................: ::        
   IPv4 Address....................: 192.168.3.3        
   Subnet Mask.....................: 255.255.255.0        
   Default Gateway.................: ::        
                                     192.168.3.1        
### Шаг 6: Проверьте подключение между ПC-A и PC-C.    
C:\ping 192.168.3.3    

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
C:\ping 192.168.3.1    

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

## Часть 2. Настройка основных параметров безопасности на R1
В этой части скопируйте и вставьте следующие команды в R1 для настройки основных параметров безопасности.  
enable
configure terminal
service password-encryption
security passwords min-length 10
enable algorithm-type scrypt secret cisco12345
ip domain name netsec.com
username user01 algorithm-type scrypt secret user01pass
username admin privilege 15 algorithm-type scrypt secret adminpasswd
banner motd " Unauthorized access is strictly prohibited! "
line con 0
 exec-timeout 5 0
login local
 logging synchronous
line aux 0
 exec-timeout 5 0
login local
line vty 0 4
 exec-timeout 5 0
 privilege level 15
transport input ssh
 login local
crypto key generate rsa general-keys modulus 1024
ip ssh time-out 90
ip ssh authentication-retries 2
ip ssh version 2

## Часть 3. Настройка Автоматических функций Безопасности
В этой части вы сделаете следующее:
* Используйте автозащиту для защиты R3.
* Просмотрите конфигурации безопасности маршрутизатора с помощью CLI.
С помощью одной команды в режиме CLI функция автозащиты позволяет отключить обычные IP-службы, которые могут быть использованы для сетевых атак. Он также может включать IP-сервисы и функции, которые могут помочь в защите сети при атаке. Автозащита упрощает настройку безопасности маршрутизатора и упрощает настройку маршрутизатора.

### Шаг 1: Используйте функцию автоматической защиты Cisco IOS на R3.
a. Войдите в привилегированный режим EXEC с помощью команды enable.
b. Выполните команду auto secure на R3, чтобы заблокировать маршрутизатор. R2 представляет собой маршрутизатор интернет-провайдера, поэтому предположим, что R3 G0/0/0 подключен к Интернету при запросе вопросов автозащиты. Ответьте на вопросы автозащиты, как показано в следующих выходных данных. Ответы выделены жирным шрифтом.

R3# auto secure
                --- AutoSecure Configuration ---

*** AutoSecure configuration enhances the security of
the router but it will not make router absolutely secure
from all security attacks ***

All the configuration done as part of AutoSecure will be
shown here. For more details of why and how this configuration
is useful, and any possible side effects, please refer to Cisco
documentation of AutoSecure.
At any prompt you may enter '?' for help.
Use ctrl-c to abort this session at any prompt.

If this device is being managed by a network management station,
AutoSecure configuration may block network management traffic.
Continue with AutoSecure? [no]: yes

Gathering information about the router for AutoSecure

Is this router connected to internet? [no]: yes
Enter the number of interfaces facing internet [1]: 
Interface              IP-Address      OK? Method Status                Protocol
GigabitEthernet0/0/0   10.2.2.1        YES manual up                    up      
GigabitEthernet0/0/1   192.168.3.1     YES manual up                    up      
Serial0/1/0            unassigned      YES unset  up                    up      
Serial0/1/1            unassigned      YES unset  up                    up      
Enter the interface name that is facing internet: GigabitEthernet0/0/0

Securing Management plane services..

Disabling service finger
Disabling service pad
Disabling udp & tcp small servers
Enabling service password encryption
Enabling service tcp-keepalives-in
Enabling service tcp-keepalives-out
Disabling the cdp protocol

Disabling the bootp server
Disabling the http server
Disabling the finger service
Disabling source routing
Disabling gratuitous arp

Here is a sample Security Banner to be shown
at every access to device. Modify it to suit your
enterprise requirements.

Authorized Access only
  This system is the property of So-&-So-Enterprise.
  UNAUTHORIZED ACCESS TO THIS DEVICE IS PROHIBITED.
  You must have explicit permission to access this
  device. All activities performed on this device
  are logged. Any violations of access policy will result
  in disciplinary action.

Enter the security banner {Put the banner between
k and k, where k is any character}:
# Unauthorized Access Prohibited #
Enable secret is either not configured or
 is the same as the enable password
Enter the new enable secret: cisco12345
Confirm the enable secret : cisco12345
Enter the new enable password: 12345cisco
Confirm the enable password: 12345cisco

Configuration of local user database
Enter the username: admin
Enter the password: adminpasswd
Confirm the password: adminpasswd
Configuring AAA local authentication
Configuring console, Aux and vty lines for
local authentication, exec-timeout, transport
Securing device against Login Attacks
Configure the following parameters

Blocking Period when Login Attack detected: 60

Maximum Login failures with the device: 2

Maximum time period for crossing the failed login attempts: 30

Configure SSH server? [yes]: [Enter]
Enter the domain-name: www.netsec.com

Configuring interface specific AutoSecure services
Disabling the following ip services on all interfaces:

 no ip redirects
 no ip proxy-arp
 no ip unreachables
 no ip directed-broadcast
 no ip mask-reply

Securing Forwarding plane services..

Enabling unicast rpf on all interfaces connected
to internet

Configure CBAC Firewall feature? [yes/no]: no

This is the configuration generated:

no service finger
no service pad
no service udp-small-servers
no service tcp-small-servers
service password-encryption
service tcp-keepalives-in
service tcp-keepalives-out
no cdp run
no ip bootp server
no ip http server
no ip finger
no ip source-route
no ip gratuitous-arps
banner motd ^C  Unauthorized Access Prohibited ^C
security passwords min-length 6
security authentication failure rate 10 log
enable secret 5 $1$lubv$Rdx4gHUcijbxV7p2z76/71
enable password 7 110A1016141D5D5B5C737B
username admin password 7 02050D4808095E731F1A5C
aaa new-model
aaa authentication login local_auth local
line console 0
 login authentication local_auth
 exec-timeout 5 0
 transport output telnet
line aux 0
 login authentication local_auth
 exec-timeout 10 0
 transport output telnet
line vty 0 4
 login authentication local_auth
 transport input telnet
line tty 1
 login authentication local_auth
 exec-timeout 15 0
login block-for 60 attempts 2 within 30
ip domain-name www.netsec.com
crypto key generate rsa general-keys modulus 1024
ip ssh time-out 60
ip ssh authentication-retries 2
line vty 0 4
 transport input ssh telnet
service timestamps debug datetime msec localtime show-timezone
service timestamps log datetime msec localtime show-timezone
logging facility local2
logging trap debugging
service sequence-numbers
logging console critical
logging buffered
int GigabitEthernet0/0/0
 no ip redirects
 no ip proxy-arp
 no ip unreachables
 no ip directed-broadcast
 no ip mask-reply
int GigabitEthernet0/0/1
 no ip redirects
 no ip proxy-arp
 no ip unreachables
 no ip directed-broadcast
 no ip mask-reply
ip access-list extended 100
 permit udp any any eq bootpc
interface GigabitEthernet0/0/0
 ip verify unicast source reachable-via rx 100
!
end


Apply this configuration to running-config? [yes]: [Enter]

Applying the config generated to running-config

 WARNING: Command has been added to the configuration using a type 5 password. However, type 5 passwords will soon be deprecated. Migrate to a supported password type
 WARNING: Command has been added to the configuration using a type 7 password. However, type 7 passwords will soon be deprecated. Migrate to a supported password type
 WARNING: Command has been added to the configuration using a type 7 password. However, type 7 passwords will soon be deprecated. Migrate to a supported password typeThe name for the keys will be: R3.www.netsec.com

% The key modulus size is 1024 bits
% Generating 1024 bit RSA keys, keys will be non-exportable...
[OK] (elapsed time was 0 seconds)

R3#

Примечание: Задаваемые вопросы и выходные данные могут отличаться в зависимости от функций образа IOS и устройства.

## Шаг 2: Установите SSH-соединение с PC-C на R3.
a. Запустите PuTTY или другой SSH-клиент и войдите в систему с учетной записью администратора и паролем adminpasswd, созданными при запуске AutoSecure. Введите IP-адрес интерфейса R3 G0/0/1 192.168.3.1.
b. Поскольку SSH был настроен с использованием AutoSecure на R3, вы получите предупреждение о безопасности PuTTY. Нажмите кнопку Да, чтобы подключиться в любом случае.
c. Войдите в привилегированный режим EXEC с паролем cisco12345 и проверьте конфигурацию R3 с помощью команды show run.

![](SSh.jpg)

Current configuration : 4444 bytes  
!
! Last configuration change at 21:42:02 UTC Sat Jun 25 2022  
!
version 15.5  
no service pad  
service tcp-keepalives-in  
service tcp-keepalives-out  
service timestamps debug datetime msec localtime show-timezone  
service timestamps log datetime msec localtime show-timezone  
service password-encryption  
service sequence-numbers  
!  
hostname R3  
!
boot-start-marker  
boot-end-marker  
!
!  
security authentication failure rate 10 log  
security passwords min-length 6  
logging console critical  
enable secret 5 $1$xfXC$ZU7sZmPJs25AhFSuWKUwe0  
enable password 7 00554155500E080F1C2243  
!  
aaa new-model  
!  
!  
aaa authentication login local_auth local  
!
!
!
!
!
aaa session-id common  
ethernet lmi ce  
!
!
!
mmi polling-interval 60  
no mmi auto-configure  
no mmi pvc  
mmi snmp-timeout 180  
!
!
!
!
!
no ip source-route  
no ip gratuitous-arps  
!
!
!
!
!
!  
no ip bootp server  
no ip domain lookup  
ip domain name www.netsec.com  
ip cef  
login block-for 60 attempts 2 within 30  
no ipv6 cef  
!  
multilink bundle-name authenticated  
!
!
!
!  
archive  
 log config   
  logging enable  
username admin password 7 070E25414707090404011C08   
!  
redundancy  
!  
no cdp run  
!
!
!
!
!
!
!
!
!
!
!  
! 
!  
!  
interface GigabitEthernet0/0  
 ip address 10.2.2.1 255.255.255.252  
 no ip redirects  
 no ip unreachables 
 no ip proxy-arp  
 ip verify unicast source reachable-via rx allow-default 100  
 duplex auto  
 speed auto 
 media-type rj45 
 no mop enabled  
!  
interface GigabitEthernet0/1  
 ip address 192.168.3.1 255.255.255.0  
 no ip redirects  
 no ip unreachables  
 no ip proxy-arp  
 duplex auto 
 speed auto  
 media-type rj45 
 no mop enabled  
! 
interface GigabitEthernet0/2  
 no ip address  
 no ip redirects  
 no ip unreachables 
 no ip proxy-arp  
 shutdown  
 duplex auto  
 speed auto  
 media-type rj45  
 no mop enabled  
!  
interface GigabitEthernet0/3  
 no ip address  
 no ip redirects  
 no ip unreachables 
 no ip proxy-arp  
 shutdown  
 duplex auto  
 speed auto  
 media-type rj45  
 no mop enabled   
!  
router ospf 1  
 passive-interface GigabitEthernet0/1  
 network 10.2.2.0 0.0.0.3 area 0  
 network 192.168.3.0 0.0.0.255 area 0  
!  
ip forward-protocol nd  
!  
!  
no ip http server  
no ip http secure-server  
ip ssh time-out 60  
ip ssh authentication-retries 2  
!  
!  
logging trap debugging  
logging facility local2  
!  
!  
access-list 100 permit udp any any eq bootpc  
!  
!  
!  
control-plane  
!  
banner exec ^C  
**************************************************************************  
* IOSv is strictly limited to use for evaluation, demonstration and IOS  *  
* education. IOSv is provided as-is and is not supported by Cisco's      *  
* Technical Advisory Center. Any use or disclosure, in whole or in part, *  
* of the IOSv Software or Documentation to any third party for any       *  
* purposes is expressly prohibited except as otherwise authorized by     *  
* Cisco in writing.                                                      *  
**************************************************************************^C  
banner incoming ^C  
**************************************************************************  
* IOSv is strictly limited to use for evaluation, demonstration and IOS  * 
* education. IOSv is provided as-is and is not supported by Cisco's      *  
* Technical Advisory Center. Any use or disclosure, in whole or in part, *  
* of the IOSv Software or Documentation to any third party for any       *  
* purposes is expressly prohibited except as otherwise authorized by     *  
* Cisco in writing.                                                      *  
**************************************************************************^C  
banner login ^C  
**************************************************************************  
* IOSv is strictly limited to use for evaluation, demonstration and IOS  *  
* education. IOSv is provided as-is and is not supported by Cisco's      *  
* Technical Advisory Center. Any use or disclosure, in whole or in part, *  
* of the IOSv Software or Documentation to any third party for any       *  
* purposes is expressly prohibited except as otherwise authorized by     *  
* Cisco in writing.                                                      *  
**************************************************************************^C  
banner motd ^C Unauthorized Access Prohibited ^C  
!
line con 0  
 exec-timeout 5 0  
 login authentication local_auth  
 transport output telnet  
line aux 0  
 exec-timeout 15 0  
 login authentication local_auth  
 transport output telnet  
line vty 0 4  
 login authentication local_auth  
 transport input telnet ssh  
!  
no scheduler allocate  
!  
end  

## В отличии от обычной защиты ,Автозащита разрывает сетевое соединение при попытке неправильного ввода данных.

![](SSH2.jpg)

