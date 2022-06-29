# Лабораторная работ № 6    
# Настройка безопасности STP  
## Представлена следующая топология     
![](topology6.jpg)   
## Таблица IP-адресации.    
![](table6.jpg)   
# Предварительная настройка.    
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
Данная топология была собрана на лабораторном(программном) стенде EVE-NG.  
В дальнейшем настройка сетевых устройств будет осуществляться согласно схемы собранной на стенде.     
![](topology61.jpg)
## Часть 1: Настройка Основных параметров Коммутатора
В части 1 вы настроите топологию сети и настроите основные параметры, такие как имена хостов, IP-адреса и пароли доступа к устройствам.
### Шаг 1: Подключите сеть, как показано в топологии.
Подсоедините устройства, как показано на схеме топологии, и при необходимости подключите кабель.
### Шаг 2: Настройте основные параметры для маршрутизатора и каждого коммутатора.
Выполните все задачи на R1, S1 и S2. Процедура для S1 показана здесь в качестве примера.
Открыть окно конфигурации
Настройте имена хостов, как показано в топологии.
Настройте IP-адреса интерфейса, как показано в таблице IP-адресации. Следующая конфигурация отображает интерфейс управления VLAN 1 на S1:   
S1(config)# interface vlan 5    
S1(config-if)# ip address 192.168.5.2 255.255.255.0    
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
a. Отправьте Ping-запрос от PCA и PCB к интерфейсу R1 G0/0/1 по IP-адресу 192.168.5.1
PC-A> ping 192.168.5.1

84 bytes from 192.168.5.1 icmp_seq=1 ttl=255 time=1.779 ms
84 bytes from 192.168.5.1 icmp_seq=2 ttl=255 time=1.735 ms
84 bytes from 192.168.5.1 icmp_seq=3 ttl=255 time=1.951 ms
84 bytes from 192.168.5.1 icmp_seq=4 ttl=255 time=1.720 ms
84 bytes from 192.168.5.1 icmp_seq=5 ttl=255 time=1.728 ms

PC-A>

PC-B> ping 192.168.5.1

84 bytes from 192.168.5.1 icmp_seq=1 ttl=255 time=2.866 ms
84 bytes from 192.168.5.1 icmp_seq=2 ttl=255 time=2.145 ms
84 bytes from 192.168.5.1 icmp_seq=3 ttl=255 time=2.082 ms
84 bytes from 192.168.5.1 icmp_seq=4 ttl=255 time=2.119 ms
84 bytes from 192.168.5.1 icmp_seq=5 ttl=255 time=2.247 ms

PC-B>

b. Отправьте Ping-запрос от PC-A к PC-B.  

PC-A> ping 192.168.5.11

84 bytes from 192.168.5.11 icmp_seq=1 ttl=64 time=0.720 ms
84 bytes from 192.168.5.11 icmp_seq=2 ttl=64 time=0.855 ms
84 bytes from 192.168.5.11 icmp_seq=3 ttl=64 time=0.906 ms
84 bytes from 192.168.5.11 icmp_seq=4 ttl=64 time=0.839 ms
84 bytes from 192.168.5.11 icmp_seq=5 ttl=64 time=0.847 ms




### Шаг 5: Сохраните основные конфигурации для маршрутизатора и обоих коммутаторов.  
Сохраните текущую конфигурацию в конфигурации запуска из приглашения привилегированного режима EXEC.  
S1# copy running-config startup-config  
## Часть 2. Настройка Защищенных Магистральных портов  
В этой части вы настроите магистральные порты, измените собственную VLAN для магистральных портов и проверите конфигурацию магистрали.  
Защита магистральных портов может помочь предотвратить атаки с перескакиванием VLAN. Лучший способ предотвратить базовую атаку с перескакиванием VLAN - это явно отключить транкинг на всех портах, кроме портов, которые специально требуют транкинга. На требуемых портах транкинга отключите переговоры DTP (auto trunking) и включите транкинг вручную. Если транкинг для интерфейса не требуется, настройте порт в качестве порта доступа. Это отключает транкинг в интерфейсе.  
Примечание: Задачи должны выполняться на S1 или S2, как указано.




S2(config)#interface range Ethernet0/2-3  отключаем не нужные порты
S2(config-if)#shutdown 
S2(config-if)#exit

