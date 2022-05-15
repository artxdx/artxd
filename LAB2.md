# Лабораторная работ № 2   
# Настройка административных ролей     
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
Необходимые Ресурсы
* 3 маршрутизатора (Cisco 4221 с универсальным образом Cisco XE версии 16.9.6 или сопоставимым с лицензией на пакет технологий безопасности)
* 2 коммутатора (Cisco 2960+ с изображением Cisco IOS версии 15.2 (7) lanbasek9 или аналогичным)
* 2 ПК (ОС Windows с установленной программой эмуляции терминала, такой как PuTTY или Tera Term)
* Консольные кабели для настройки сетевых устройств Cisco
* Кабели Ethernet , как показано в топологии
# Выполнение лабораторной работы № 3
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
б. Чтобы маршрутизатор не пытался перевести неправильно введенные команды, как если бы они были именами хостов, отключите поиск DNS. R1 показан здесь в качестве примера.
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
a. Выполните команду show ip ospf neighbor, чтобы убедиться, что каждый маршрутизатор перечисляет другие маршрутизаторы в сети в качестве соседей.
R1# show ip ospf neighbor
Neighbor ID     Pri   State           Dead Time   Address         Interface
10.2.2.2          1   FULL/BDR        00:00:37    10.1.1.2        GigabitEthernet0/0/0
b. Выполните команду show ip route , чтобы убедиться, что все сети отображаются в таблице маршрутизации на всех маршрутизаторах.
R1# show ip route
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override, p - overrides from PfR

Gateway of last resort is not set

      10.0.0.0/8 is variably subnetted, 3 subnets, 2 masks
C        10.1.1.0/30 is directly connected, GigabitEthernet0/0/0
L        10.1.1.1/32 is directly connected, GigabitEthernet0/0/0
O        10.2.2.0/30 [110/2] via 10.1.1.2, 00:01:11, GigabitEthernet0/0/0
      192.168.1.0/24 is variably subnetted, 2 subnets, 2 masks
C        192.168.1.0/24 is directly connected, GigabitEthernet0/0/1
L        192.168.1.1/32 is directly connected, GigabitEthernet0/0/1
O     192.168.3.0/24 [110/3] via 10.1.1.2, 00:01:07, GigabitEthernet0/0/0
### Шаг 5: Настройте параметры IP-адреса хоста ПК.
Настройте статический IP-адрес, маску подсети и шлюз по умолчанию для PCA и PCC, как показано в таблице IP-адресации.
### Шаг 6: Проверьте подключение между ПК и ПК-C.
a. Выполните поиск с R1 на R3.
Если запросы не завершились успешно, устраните неполадки в основных конфигурациях устройства, прежде чем продолжить.
b. Выполните пинг с PC-A в локальной сети R1 на PCC в локальной сети R3.
Если запросы не завершились успешно, устраните неполадки в основных конфигурациях устройства, прежде чем продолжить.
Примечание: Если вы можете выполнить пинг с ПК-A на ПК-C, вы продемонстрировали, что маршрутизация OSPF настроена и функционирует правильно. 
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
Когда вы находитесь в представлении CLI, у вас есть доступ только к командам, которые были добавлены в это представление пользователем корневого представления.
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
### Шаг 4: Создайте представление admin 1, установите пароль и назначьте привилегии.
a. Пользователь admin1 - это пользователь верхнего уровня ниже root, которому разрешен доступ к этому маршрутизатору. 
Он обладает наибольшим авторитетом. Пользователь admin1 может использовать все команды show, config и debug.
Используйте следующую команду, чтобы создать представление admin 1 в корневом представлении.
R1# configure terminal
R1(config)# parser view admin1
R1(config-view)#
Примечание: Чтобы удалить представление, используйте команду no parser view viewname.
b. Свяжите представление admin1 с зашифрованным паролем.
R1(config-view)# secret admin1pass
R1(config-view)#
c. Просмотрите команды, которые можно настроить в представлении admin 1. Использовать команды? команда для просмотра доступных команд.
Ниже приведен частичный список доступных команд.
R1(config-view)# commands ?
  RITE-profile           Router IP traffic export profile command mode
  RMI Node Config        Resource Policy Node Config mode
  RMI Resource Group     Resource Group Config mode
  RMI Resource Manager   Resource Manager Config mode
  RMI Resource Policy    Resource Policy Config mode
  SASL-profile           SASL profile configuration mode
  aaa-attr-list          AAA attribute list config mode
  aaa-user               AAA user definition
  accept-dialin          VPDN group accept dialin configuration mode
  accept-dialout         VPDN group accept dialout configuration mode
  address-family         Address Family configuration mode
<output omitted>
d. Добавьте все команды config, show и debug в представление admin 1, а затем выйдите из режима просмотра конфигурации.
  R1(config-view)# commands exec include all show
R1(config-view)# commands exec include all config terminal
R1(config-view)# commands exec include all debug
R1(config-view)# end
e. Проверьте представление admin1.
  R1# enable view admin1
  Password: admin1pass
  R1# show parser view
