# Операционные системы UNIX/Linux (Базовый).

Установка и обновления системы Linux. Основы администрирования.


## Contents

  1 [Установка ОС](#part-1-установка-ос)  
  2 [Создание пользователя](#part-2-создание-пользователя)  
  3 [Настройка сети ОС](#part-3-настройка-сети-ос)   
  4 [Обновление ОС](#part-4-обновление-ос)  
  5 [Использование команды  sudo](#part-5-использование-команды-sudo)  
  6 [Установка и настройка службы времени](#part-6-установка-и-настройка-службы-времени)  
  7 [Установка и использование текстовых редакторов](#part-7-установка-и-использование-текстовых-редакторов)  
  8 [Установка и базовая настройка сервиса SSHD](#part-8-установка-и-базовая-настройка-сервиса-sshd)   
  9 [Установка и использование утилит top, htop](#part-9-установка-и-использование-утилит-top-htop)   
  10 [Использование утилиты fdisk](#part-10-использование-утилиты-fdisk)   
  11 [Использование утилиты df](#part-11-использование-утилиты-df)    
  12 [Использование утилиты du](#part-12-использование-утилиты-du)    
  13 [Установка и использование утилиты ncdu](#part-13-установка-и-использование-утилиты-ncdu)    
  14 [Работа с системными журналами](#part-14-работа-с-системными-журналами)     
  15 [Использование планировщика заданий CRON](#part-15-использование-планировщика-заданий-cron)    

## Part 1. Установка ОС

**== Задание ==**

### Установить Ubuntu 20.04 Server LTS без графического интерфейса. (Используем программу для виртуализации - VirtualBox)

**== Ответ ==**

- Ubuntu 20.04 Server LTS без графического интерфейса

![01_1](src/screens/01_1.png)

## Part 2. Создание пользователя

**== Задание ==**

##### Создать пользователя, отличного от пользователя, который создавался при установке. Пользователь должен быть добавлен в группу `adm`.

- Вставьте скриншот вызова команды для создания пользователя
- Новый пользователь должен быть в выводе команды `cat /etc/passwd`
- Вставьте скриншот с выводом команды

**== Ответ ==**

- Создание нового пользователя `sudo adduser test` и Добавление его в группу adm `sudo usermod -aG adm test`

![02_1](src/screens/02_1.png)

- Проверка внесения пользователя в группу adm `vi /etc/group`

![02_2](src/screens/02_2.png)

- Новый пользователь должен быть в выводе команды `cat /etc/passwd`

![02_3](src/screens/02_3.png)

## Part 3. Настройка сети ОС

**== Задание ==**

##### Задать название машины вида user-1  
##### Установить временную зону, соответствующую вашему текущему местоположению.  
##### Вывести названия сетевых интерфейсов с помощью консольной команды.
- В отчёте дать объяснение наличию интерфейса lo.  
##### Используя консольную команду получить ip адрес устройства, на котором вы работаете, от DHCP сервера. 
- В отчёте дать расшифровку DHCP.  
##### Определить и вывести на экран внешний ip-адрес шлюза (ip) и внутренний IP-адрес шлюза, он же ip-адрес по умолчанию (gw). 
##### Задать статичные (заданные вручную, а не полученные от DHCP сервера) настройки ip, gw, dns (использовать публичный DNS серверы, например 1.1.1.1 или 8.8.8.8).  
##### Перезагрузить виртуальную машину. Убедиться, что статичные сетевые настройки (ip, gw, dns) соответствуют заданным в предыдущем пункте.  

- В отчёте опишите, что сделали для выполнения всех семи пунктов (можно как текстом, так и скриншотами).
- Успешно пропинговать удаленные хосты 1.1.1.1 и ya.ru и вставить в отчёт скрин с выводом команды. В выводе команды должна быть фраза "0% packet loss".

**== Ответ ==**

1) Изменение имени машины

  Смена старого имени `sudo nano /etc/hostname` на новое: `sudo hostnamectl set-hostname user-1`

  ![03_1](src/screens/03_1.png)

2) Установка временной зоны `sudo timedatectl set-timezone Asia/Novosibirsk`

  ![03_2](src/screens/03_2.png)

3) Вывод названия сетевых интерфейсов с помощью консольной команды

  `ip addr show`

  ![03_3](src/screens/03_3.png)

  lo — это loopback device, специальный виртуальный интерфейс, который система использует, чтобы общаться самой с собой. Благодаря lo даже без подключения к сети локальные приложения могут взаимодействовать друг с другом.

4) Получение ip устройства от DHCP сервера `sudo dhclient -v`

  ![03_4](src/screens/03_4.png)

DHCP (англ. Dynamic Host Configuration Protocol — протокол динамической настройки узла) — сетевой протокол, позволяющий сетевым устройствам автоматически получать IP-адрес и другие параметры, необходимые для работы в сети TCP/IP. Данный протокол работает по модели «клиент-сервер». Для автоматической конфигурации компьютер-клиент на этапе конфигурации сетевого устройства обращается к так называемому серверу DHCP и получает от него нужные параметры.

5) Внешний и внутренний ip адрес шлюза

  - Внешний ip шлюза `wget eth0.me -qO -` 

  ![03_5](src/screens/03_5.png)

  - Внутренний ip шлюза `hostname -I`

  ![03_6](src/screens/03_6.png)

6) Задание статичных настроек ip, gw, dns

  Изменим конфигурационный файл интерфейса `sudo nano /etc/network/interfaces`

  ![03_7](src/screens/03_7.png)

  Перезапуск сетевых сервисов `sudo systemctl restart networking`

7) Пингование удаленных хостов 1.1.1.1 и ya.ru

  - `ping -c 4 1.1.1.1` 
  - `ping -c 4 ya.ru`

  ![03_8](src/screens/03_8.png)

  
## Part 4. Обновление ОС

**== Задание ==**

##### Обновить системные пакеты до последней на момент выполнения задания версии.  

- После обновления системных пакетов, если ввести команду обновления повторно, должно появится сообщение, что обновления отсутствуют.
- Вставить скриншот с этим сообщением в отчёт.

**== Ответ ==**

Для обновления системных пакетов используем команду  `sudo apt update && sudo apt upgrade`

После повторного введения команды получим следующий результат:

![04_1](src/screens/04_1.png)


## Part 5. Использование команды **sudo**

**== Задание ==**

##### Разрешить пользователю, созданному в [Part 2](#part-2-создание-пользователя), выполнять команду sudo.

- В отчёте объяснить *истинное* назначение команды sudo (про то, что это слово - "волшебное", писать не стоит).  
- Поменять hostname ОС от имени пользователя, созданного в пункте [Part 2](#part-2-создание-пользователя) (используя sudo).
- Вставить скрин с изменённым hostname в отчёт.

**== Ответ ==**

  1) Даем права на sudo пользователю test командой `sudo usermod -aG sudo test`

  2) Команда sudo предоставляет возможность выполнять команды от имени суперпользователя (root) с правами доступа к системным ресурсам и файлам. Это делает ее мощным инструментом для управления системой

  3) Обновление hostname: `su - test`, `sudo hostnamectl set-hostname new_host_name`, `hostname`

  ![05_1](src/screens/05_1.png)
  

## Part 6. Установка и настройка службы времени

**== Задание ==**

##### Настроить службу автоматической синхронизации времени.  

- Вывести время, часового пояса, в котором вы сейчас находитесь.
- Вывод следующей команды должен содержать `NTPSynchronized=yes`: \
  `timedatectl show`
- Вставить скрины с корректным временем и выводом команды в отчёт.

**== Ответ ==**

  1) Вывод времени, часового пояса, в котором сейчас находимся (`date`):

  2) Вывод следующей команды (`timedatectl show`) содержит `NTPSynchronized=yes`:

![06_1](src/screens/06_1.png)

## Part 7. Установка и использование текстовых редакторов 

**== Задание ==**

##### Установить текстовые редакторы **VIM** (+ любые два по желанию **NANO**, **MCEDIT**, **JOE** и т.д.)  
##### Используя каждый из трех выбранных редакторов, создайте файл *test_X.txt*, где X -- название редактора, в котором создан файл. Напишите в нём свой никнейм, закройте файл с сохранением изменений.  
- В отчёт вставьте скриншоты:
  - Из каждого редактора с содержимым файла перед закрытием.
- В отчёте укажите, что сделали для выхода с сохранением изменений.
##### Используя каждый из трех выбранных редакторов, откройте файл на редактирование, отредактируйте файл, заменив никнейм на строку "21 School 21", закройте файл без сохранения изменений.
- В отчёт вставьте скриншоты:
    - Из каждого редактора с содержимым файла после редактирования.
- В отчёте укажите, что сделали для выхода без сохранения изменений.
##### Используя каждый из трех выбранных редакторов, отредактируйте файл ещё раз (по аналогии с предыдущим пунктом), а затем освойте функции поиска по содержимому файла (слово) и замены слова на любое другое.
- В отчёт вставьте скриншоты:
    - Из каждого редактора с результатами поиска слова.
    - Из каждого редактора с командами, введёнными для замены слова на другое.

**== Ответ ==**

  Установка всех редакторов - nano,vim,joe

  ![07_1](src/screens/07_1.png) 

  1) Создание файла
   - при помощи nano `nano test_nano.txt`

     ![07_2](src/screens/07_2.png) 

     для выхода с сохранением нажимаем ^x, y, enter

   - при помощи vim `vim test_vim.txt` (для ввода текста нажимаем i)

     ![07_3](src/screens/07_3.png) 

     для выхода с сохранением нажимаем esc :wq

   - создаем файл при помощи joe `joe test_joe.txt`

     ![07_4](src/screens/07_4.png) 

     для выхода с сохранением нажимаем ctrl+k, x
 
 2) Выход без сохранения
   - открываем файл при помощи nano `nano test_nano.txt`

   отредактированный файл:

   ![07_5](src/screens/07_5.png)

   файл без сохранения изменений:

   ![07_6](src/screens/07_6.png)

   для выхода без сохранения нажимаем ^x, n

  - открываем файл при помощи vim `vim test_vim.txt`

   отредактированный файл:

   ![07_7](src/screens/07_7.png)

   файл без сохранения изменений:

   ![07_8](src/screens/07_8.png)

   для выхода без сохранения нажимаем esc :q! enter

  - открываем файл при помощи joe `joe test_joe.txt`

   отредактированный файл:

   ![07_9](src/screens/07_9.png)

   файл без сохранения изменений:

   ![07_10](src/screens/07_10.png)

   для выхода без сохранения нажимаем ^k q n  
  
3) Поиск и замена слова. Во всех трех файлай никнейм предварительно заменен на "21 School 21" по аналогии с предыдущим пунктом.

  - открываем файл при помощи nano `nano test_nano.txt`\
   для поиска используем ^w \
   для замены используем ^\ \

   результат поиска слова "School" заменяем на "Great"

   ![07_11](src/screens/07_11.png)

  - открываем файл при помощи vim `vim test_vim.txt`\
   для поиска используем :s/ \
   для замены используем :s/предыдущий текст/новый текст\
   результат поиска слова "School" заменяем на "Best":

   результат замены слова:
   
   ![07_12](src/screens/07_12.png)


  - открываем файл при помощи joe `joe test_joe.txt`\
   для поиска используем ^k f \
   для замены используем ^ k f enter r enter y \
   результат поиска слова "School":

   результат поиска слова "School" заменяем на "Best":

   ![07_13](src/screens/07_13.png)

## Part 8. Установка и базовая настройка сервиса **SSHD**

**== Задание ==**

##### Установить службу SSHd.  
##### Добавить автостарт службы при загрузке системы.  
##### Перенастроить службу SSHd на порт 2022.  
##### Используя команду ps, показать наличие процесса sshd. Для этого к команде нужно подобрать ключи.
- В отчёте объяснить значение команды и каждого ключа в ней.
##### Перезагрузить систему.
- В отчёте опишите, что сделали для выполнения всех пяти пунктов (можно как текстом, так и скриншотами).
- Вывод команды netstat -tan должен содержать  \
`tcp 0 0 0.0.0.0:2022 0.0.0.0:* LISTEN`  \534
- Скрин с выводом команды вставить в отчёт.
- В отчёте объяснить значение ключей -tan, значение каждого столбца вывода, значение 0.0.0.0.

**== Ответ ==**

1) `sudo apt install openssh-server` (Установка службы SSHd)