*Jun 29 10:50:54.636: %LINK-5-CHANGED: Interface Ethernet0/2, changed state to administratively down
*Jun 29 10:50:54.645: %LINK-5-CHANGED: Interface Ethernet0/3, changed state to administratively down
*Jun 29 10:50:55.640: %LINEPROTO-5-UPDOWN: Line protocol on Interface Ethernet0/2, changed state to down
*Jun 29 10:50:55.649: %LINEPROTO-5-UPDOWN: Line protocol on Interface Ethernet0/3, changed state to down

### Шаг 1: Настройте S1 в качестве корневого коммутатора.  
Для целей этой вкладки S2 в настоящее время является корневым мостом. Вы настроите S1 в качестве корневого моста, изменив уровень приоритета идентификатора моста.  
a. Из консоли на S1 войдите в режим глобальной конфигурации.    
b. Приоритет по умолчанию для S1 и S2 равен 32769 (32768 + 1 с расширением системного идентификатора). Установите приоритет S1 равным 0, чтобы он стал корневым коммутатором.  
S1(config)# spanning-tree vlan 5 priority 0
S1(config)# exit
### Примечание: Вы также можете использовать команду spanning-tree vlan 5 root primary, чтобы сделать S1 корневым коммутатором для VLAN 5.  
c. Выполните команду show spanning-tree , чтобы убедиться, что S1 является корневым мостом, чтобы увидеть используемые порты и их статус.  
S1#show spanning-tree
VLAN0005
  Spanning tree enabled protocol rstp
  Root ID    Priority    5
             Address     aabb.cc00.5000
             This bridge is the root
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    5      (priority 0 sys-id-ext 5)
             Address     aabb.cc00.5000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Et0/0               Desg FWD 100       128.1    Shr
Et0/1               Desg FWD 100       128.2    Shr
Et0/2               Desg FWD 100       128.3    Shr Edge
Et0/3               Desg FWD 100       128.4    Shr

S2#show spanning-tree
VLAN0005
  Spanning tree enabled protocol rstp
  Root ID    Priority    5
             Address     aabb.cc00.5000
             Cost        100
             Port        1 (Ethernet0/0)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32773  (priority 32768 sys-id-ext 5)
             Address     aabb.cc00.7000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Et0/0               Root FWD 100       128.1    Shr
Et0/1               Desg FWD 100       128.2    Shr Edge

### Вопрос:  
### Каков приоритет S1? 
Приоритет коммутатора S1 равен нулю.
"This bridge is the root" -данный коммутатор является корневым.
### Какие порты используются и каков их статус?   
Коммутатор S1 -все 4 порта являются назначенными в режиме пересылки
Также порты находятся в режиме shared (half-duplex) - полудуплекс либо передает либо получает.
Et0/2 - является пограничным портом(к нему подключено конечное устройство)
### Шаг 2: Настройте магистральные порты на S1 и S2.  
a. Настройте порт e0/1 на S1 в качестве магистрального порта.   
S1(config)#interface Ethernet0/1
S1(config-if)#switchport trunk encapsulation dot1q
S1(config-if)#switchport mode trunk
S1(config-if)#switchport trunk allowed vlan 5 (привязываем порт к VLAN 5)
### Примечание: При выполнении этой лабораторной работы с коммутатором 3560 пользователь должен сначала ввести команду switchport trunk encapsulation dot1q.    
Выводим данные по транкам

S1#show interfaces trunk
Port        Mode             Encapsulation  Status        Native vlan
Et0/0       on               802.1q         trunking      59
Et0/1       on               802.1q         trunking      59

Port        Vlans allowed on trunk
Et0/0       5
Et0/1       5

Port        Vlans allowed and active in management domain
Et0/0       5
Et0/1       5

Port        Vlans in spanning tree forwarding state and not pruned
Et0/0       5
Et0/1       5

b. Настройте порт e0/0 на S2 в качестве магистрального порта.  
S2(config)#interface Ethernet0/0
S2(config-if)#switchport trunk encapsulation dot1q
S2(config-if)#switchport mode trunk
S2(config-if)#switchport trunk allowed vlan 5 (привязываем порт к VLAN 5)

Выводим данные по транкам
S2#show interfaces trunk

Port        Mode             Encapsulation  Status        Native vlan
Et0/0       auto             802.1q         trunking      59

Port        Vlans allowed on trunk
Et0/0       5

Port        Vlans allowed and active in management domain
Et0/0       5

Port        Vlans in spanning tree forwarding state and not pruned
Et0/0       5