Current view is ‘admin1’
f. Изучите команды, доступные в представлении admin1.
  R1# ?
Exec commands:
  <0-0>/<0-4>  Enter card slot/sublot number
  configure    Enter configuration mode
  debug        Debugging functions (see also 'undebug')
  do-exec      Mode-independent "do-exec" prefix support
  enable       Turn on privileged commands
  exit         Exit from the EXEC
  show         Show running system 
Примечание: Может быть доступно больше команд EXEC, чем отображается. Это зависит от вашего устройства и используемого образа IOS.
g. Изучите команды отображения, доступные в представлении admin1.
R1# show ?
  aaa                       Show AAA values
  access-expression         List access expression
  access-lists              List access lists
  acircuit                  Access circuit info
  adjacency                 Adjacent nodes
  aliases                   Display alias commands
  alignment                 Show alignment information
  appfw                     Application Firewall information
  archive                   Archive functions
  arp                       ARP table
<output omitted>
### Шаг 5: Создайте представление admin 2, установите пароль и назначьте привилегии.
a. Пользователь admin2 является младшим администратором, проходящим обучение, которому разрешено просматривать все конфигурации, но не разрешается настраивать маршрутизаторы или использовать команды отладки.
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
  R1# ?
Exec commands:
  <0-0>/<0-4>  Enter card slot/sublot number
  do-exec      Mode-independent "do-exec" prefix support
  enable       Turn on privileged commands
  exit         Exit from the EXEC
  show         Show running system information
Примечание: Может быть доступно больше команд EXEC, чем отображается. Это зависит от вашего устройства и используемого образа IOS.
Вопрос:
Чего не хватает в списке команд admin 2, который присутствует в командах admin 1?
### Шаг 6: Создайте техническое представление, установите пароль и назначьте привилегии.
a. Технический пользователь обычно устанавливает устройства конечного пользователя и кабели. Техническим пользователям разрешено использовать только выбранные команды показа.
b. Используйте команду включить просмотр, чтобы включить корневой просмотр, и введите секретный пароль включения cisco12345.
R1# enable view
Password: cisco12345
c. Используйте следующую команду для создания технического представления.
R1(config)# parser view tech
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
R1# show parser view
Current view is ‘tech’
g. Изучите команды, доступные в техническом представлении.
R1# ?
Exec commands:
  <0-0>/<0-4>  Enter card slot/sublot number
  do-exec      Mode-independent "do-exec" prefix support
  enable       Turn on privileged commands
  exit         Exit from the EXEC
  show         Show running system information
Примечание: Может быть доступно больше команд EXEC, чем отображается. Это зависит от вашего устройства и используемого образа IOS.
h. Изучите команды отображения, доступные в техническом представлении.
R1# show ?
  banner      Display banner information
  flash0:     display information about flash0: file system
  flash1:     display information about flash1: file system
  flash:      display information about flash: file system
  interfaces  Interface status and configuration
  ip          IP information
  parser      Display parser information
  usbflash0:  display information about usbflash0: file system
  version     System hardware and software status
Примечание: Может быть доступно больше команд EXEC, чем отображается. Это зависит от вашего устройства и используемого образа IOS.
i. Выполните краткую команду показать ip-интерфейс.
Вопрос:
Смогли ли вы сделать это как технический пользователь? Объяснять.
Введите свои ответы здесь.
j. Выполните команду показать ip-маршрут.
Вопрос:
Смогли ли вы сделать это как технический пользователь?
Введите свои ответы здесь.
k. Вернитесь к корневому просмотру с помощью команды включить просмотр.
R1# включить просмотр
Пароль: cisco12345
l. Выполните команду show run, чтобы просмотреть созданные вами представления.
Вопрос:
Для технического просмотра, почему перечислены команды show и show ip, а также show ip interface и show ip interface brief?
Введите свои ответы здесь.
#### m. Настройте те же административные роли на маршрутизаторе R3.
### Шаг 7: Сохраните конфигурацию на маршрутизаторах R1 и R3.
Сохраните текущую конфигурацию в конфигурации запуска из привилегированной командной строки EXEC.
Сводная таблица Интерфейса маршрутизатора
    
Примечание: Чтобы узнать, как настроен маршрутизатор, посмотрите на интерфейсы, чтобы определить тип маршрутизатора и количество интерфейсов,
которые имеет маршрутизатор. Нет никакого способа эффективно перечислить все комбинации конфигураций для каждого класса маршрутизатора. 
Эта таблица содержит идентификаторы для возможных комбинаций Ethernet и последовательных интерфейсов в устройстве.    
Таблица не включает в себя какой-либо другой тип интерфейса, даже если конкретный маршрутизатор может содержать его.
Примером этого может быть интерфейс ISDN BRI. Строка в круглых скобках - это юридическая аббревиатура, которая может использоваться 
в командах Cisco IOS для представления интерфейса.    