2) `sudo systemctl enable ssh` (Автостарт при загрузке системы)
 - служба SSHd работает

 ![08_1](src/screens/08_1.png)

3) Для изменения порта на 2022 требуется отредактировать файл `sudo nano /etc/ssh/sshd_config`

 ![08_2](src/screens/08_2.png)

 для применения изменений: `sudo service ssh restart`

4) Вывод процесса ssh `ps -ef | grep ssh`

 ![08_3](src/screens/08_3.png)

 - команда ps отображает список текущих процессов
 - флаг -е показывает все процессы, а -f показывает полную информацию
 - sshd - это служба, принимающая запросы на соединения от клиентов. Обычно она запускается при загрузке системы из /etc/rc. Для каждого нового соединения создаётся новый экземпляр службы. Ответвлённый экземпляр обрабатывает обмен ключами, шифрование, аутентификацию, выполнение команд и обмен данными.

5) Вывод команды `netstat -tan` содержит `tcp 0 0 0.0.0.0:2022 0.0.0.0:* LISTEN`

 ![08_4](src/screens/08_4.png)

 - netstat(network status) отображает активные TCP-соединения, порты, которые прослушивает компьютер, статистику Ethernet, таблицу IP-маршрутизации.
 - Ключи tan: 
   - -t отображение только TCP-портов
   - -a отображение всех открытых портов (как TCP, так и UDP)
   - -n отображение портов в числовом формате, без имен хостов и сервисов