### Шаг 3: Измените собственную VLAN для магистральных портов на S1 и S2.  
a. Изменение собственной VLAN для магистральных портов на неиспользуемую VLAN помогает предотвратить атаки с перескакиванием VLAN.    
Вопрос:  
Из выходных данных команды show interfaces trunk на предыдущем шаге, какова текущая собственная VLAN для интерфейса магистрали S1 E0/1? 
VLAN 5
b. Установите для собственной VLAN на магистральном интерфейсе S1 e0/1 значение неиспользуемой VLAN 59.  
S1(config)#interface Ethernet0/0  -встроенной vlan присваеваем номер нерабочей vlan 59 (только для транка)
S1(config-if)#switchport trunk native vlan 59
S1(config-if)#exit
c. Через короткий промежуток времени должно появиться следующее сообщение:  
*Jun 29 11:48:07.345: %CDP-4-NATIVE_VLAN_MISMATCH: Native VLAN mismatch discovered on Ethernet0/1 (59), with S2 Ethernet0/0 (1).
  
Что означает это сообщение?
Обнаружено несоответствие собственной VLAN коммутатора S1  и коммутатора S2
Для того чтобы не выводилось данное сообщение .Необходимо установить такое же значение неиспользуемой VLAN 59 для коммутатора S2  
d. Установите для собственной VLAN на магистральном интерфейсе S2 e0/0 значение VLAN 59.  
S2(config)# interface e0/0  
S2(config-if)# switchport trunk native vlan 59  
S2(config-if)# end    
### Шаг 4: Предотвратите использование DTP на S1 и S2.  
S2(config)#interface range Ethernet0/0-3  отключаем DTP на портах.  
S2(config-if)#switchport nonegotiate   
S2(config-if)#exit  
Command rejected: Conflict between 'nonegotiate' and 'dynamic' status on this interface: Et0/0  - в данном случае отключить DTP на магистральном порту не получилось.   

Выводим данные по интерфейсам

S2#sh interface switchport
Name: Et0/0
Switchport: Enabled
Administrative Mode: dynamic auto
Operational Mode: trunk
Administrative Trunking Encapsulation: dot1q
Operational Trunking Encapsulation: dot1q
Negotiation of Trunking: On
Access Mode VLAN: 1 (default)
Trunking Native Mode VLAN: 59 (Inactive)
Administrative Native VLAN tagging: enabled
Voice VLAN: none
Administrative private-vlan host-association: none
Administrative private-vlan mapping: none
Administrative private-vlan trunk native VLAN: none
Administrative private-vlan trunk Native VLAN tagging: enabled
Administrative private-vlan trunk encapsulation: dot1q
Administrative private-vlan trunk normal VLANs: none
Administrative private-vlan trunk associations: none
Administrative private-vlan trunk mappings: none
Operational private-vlan: none
Trunking VLANs Enabled: 5
Pruning VLANs Enabled: 2-1001
Capture Mode Disabled
Capture VLANs Allowed: ALL

Protected: false
Appliance trust: none

Name: Et0/1
Switchport: Enabled
Administrative Mode: static access
Operational Mode: static access
Administrative Trunking Encapsulation: negotiate
Operational Trunking Encapsulation: native
Negotiation of Trunking: Off
Access Mode VLAN: 5 (otus)
Trunking Native Mode VLAN: 1 (default)
Administrative Native VLAN tagging: enabled
Voice VLAN: none
Administrative private-vlan host-association: none
Administrative private-vlan mapping: none
Administrative private-vlan trunk native VLAN: none
Administrative private-vlan trunk Native VLAN tagging: enabled
Administrative private-vlan trunk encapsulation: dot1q
Administrative private-vlan trunk normal VLANs: none
Administrative private-vlan trunk associations: none
Administrative private-vlan trunk mappings: none
Operational private-vlan: none
Trunking VLANs Enabled: ALL
Pruning VLANs Enabled: 2-1001
Capture Mode Disabled
Capture VLANs Allowed: ALL

Protected: false
Appliance trust: none

% Range command terminated because it failed on Ethernet0/0

## Negotiation of Trunking: Off  - согласование транкинг - отключено

