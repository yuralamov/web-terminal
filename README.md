Терминал с браузером Firefox. Подойдет для установки на убогих компьютерах.

Данное "творение"  появилось после посещения детской библиотеки не в самом последнем городе РФ, а скорее во втором.

Вроде нигде не ошибся, проверял на vbox с 1G и 1 ядре.
После всех манипуляций настроен автовход user с запуском firefox в приватном окне.
При входе очищаются папки firefox.
При закрытии firefox он снова открывается.
Если какая-то заморочка, то выключаем компьютер (5 сек на кнопку включения) и снова включаем или по SSH.
Вместо винта, можно установливать на флешку, которую подключить по USB, засунуть в системник и настроить BIOS на запуск с флешки.

ОЗУ жрет после установки порядка 500М, площадь на диске - около 2G

Разбил на части - много текстовки

1. debian-base Базовая установка системы на терминал
2. locale Настройка локали
3. net-config Настройка сетевых интерфейсов
4. Установка и настройка общепрограммного обеспечения терминала
 debian login: root
 Password: Flvby_123
 apt update && apt install xinit openbox firefox-esr firefox-esr-l10n-ru -y
 Переконфигурируем x-server на Кто угодно
 /sbin/dpkg-reconfigure xserver-xorg-legacy
 Активируем и редактируем терминальный сервис
 systemctl enable getty@.service
 mkdir -p /etc/systemd/system/getty@tty1.service.d/
 nano /etc/systemd/system/getty@tty1.service.d/override.conf
 #####
 [Service]
 ExecStart=
 ExecStart=-/sbin/agetty -o '-p -f -- \\u' --noclear --autologin user %I $TERM 
 #####
6. remote-ssh Настройка удаленного управления
7. disable-sleep Отключим ненужные слипы