- Значение столбцов вывода: 
    - Proto - название протокола 
    - Recv-Q - очередь получения сети 
    - Send-Q - сетевая очередь отправки 
    - Local Address - локальный IP-адрес, участвующий в соединении или связанный со службой, участвующей в соединении 
    - Foreign Address - внешний IP-адрес, учавствующий в создании соединения
    - State - состояние соединение
- Значение 0.0.0.0 - означает, что в соединении могут использоваться все IP-адреса на локальном компьютере.

## Part 9. Установка и использование утилит **top**, **htop**

**== Задание ==**

##### Установить и запустить утилиты top и htop.  

- По выводу команды top определить и написать в отчёте:
  - uptime
  - количество авторизованных пользователей
  - общую загрузку системы
  - общее количество процессов
  - загрузку cpu
  - загрузку памяти
  - pid процесса занимающего больше всего памяти
  - pid процесса, занимающего больше всего процессорного времени
- В отчёт вставить скрин с выводом команды htop:
  - отсортированному по PID, PERCENT_CPU, PERCENT_MEM, TIME
  - отфильтрованному для процесса sshd
  - с процессом syslog, найденным, используя поиск 
  - с добавленным выводом hostname, clock и uptime  

**== Ответ ==**

- Вывод команды top:

 ![09_1](src/screens/09_1.png)

 сортировка по памяти Shift + M

 ![09_2](src/screens/09_2.png)

  - uptime: 6 min
  - количество авторизованных пользователей: 1
  - общую загрузку системы: 0.05
  - общее количество процессов: 138
  - загрузку cpu: 0%
  - загрузку памяти: 177.9
  - pid процесса занимающего больше всего памяти:  753
  - pid процесса, занимающего больше всего процессорного времени: 753