### Шаг 7: Отключите транкинг на портах доступа S1.  
a. На S1 настройте e0/0, порт, к которому подключен R1, только в режиме доступа.  
Данный порт работает в режиме транкинга
b. На S1 настройте e0/2, порт, к которому подключен PCA, только в режиме доступа.
Переведем все оставшиеся порты в режим доступа
S1(config)#interface range Ethernet0/2-3
S1(config-if)#switchport access
S1(config-if)#exit 
### Шаг 8: Отключите транкинг на портах доступа S2.
На S2 настройте e0/1, порт, к которому подключена PCB, только в режиме доступа.  
Переведем все оставшиеся порты в режим доступа
S2(config)#interface range Ethernet0/1-3
S2(config-if)#switchport access
S2(config-if)#exit

## Часть 3: Защита От STP-Атак
Сетевые злоумышленники надеются подделать свою систему или мошеннический коммутатор, который они добавляют в сеть, в качестве корневого моста в топологии, манипулируя параметрами корневого моста STP. Если порт, настроенный с помощью PortFast, получает BPDU, STP может перевести порт в состояние блокировки с помощью функции, называемой BPDU guard.  
Топология имеет только два коммутатора и не имеет избыточных путей, но STP все еще активен. В этой части вы включите функции безопасности коммутаторов, которые могут помочь уменьшить вероятность того, что злоумышленник манипулирует коммутаторами с помощью методов, связанных с STP.   
### Шаг 1: Включите portfast.  
PortFast настраивается на портах доступа, которые подключаются к одной рабочей станции или серверу, что позволяет им быстрее становиться активными         
a. Включите PortFast на порту доступа S1 e0/0.  
S1(config)#interface Ethernet0/0  
S1(config-if)#spanning-tree portfast
%Warning: portfast should only be enabled on ports connected to a single
 host. Connecting hubs, concentrators, switches, bridges, etc... to this
 interface  when portfast is enabled, can cause temporary bridging loops.
 Use with CAUTION

%Portfast has been configured on Ethernet0/0 but will only
 have effect when the interface is in a non-trunking mode.

b. Включите PortFast на порту доступа S1 e0/2.  
S1(config)#interface Ethernet0/2
S1(config-if)#spanning-tree portfast
%Warning: portfast should only be enabled on ports connected to a single
 host. Connecting hubs, concentrators, switches, bridges, etc... to this
 interface  when portfast is enabled, can cause temporary bridging loops.
 Use with CAUTION

%Portfast has been configured on Ethernet0/2 but will only
 have effect when the interface is in a non-trunking mode.
S1(config-if)#exit

c. Включите PortFast на портах доступа S2 e0/1.  
S2(config)#interface Ethernet0/1
 S2(config-if)#spanning-tree portfast

%Warning: portfast should only be enabled on ports connected to a single
 host. Connecting hubs, concentrators, switches, bridges, etc... to this
 interface  when portfast is enabled, can cause temporary bridging loops.
 Use with CAUTION

%Portfast has been configured on Ethernet0/1 but will only
 have effect when the interface is in a non-trunking mode.

### Шаг 2: Включите защиту BPDU.  
BPDU guard - это функция, которая может помочь предотвратить несанкционированные коммутаторы и подмену портов доступа.  
a. Включите защиту BPDU на порту коммутатора e0/0.         
S1(config)#interface Ethernet0/0
S1(config-if)# spanning-tree bpduguard enable
S1(config-if)#interface Ethernet0/2
S1(config-if)#spanning-tree  bpduguard enable
S2(config)#interface range Ethernet0/1-3
S2(config-if-range)#spanning-tree bpduguard enable
S2(config-if-range)#exit
Примечание: PortFast и BPDU guard также можно включить глобально с помощью команд spanning-tree portfast default и spanning-tree portfast bpduguard в режиме глобальной конфигурации. 
Примечание: Защита BPDU может быть включена на всех портах доступа, для которых включена функция PortFast. Эти порты никогда не должны получать BPDU. BPDU guard лучше всего развертывать на портах, ориентированных на пользователя, чтобы предотвратить несанкционированное расширение сети коммутатора злоумышленником. Если порт включен с помощью BPDU guard и получает BPDU, он отключен и должен быть повторно включен вручную. На порту можно настроить тайм-аут, допускающий ошибку, чтобы он мог автоматически восстанавливаться по истечении указанного периода времени.   
b. Убедитесь, что защита BPDU настроена с помощью команды show spanning-tree interface f0/6 detail на S1.      
S1#show spanning-tree interface Ethernet0/2

