# Настройка Home Assistant  HACS и VScode на мини компьютер OrangePi, исправление ошибок AppArmor и CGroup

> **Внимание!** Команды по установке Home Assistant со временем могут менятся и дополняться розработчиками Home Assistant. Установка актуальна для версии Home Assistant 2024.06-07. Мини компьютер OrangePi

## Первичные шаги по донастроке мини ПК

1. Смена стандартного (дефолтного) пароля.

   **Помните пароли должны быть сложными, запишите и сохраните их в надежном месте!**

   ```
   passwd
   ```
2. Установка корретных настроек времени, часовой пояс и синхронизация времени с серверами времени в интеренте.
   a. смотрим текущие настройки:

   ```
   timedatectl
   ```

   b. Просматриваем список таймзон

   ```
   timedatectl list-timezones
   ```

   с. Поиск по списку таймзон

   ```
   timedatectl list-timezones | grep Moscow
   ```

   d. Установка таймзоны

   ```
   timedatectl set-timezone Europe/Moscow
   ```

   e. Проверка того что таймзона установилась корректно

   ```
   timedatectl
   ```

   f. Устанавливаем дополнительный пакет длоя синхронизации времени

   ```
   sudo apt install systemd-timesyncd
   ```

   g. Проверяем статус синхронизации времени

   ```
   systemctl status systemd-timesyncd
   timedatectl
   ```
3. Установка имени хоста

   a. Просматриваем текущее имя хоста

   ```
   hostnamectl
   ```

   b. Меняем имя хоста

   ```
   hostnamectl set-hostname test-ha-orange
   ```

   c. Меняем имя хоста для работы dns

   ```
   sudo nano /etc/hosts
   ```

## Исправление ошибок AppArmor и CGroup

1. Заходим под сессию с повышенными правами
   ```
   sudo su -
   ```
2. Проверяем статус сервиса AppArmor
   ```
   systemctl status apparmor.service
   ```
3. Проверяем дополнительные параметры загрузки в файле /boot/orangepiEnv.txt
   ```
   ls /boot/
   cat /boot/orangepiEnv.txt
   ```
4. Можно создать резервную копию файла
   ```
   cp /boot/orangepiEnv.txt /boot/orangepiEnv.bak
   ```
5. Вносим изменения в файл /boot/orangepiEnv.txt
   ```
   echo "extraargs=apparmor=1 security=apparmor systemd.unified_cgroup_hierarchy=false systemd.legacy_systemd_cgroup_controller=false" >> /boot/orangepiEnv.txt
   ```
6. Проверям корретно ли внеслись изменения
   ```
   cat /boot/orangepiEnv.txt
   ```
7. Формируем загрузочный имидж файл для последующей загрузки с дополнительными параметрами
   ```
   update-initramfs -u
   ```
8. Перезагружаемся
   ```
   reboot
   ```
9. Проверяем что все исправлено
   ```
   systemctl status apparmor.service
   ```
