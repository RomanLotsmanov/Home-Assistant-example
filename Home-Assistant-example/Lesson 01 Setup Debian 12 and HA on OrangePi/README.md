# Установка OS Debian 12 + Home Assistant на мини компьютер OrangePi

> **Внимание!** Команды по установке Home Assistant со временем могут менятся и дополняться розработчиками Home Assistant. Установка актуальна для версии Home Assistant 2024.06-07.

Для мини компьютеров с интерфейсом M2 установка возможна на eMMC и на NVME.

## Оборудование необходимое для установки

1. Мини компьютер OrangePi (например 3B)
2. Модуль память eMMC OrangePi либо модуль NVME
3. micro SD card на 16 Gb желательно
4. ПК (персональный компьютер) например с Windows
5. Картридер для подключения и записи образа ос ОС на ПК.
6. Желателен корпус с радиаторами для охлажения мини компьютера.
7. Провод USB type C и HDMI для подачи питания на мини компьютер и получения с него изображения на мониторе.
8. Монитор с поддержкой HDMI (необязателен т.к. ip адрес можно подсмотреть на роутере в списке выданных адресов)

## Все дополнительные программы и утилиты необходимые для установки и сипользованные в ролике.

1. Программа для форматирования micro SD card [SD Memory Card Formatter](https://www.sdcard.org/downloads/formatter/)
2. Программа для записи образа на micro SD card [Balena Etcher](https://github.com/balena-io/etcher/releases)
3. Программа для удаленного доступа по SSH [Putty](https://www.putty.org/)
4. Программа для управления файлами по SCP [WinScp](https://winscp.net/eng/downloads.php)

## Подготовка образа OS Debian 12 и мини компьютера OrangePi

> Описание самого мини компьютера OrangePi включая расположение разьемов, подключение вентилятора и порядок сборки корпуса представлен в официальной документации, которая представлена на официальном сайте OrangePi
>
> [OrangePi](http://www.orangepi.org/) в разделе Download - User manual

1. Скачать образ OS Debian 12 bookworm server с официального сайта OrangePi в разделе Download - Official Images - Debian Images (имя файла должно быть примерно такое: Orangepi3b_1.0.6_debian_bookworm_server_linux5.10.160.7z)
2. Подключать micro SD card в ПК через кард ридер и отформатировать ее программой SD Memory Card Formatter
3. С помощью программы Balena Etcher осуществить запись скачаного образа на micro SD card.
4. После окончания записи образа на micro SD card отключить её из ПК и подключить в мини компьютер OrangePi.
5. Подключить к мини компьютеру OrangePi и роутеру кабель локальной сети, а так же желаетльно HDMI кабель к монитору.
6. Подключить к мини компьютеру OrangePi кабель USB type C и подать питание.
7. Ждем пока мини компьютер OrangePi загрузится контролирую процесс с помощью монитора или проверяем выдачу нового ip адреса на роутере.

## Установка OS Debian 12 и мини компьютер OrangePi

1. С помощью ip адреса мини компьютера OrangePi полученного на предыдущем этапе подключаемся к мини компьютеру OrangePi по SSH в командной строке либо с помощью утилиты Putty.

   ```
   ssh orangepi@192.168.x.x
   ```
2. С помощью утилиты WinSCP подключаемся к мини компьютеру OrangePi по протоколу **SCP** и закачиваем образ OS Debian 12 на мини компьютер OrangePi (по умолчанияв каталог /home/orangepi). **Внимание!!! образ должен быть распакован из файла 7z и быть с расширением img.**
3. С помощью команды

   ```
   lsblk
   ```

   проверяем, что на мини компьютере OrangePi корректно видно устройство eMMC модуля памяти. В случае NVME устройств, его присутствие проверяется командой lspci (инструкция для NVME в фоициально документации, раздел 2.6.2.
   The method of using the dd command to burn).
4. Как только образ img полностью закачался на мини компьютер OrangePi убеждаемся в том что он присутствует на мини компьютере с помощью команды

   ```
   ls
   ```
5. Выполняем команду

   ```
   ls /dev/mmcblk*boot0 | cut -c1-12
   ```
6. Выполняем команду зануления памяти eMMC

   ```
   sudo dd bs=1M if=/dev/zero of=/dev/mmcblk0 count=1000 status=progress
   sudo sync
   ```
7. Выполняем команду непосредственного переноса образа на внутреннюю память (имя образа можно прдсмотреть и скопировать с пункта 4 поле команды ls, в нашем случае Orangepi3b_1.0.6_debian_bookworm_server_linux5.10.160.img) eMMC

   ```
   sudo dd bs=1M if=Orangepi3b_1.0.6_debian_bookworm_server_linux5.10.160.img of=/dev/mmcblk0 status=progress
   sudo sync
   ```
8. Выключаем мини компьютер OrangePi командой

   ```
   sudo poweroff
   ```
9. OS Debian 12 успешно установлена, можно извлекать micro SD card.
10. Включаем мини компьютер OrangePi и проверяем, чтол все загружается и работает.

## Установка Home Assistant на мини компьютер OrangePi

> Актуальные команды по установке Home Assistant нужно сверять на оффициальном сайте
>
> [Home Assistant Linux supervised installation](https://github.com/home-assistant/supervised-installer)

1. С помощью ip адреса мини компьютера OrangePi полученного на предыдущем этапе подключаемся к мини компьютеру OrangePi по SSH в командной строке либо с помощью утилиты Putty.

   ```
   ssh orangepi@192.168.x.x
   ```
2. Первое что нужно сделать обновить пакеты в OS

   ```
   sudo apt update
   sudo apt upgrade
   ```
3. Далее выполняем установку пакетов как указано в официальной документации (Step 1: Install the following dependencies with this command:)

   ```
   sudo apt install
   apparmor
   bluez
   cifs-utils
   curl
   dbus
   jq
   libglib2.0-bin
   lsb-release
   network-manager
   nfs-common
   systemd-journal-remote
   systemd-resolved
   udisks2
   wget -y
   ```
4. Устанавливаем Docker CE

   ```
   curl -fsSL https://get.docker.com -o get-docker.sh
   sudo sh ./get-docker.sh
   ```
5. Устанавливаем OS агент Home Assistant необходимо найти необходимый файл на [страничке агента](https://github.com/home-assistant/os-agent/releases/latest) для нашего мини компьютера нужен файл с архитектурой aarch копируем на него ссылку в браузере и закачиваем его на мини компьютер с помощью команды (имя файла может отличатся):

   ```
   wget https://github.com/home-assistant/os-agent/releases/download/1.6.0/os-agent_1.6.0_linux_aarch64.deb
   ```
6. Далее устанавливаем скачанный пакет с помощбю команды (имя файла может отличатся):

   ```
   sudo dpkg -i os-agent_1.6.0_linux_aarch64.deb
   ```
7. Далее устанавливаем сам Home Assistant:

   ```
   wget -O homeassistant-supervised.deb https://github.com/home-assistant/supervised-installer/releases/latest/download/homeassistant-supervised.deb
   sudo apt install ./homeassistant-supervised.deb
   ```
8. При появлении диалога выбора процессора выбираем **raspberrypi4-64 для OrangePi 3B**
9. Ждем пока появится сообщение от установщика с сылкой на вэб интерфейс Home Assistant. Вэб интерфейс можен не прогружатся во время установки, установка на различиные модели мини компьютеров занимает от 5 до 25 мин.
10. Проверить, что происходит на мини компьютере во время установки можно командой

    ```
    htop
    ```

## На этом все! Если чтото идет не так нужно в первую очередь сверятся с официальной документацией.