Vlan                Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
VLAN0005            Desg FWD 100       128.3    Shr Edge
S1#show spanning-tree interface Ethernet0/2 detail
 Port 3 (Ethernet0/2) of VLAN0005 is designated forwarding
   Port path cost 100, Port priority 128, Port Identifier 128.3.
   Designated root has priority 5, address aabb.cc00.5000
   Designated bridge has priority 5, address aabb.cc00.5000
   Designated port id is 128.3, designated path cost 0
   Timers: message age 0, forward delay 0, hold 0
   Number of transitions to forwarding state: 1
   The port is in the portfast edge mode
   Link type is shared by default
   Bpdu guard is enabled
   Root guard is enabled on the port
   BPDU: sent 2422, received 0
S1#


### Шаг 3: Включите root guard.
Root guard - это еще один вариант предотвращения несанкционированных переключений и подмены. Защита Root может быть включена на всех портах коммутатора, которые не являются корневыми портами. Обычно он включен только на портах, подключаемых к пограничным коммутаторам, где никогда не должен приниматься улучшенный BPDU. Каждый коммутатор должен иметь только один корневой порт, который является наилучшим путем к корневому коммутатору.
a. Следующая команда настраивает root guard на интерфейсе S2 e0/1. Обычно это делается, если к этому порту подключен другой коммутатор. Root guard лучше всего развертывать на портах, которые подключаются к коммутаторам, которые не должны быть корневым мостом. В лабораторной топологии S1  был бы наиболее логичным кандидатом на root guard.        
S2(config)#interface range Ethernet0/1-3
S2(config-if-range)#spanning-tree guard root
S2# show spanning-tree inconsistentports  

Name                 Interface              Inconsistency   
-------------------- ---------------------- ------------------  
Number of inconsistent ports (segments) in the system : 0  
### Примечание: Root guard позволяет подключенному коммутатору участвовать в STP до тех пор, пока устройство не попытается стать root. Если root guard блокирует порт, последующее восстановление происходит автоматически. Порт возвращается в состояние пересылки, если вышестоящие BPDU останавливаются.  
Шаг 4: Включите loop guard.
Функция STP loop guard обеспечивает дополнительную защиту от циклов пересылки уровня 2 (STP loops). Цикл STP создается, когда порт, блокирующий STP, в избыточной топологии ошибочно переходит в состояние пересылки. Обычно это происходит потому, что один из портов физически избыточной топологии (не обязательно порт, блокирующий STP) больше не принимает STP BPDU. Наличие всех портов в состоянии пересылки приведет к циклам пересылки. Если порт, включенный с помощью loop guard, перестает прослушивать BPDU с назначенного порта в сегменте, он переходит в состояние несогласованности цикла вместо перехода в состояние пересылки. Несогласованность цикла в основном блокирует, и трафик не пересылается. Когда порт снова обнаруживает BPDU, он автоматически восстанавливается, возвращаясь в состояние блокировки.    
a. Защита петли должна быть применена к не обозначенным портам. Таким образом, глобальная команда может быть настроена на некорневых коммутаторах.  
S2(config)# spanning-tree loopguard default    
b. Проверьте конфигурацию loopguard.    
S2# show spanning-tree summary  
Switch is in pvst mode  

Extended system ID           is enabled   
Portfast Default             is disabled  
PortFast BPDU Guard Default  is disabled  
Portfast BPDU Filter Default is disabled  
Loopguard Default            is enabled  
EtherChannel misconfig guard is enabled  
UplinkFast                   is disabled  
BackboneFast                 is disabled  
Configured Pathcost method used is short  

Name                   Blocking Listening Learning Forwarding STP Active  
---------------------- -------- --------- -------- ---------- ----------  
VLAN0005                     0         0        0          3          3   
---------------------- -------- --------- -------- ---------- ---------  

## Часть 4: Настройка безопасности портов и отключение неиспользуемых портов
Коммутаторы могут быть подвержены переполнению CAM-таблицы, также известной как таблица MAC-адресов, атакам подмены MAC и несанкционированным подключениям к портам коммутатора. В этой задаче вы настроите безопасность порта, чтобы ограничить количество MAC-адресов, которые могут быть изучены на порту коммутатора, и отключить порт, если это число будет превышено.  
### Шаг 1: Запишите MAC-адрес R1 G0/0/1.  
Из командной строки R1 используйте команду show interface и запишите MAC-адрес интерфейса.  
R1# show interfaces g0/0/1  
GigabitEthernet0/1 is up, line protocol is up
  Hardware is iGbE, address is 5000.0006.0001 (bia 5000.0006.0001)
  Internet address is 192.168.1.1/24
  MTU 1500 bytes, BW 1000000 Kbit/sec, DLY 10 usec,
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set