- Вывод команды htop: сортировка F6, фильтр F4, поиск F3, добавить вывод F2

  - отсортированный по PID

  ![09_3](src/screens/09_3.png)

  - отсортированный по PERCENT_CPU

  ![09_4](src/screens/09_4.png)

  - отсортированный по PERCENT_MEM

  ![09_5](src/screens/09_5.png)

  - отсортированный по TIME

  ![09_6](src/screens/09_6.png)

  - отфильтрованному для процесса sshd

  ![09_7](src/screens/09_7.png)

  - с процессом syslog, найденным, используя поиск 

  ![09_8](src/screens/09_8.png)

  - с добавленным выводом hostname, clock и uptime 

  ![09_9](src/screens/09_9.png)


## Part 10. Использование утилиты **fdisk**

**== Задание ==**

##### Запустить команду fdisk -l.

- В отчёте написать название жесткого диска, его размер и количество секторов, а также размер swap.

**== Ответ ==**

- `sudo fdisk -l`

 ![10_1](src/screens/10_1.png)

   - название жесткого диска: sda 
   - размер 25 GiB
   - количество секторов 52428800
   - размер swap  0 GB

   ![10_2](src/screens/10_2.png)

## Part 11. Использование утилиты **df** 

**== Задание ==**

##### Запустить команду df.  
- В отчёте написать для корневого раздела (/):
  - размер раздела
  - размер занятого пространства
  - размер свободного пространства
  - процент использования
- Определить и написать в отчёт единицу измерения в выводе.  

##### Запустить команду df -Th.
- В отчёте написать для корневого раздела (/):
    - размер раздела
    - размер занятого пространства
    - размер свободного пространства
    - процент использования
