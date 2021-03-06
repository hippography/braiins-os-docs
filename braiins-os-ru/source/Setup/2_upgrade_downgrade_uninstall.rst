################################
Обновление, даунгрейд и удаление
################################

.. contents::
	:local:
	:depth: 2

.. _upgrade_bos:

*******************
Обновления прошивки
*******************

Процесс обновления прошивки использует стандартный механизм установки/обновления пакетов программного обеспечения в любой системе на базе OpenWrt.
Выполните следующие действия, чтобы произвести обновление прошивки.

Обновление через веб-интерфейс
==============================

Прошивка периодически проверяет наличие новой версии и автоматически обновляет систему. В случае, если функция автообновления отключена, синяя кнопка **Upgrade** появляется с правой стороны верхней панели. Нажмите кнопку и подтвердите, чтобы начать обновление.

Кроме того, вы можете обновить информацию о хранилище вручную, нажав кнопку *Update lists* в меню System > Software menu. Если кнопка отсутствует, попробуйте обновить страницу. Чтобы запустить процесс обновления, введите ``firmware`` в поле *Download and install package* и нажмите *OK*.

Обновление через SSH
====================

После подключения к майнеру через SSH обновление до последней версии можно запустить с помощью следующих команд:

::

  opkg update && opkg install firmware

Поскольку при установке прошивки происходит перезагрузка, ожидается следующий вывод:

::

  ...
  Collected errors:
  * opkg_conf_load: Could not lock /var/lock/opkg.lock: Resource temporarily unavailable.
    Saving config files...
    Connection to 10.10.10.1 closed by remote host.
    Connection to 10.10.10.1 closed.

.. _upgrade_community_bos_plus:

**********************
Переход на Braiins OS+
**********************

Чтобы обновить старую версию прошивки или open-source версию Общественного релиза до Braiins OS +, подключитесь к майнеру по SSH
и используйте следующие команды:

::

    opkg update && opkg install bos_plus

.. _downgrade_bos_plus_community:

********************************************
Обновление / Даунгрейд на Общественный Релиз
********************************************

Для обновления c более старой версии Braiins OS или перехода с версии Braiins OS+ подключитесь к майнеру через
SSH и используйте следующую команду (замените ``IP_ADDRESS`` соответственно):

::

  ssh root@IP_ADDRESS 'wget -O /tmp/firmware.tar https://feeds.braiins-os.org/am1-s9/firmware_2020-03-29-0-6ec1a631_arm_cortex-a9_neon.tar && sysupgrade -F /tmp/firmware.tar'

.. _downgrade_bos_stock:

************************************
Сброс до начальной версии Braiins OS
************************************

Текущий пакет прошивки может быть даунгрейдован до версии, которая была изначально установлена при замене стоковой прошивки. Это можно сделать с помощью

-  *кнопки IP SET* - жмите на нее *10сек* пока красный светодиод не начнет мигать
-  *SD карты* - измените *uEnv.txt* файл так, чтобы он содержал строку **factory_reset=yes**
-  *miner utility* - вызовите ``miner factory_reset`` в командной строке майнера (во то время как SSH подключен)
-  *opkg package* - вызовите ``opkg remove firmware`` в командной строке майнера (во то время как SSH подключен)

*******************************
Перепрошивка заводской прошивки
*******************************

Использование ранее созданной резервной копии
=============================================

По умолчанию резервная копия исходной прошивки создается во время миграции на Braiins OS и может быть восстановлена с помощью следующих команд (замените ``BACKUP_ID_DATE`` и ``IP_ADDRESS`` соответственно):

::

  cd ~/braiins-os_am1-s9_ssh_2019-02-21-0-572dd48c_2020-03-29-0-6ec1a631 && source .env/bin/activate
  python3 restore2factory.py backup/BACKUP_ID_DATE/ IP_ADDRESS

Использование заводского образа прошивки
========================================

На Antminer S9 вы можете альтернативно прошить заводской образ прошивки с сайта производителя, где ``FACTORY_IMAGE`` это путь к файлу или URL к ``tar.gz`` (не извлеченному!). Поддерживаемые образы с соответствующими хэшами MD5 перечислены в `platform.py <https://github.com/braiins/braiins/blob/master/braiins-os/upgrade/am1/platform.py>`__
file.

Запустите (замените ``BACKUP_ID_DATE`` и ``IP_ADDRESS`` соответственно):

::

  cd ~/braiins-os_am1-s9_ssh_2019-02-21-0-572dd48c_2020-03-29-0-6ec1a631 && source .env/bin/activate
  python3 restore2factory.py --factory-image FACTORY_IMAGE IP_ADDRESS