### Каков MAC-адрес интерфейса R1 G0/0/1?   
5000.0006.0001
### Шаг 2: Настройте базовую безопасность портов.
Эта процедура должна выполняться на всех используемых портах доступа. Порт S1 e0/0 показан здесь в качестве примера.  
a. Из командной строки S1 войдите в режим настройки интерфейса для порта, который подключается к маршрутизатору (FastEthernet0/5).  
S1(config)# interface e0/0  
b. Выключите порт коммутатора.  
S1(config)#interface Ethernet0/0
S1(config-if)#shutdown
S1(config-if)#
*Jun 29 14:18:53.210: %LINK-5-CHANGED: Interface Ethernet0/0, changed state to administratively down
*Jun 29 14:18:54.214: %LINEPROTO-5-UPDOWN: Line protocol on Interface Ethernet0/0, changed state to down
c. Включите режим защитного порта.  
S1(config-if)# switchport port-security  
Примечание: Порт коммутатора должен быть настроен как порт доступа, чтобы включить защиту порта.  
Примечание: Ввод только команды switchport port-security устанавливает максимальные MAC-адреса равными 1, а действие нарушения - завершению работы. Команды switchport port-security maximum и switchport port-security violation можно использовать для изменения поведения по умолчанию.   
d. Настройте статическую запись для MAC-адреса интерфейса R1 G0/0/1, записанного на шаге 1.  
S1(config-if)# switchport port-security mac-address xxxx.xxxx.xxxx   
Примечание: xxxx.xxxx.xxxx - это фактический MAC-адрес интерфейса маршрутизатора G0/0/1.  
Примечание: Вы также можете использовать команду switchport port-security mac-address sticky для добавления всех защищенных MAC-адресов, которые динамически запоминаются на порту (до максимального значения), в конфигурацию коммутатора. 
S1(config-if)#switchport port-security mac-address 5000.0006.0001  

e. Включите порт коммутатора.  
S1(config-if)# no shutdown  
### Шаг 3: Проверьте безопасность порта на S1 F0/5.  
a. На S1 выполните команду show port-security, чтобы убедиться, что безопасность порта настроена на S1 e0/0.  

S1#show port-security

Secure Port  MaxSecureAddr  CurrentAddr  SecurityViolation  Security Action
                (Count)       (Count)          (Count)
---------------------------------------------------------------------------
      Et0/0              1            1                  0         Shutdown
---------------------------------------------------------------------------
Total Addresses in System (excluding one mac per port)     : 0
Max Addresses limit in System (excluding one mac per port) : 4096


Каково количество нарушений безопасности?  
Введите свои ответы здесь.  

Каков статус порта e0/0?  
Введите свои ответы здесь.  

Каков последний адрес источника и VLAN?  
  
b. С помощью интерфейса командной строки R1 выполните ping PC-A для проверки подключения. Это также гарантирует, что коммутатор узнает MAC-адрес R1 G0/0/1.  
R1# ping 192.168.1.10  
c. Теперь нарушите безопасность, изменив MAC-адрес в интерфейсе маршрутизатора. Войдите в режим настройки интерфейса для Fast Ethernet 0/1. Настройте MAC-адрес для интерфейса на интерфейсе, используя aaaa.bbbb.cccc в качестве адреса.  
R1(config)# interface g0/0/1  
R1(config-if)# mac-address aaaa.bbbb.cccc  
R1(config-if)# end  
### Примечание: Вы также можете изменить MAC-адрес ПК, прикрепленный к S1 F0/6, и добиться результатов, аналогичных показанным здесь.  
Из командной строки R1 выполните поиск PC-A. Был ли пинг успешным? Объяснять.  
d. На консоли S1 следите за сообщениями, когда порт F0/5 обнаруживает нарушающий MAC-адрес.  