- Определить и написать в отчёт тип файловой системы для раздела.

**== Ответ ==**
- Запущена команда df

 ![11_1](src/screens/11_1.png)

- Для корневого раздела (/):
  - размер раздела: 11758760
  - размер занятого пространства: 3158344
  - размер свободного пространства: 7981308
  - процент использования: 29%
- Единица измерения: Килобайт

- Запущена команда df -Th

 ![11_2](src/screens/11_2.png)

- Для корневого раздела (/):
    - размер раздела: 13 ГБ
    - размер занятого пространства: 3,3 ГБ
    - размер свободного пространства: 8,2 ГБ
    - процент использования: 29%
- Тип файловой системы для раздела: ext4

## Part 12. Использование утилиты **du**  

**== Задание ==**

##### Запустить команду du.
##### Вывести размер папок /home, /var, /var/log (в байтах, в человекочитаемом виде)
##### Вывести размер всего содержимого в /var/log (не общее, а каждого вложенного элемента, используя *)

- В отчёт вставить скрины с выводом всех использованных команд.

**== Ответ ==**

 - Запущена команда du

 ![12_1](src/screens/12_1.png)

 - Размер папок /home, /var, /var/log
   - в байтах `sudo du -s /home && sudo du -s /var && sudo du -s /var/log`

    ![12_2](src/screens/12_2.png)

   - в человекочитаемом виде `sudo du -sh /home && sudo du -sh /var && sudo du -sh /var/log`

    ![12_3](src/screens/12_3.png)
    
 - Размер всего содержимого в /var/log `du -sh /var/log/*`

    ![12_4](src/screens/12_4.png)
  

## Part 13. Установка и использование утилиты **ncdu**

**== Задание ==**

##### Установить утилиту ncdu.
##### Вывести размер папок /home, /var, /var/log.

- Размеры должны примерно совпадать с полученными в [Part 12](#part-12-использование-утилиты-du).

- В отчёт вставить скрины с выводом использованных команд.

**== Ответ ==**

 - Установка `sudo apt install ncdu`
 
 - `ncdu /home`

  ![13_1](src/screens/13_1.png)

 - `ncdu /var`

  ![13_2](src/screens/13_2.png)

 - `ncdu /var/log`

  ![13_3](src/screens/13_3.png)


## Part 14. Работа с системными журналами

**== Задание ==**

##### Открыть для просмотра:
##### 1. /var/log/dmesg
##### 2. /var/log/syslog
##### 3. /var/log/auth.log  

- Написать в отчёте время последней успешной авторизации, имя пользователя и метод входа в систему.
- Перезапустить службу SSHd.
- Вставить в отчёт скрин с сообщением о рестарте службы (искать в логах).

**== Ответ ==**

1) `tail /var/log/dmesg`

   ![14_1](src/screens/14_1.png)

2) `tail /var/log/syslog`

   ![14_2](src/screens/14_2.png)

3) `tail /var/log/auth.log`

   ![14_3](src/screens/14_3.png)

- Время последней успешной авторизации 3 Апреля 22:15:29
  - Имя пользователя gailfeda
  - Метод входа в систему systemd-logind
- Перезапустить службу SSHd `sudo systemctl restart sshd`
- Cкрин с сообщением о рестарте службы

 ![14_4](src/screens/14_4.png)


## Part 15. Использование планировщика заданий **CRON**

**== Задание ==**

##### Используя планировщик заданий, запустите команду uptime через каждые 2 минуты.
- Найти в системных журналах строчки (минимум две в заданном временном диапазоне) о выполнении.
- Вывести на экран список текущих заданий для CRON.
- Вставить в отчёт скрины со строчками о выполнении и списком текущих задач.

##### Удалите все задания из планировщика заданий.
- В отчёт вставьте скрин со списком текущих заданий для CRON.

**== Ответ ==**

1) Используя планировщик заданий, запустить команду uptime через каждые 2 минуты

 `crontab -e`

 ![15_1](src/screens/15_1.png)

- Найти в системных журналах строчки (минимум две в заданном временном диапазоне) о выполнении

 `tail /var/log/syslog`

 ![15_2](src/screens/15_2.png)

- Вывести на экран список текущих заданий для CRON

 `crontab -l`

 ![15_3](src/screens/15_3.png)

2) Удалить все задания из планировщика заданий

 `crontab -r`

 ![15_4](src/screens/15_4.png)