*Jan 14 01:34:39.750: %PM-4-ERR_DISABLE: psecure-violation error detected on Fa0/5, putting Fa0/5 in err-disable state  
*Jan 14 01:34:39.750: %PORT_SECURITY-2-PSECURE_VIOLATION: Security violation occurred, caused by MAC address aaaa.bbbb.cccc on port FastEthernet0/5.  
*Jan 14 01:34:40.756: %LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/5, changed state to down  
*Jan 14 01:34:41.755: %LINK-3-UPDOWN: Interface FastEthernet0/5, changed state to down  
e. На коммутаторе используйте команды show port-security, чтобы убедиться, что безопасность порта была нарушена.   

S1# show port-security  
Secure Port MaxSecureAddr CurrentAddr SecurityViolation Security Action  
               (Count)       (Count)        (Count)  
--------------------------------------------------------------------  
      Fa0/5            1           1                 1         Shutdown  
----------------------------------------------------------------------  
Total Addresses in System (excluding one mac per port)     : 0  
Max Addresses limit in System (excluding one mac per port) : 8192  
  
S1# show port-security interface f0/5  
Port Security              : Enabled  
Port Status                : Secure-shutdown  
Violation Mode             : Shutdown  
Aging Time                 : 0 mins  
Aging Type                 : Absolute  
SecureStatic Address Aging : Disabled  
Maximum MAC Addresses      : 1  
Total MAC Addresses        : 1  
Configured MAC Addresses   : 1  
Sticky MAC Addresses       : 0  
Last Source Address:Vlan   : aaaa.bbbb.cccc:1  
Security Violation Count   : 1  

S1# show port-security address  
Secure Mac Address Table  
-----------------------------------------------------------------------------  
Vlan    Mac Address       Type                          Ports   Remaining Age  
                                                                   (mins)  
----    -----------       ----                          -----   -------------  
   1    fc99.4775.c3e1    SecureConfigured              Fa0/5        -  
----------------------------------------------------------------------------- 
Total Addresses in System (excluding one mac per port)     : 0  
Max Addresses limit in System (excluding one mac per port) : 8192  
f. Удалите жестко закодированный MAC-адрес с маршрутизатора и повторно включите интерфейс G0/0/1.  
R1(config)# interface g0/0/1  
R1(config-if)# no mac-address aaaa.bbbb.cccc  
Примечание: Это восстановит исходный MAC-адрес интерфейса Gigabit Ethernet.  
Из R1 попробуйте снова выполнить пинг PC-A по адресу 192.168.1.10. Был ли пинг успешным? Объяснять.  
### Шаг 4: Снимите статус отключения ошибки S1 F0/5.  
a. В консоли S1 устраните ошибку и повторно включите порт, используя команды, показанные в примере. Это изменит статус порта с безопасного завершения работы на Безопасное включение.  
S1(config)# interface f0/5  
S1(config-if)# shutdown  
S1(config-if)# no shutdown  
### Примечание: При этом предполагается, что устройство/интерфейс с нарушающим MAC-адресом был удален и заменен исходной конфигурацией устройства/интерфейса.  
b. Из R1 снова выполните поиск PC-A. На этот раз вы должны добиться успеха.  
R1# ping 192.168.1.10   
### Шаг 5: Удалите базовую защиту порта на S1 F0/5.  
С консоли S1 снимите защиту порта на F0/5. Эта процедура также может быть использована для повторного включения порта, но команды port security  должны быть перенастроены.  
S1(config)# interface f0/5  
S1(config-if)# no switchport port-security  
S1(config-if)# no switchport port-security mac-address fc99.4775.c3e1  
Вы также можете использовать следующие команды для возврата интерфейса к настройкам по умолчанию:  
S1(config)# default interface f0/5  
S1(config)# interface f0/5  
Примечание: Эта команда default interface также требует, чтобы вы перенастроили порт в качестве порта доступа, чтобы повторно включить команды безопасности.    
### Шаг 6: (Необязательно) Настройте безопасность портов для VoIP.  
 В этом примере показана типичная конфигурация безопасности порта для голосового порта. Разрешены три MAC-адреса, которые должны быть изучены динамически. Один MAC-адрес предназначен для IP-телефона, один - для коммутатора и один - для ПК, подключенного к IP-телефону. Нарушения этой политики приводят к закрытию порта. Время ожидания устаревания для изученных MAC-адресов установлено равным двум часам.    
В следующем примере отображается порт S2 F0/18:  
S2(config)# interface f0/18  
S2(config-if)# switchport mode access  
S2(config-if)# switchport port-security  
S2(config-if)# switchport port-security maximum 3  
S2(config-if)# switchport port-security violation shutdown  
S2(config-if)# switchport port-security aging time 120  
### Шаг 7: Отключите неиспользуемые порты на S1 и S2.  
В качестве дополнительной меры безопасности отключите порты, которые не используются на коммутаторе.  
a. Порты F0/1, F0/5 и F0/6 используются на S1. Остальные порты Fast Ethernet и два порта Gigabit Ethernet будут отключены.  
S1(config)# interface range f0/2 - 4  
S1(config-if-range)# shutdown  
S1(config-if-range)# interface range f0/7 - 24  
S1(config-if-range)# shutdown  
S1(config-if-range)# interface range g0/1 - 2  
S1(config-if-range)# shutdown  
b. Порты F0/1 и F0/18 используются на S2. Остальные порты Fast Ethernet и порты Gigabit Ethernet будут отключены. 
S2(config)# interface range f0/2 – 17, f0/19 – 24, g0/1 - 2  
S2(config-if-range)# shutdown  
### Шаг 8: Переместите активные порты в VLAN, отличную от VLAN 1 по умолчанию.  
В качестве дополнительной меры безопасности вы можете переместить все активные порты конечного пользователя и порты маршрутизатора в VLAN, отличную от VLAN 1 по умолчанию на обоих коммутаторах.  
a. Настройте новую VLAN для пользователей на каждом коммутаторе, используя следующие команды:  
S1(config)# vlan 20   
S1(config-vlan)# name Users  

S2(config)# vlan 20  
S2(config-vlan)# name Users  
b. Добавьте текущие активные порты доступа (не магистральные) в новую VLAN.  
S1(config)# interface f0/6  
S1(config-if-range)# switchport access vlan 20  

S2(config)# interface f0/18  
S2(config-if)# switchport access vlan 20  
### Примечание: Это предотвратит связь между хостами конечных пользователей и IP-адресом VLAN управления коммутатора, который в настоящее время является VLAN 1. Доступ к коммутатору по-прежнему можно получить и настроить с помощью консольного подключения.
Примечание: Чтобы обеспечить SSH-доступ к коммутатору, определенный порт может быть назначен в качестве порта управления и добавлен в VLAN 1 с подключенной конкретной рабочей станцией управления. Более сложным решением является создание новой VLAN для управления коммутаторами (или использование существующей собственной магистральной VLAN 99) и настройка отдельной подсети для управляющих и пользовательских VLAN. 
### Шаг 9: Настройте порт с помощью функции PVLAN Edge.
Некоторые приложения требуют, чтобы трафик не пересылался на уровне 2 между портами на одном и том же коммутаторе, чтобы один сосед не видел трафик, генерируемый другим соседом. В такой среде использование пограничной функции частной VLAN (PVLAN), также известной как защищенные порты, гарантирует отсутствие обмена одноадресным, широковещательным или многоадресным трафиком между этими портами коммутатора. Пограничная функция PVLAN может быть реализована только для портов на одном коммутаторе и является локально значимой.  
Например, чтобы предотвратить трафик между хостом PC-A на S1 (порт F0/6) и хостом на другом порту S1 (например, порт F0/7, который ранее был отключен), вы можете использовать команду switchport protected для активации пограничной функции PVLAN на этих двух портах. Используйте команду настройки защищенного интерфейса no switchport, чтобы отключить защищенный порт.  
a. Настройте функцию PVLAN Edge в режиме настройки интерфейса, используя следующие команды:  
S1(config)# interface f0/6    
S1(config-if)# switchport protected    
S1(config-if)# interface f0/7    
S1(config-if)# switchport protected    
S1(config-if)# no shut    
S1(config-if)# end    
b. Убедитесь, что пограничная функция PVLAN (protected port) включена на F0/6.    
S1# show interfaces f0/6 switchport  
Name: Fa0/6  
Switchport: Enabled  
Administrative Mode: dynamic auto  
Operational Mode: static access  
Administrative Trunking Encapsulation: dot1q  
Negotiation of Trunking: On  
Access Mode VLAN: 20 (Users)  
Trunking Native Mode VLAN: 1 (default)  
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
Protected: true   
Unknown unicast blocked: disabled  
Unknown multicast blocked: disabled  
Appliance trust: none  
c. Деактивируйте защищенный порт на интерфейсах F0/6 и F0/7, используя следующие команды:  
S1(config)# interface range f0/6 - 7  
S1(config-if-range)# no switchport protected  




  
  



  
       
       
       
