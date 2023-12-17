Терминал с браузером Firefox
Подойдет для установки на убогих компьютерах.
Данное "творение"  появилось после посещения детской библиотеки не в самом последнем городе РФ, а скорее во втором.
Вроде нигде не ошибся, проверял на vbox с 1G и 1 ядре. После всех манипуляций настроен автовход user с запуском firefox в приватном окне. При входе очищаются папки firefox. При закрытии firefox он снова открывается. Если какая-то заморочка, то выключаем компьютер (5 сек на кнопку включения) и снова включаем или по SSH. Вместо винта, можно установливать на флешку, которую подключить по USB, засунуть в системник и настроить BIOS на запуск с флешки.
ОЗУ жрет после установки порядка 500М, площадь на диске - около 2G
----- Базовая установка системы
1. Скачать образ debian
Например:
для 32-разрядной
wget http://mirror.mephi.ru/debian-cd/current/i386/iso-cd/debian-12.2.0-i386-netinst.iso
wget https://cdimage.debian.org/cdimage/archive/11.8.0/i386/iso-cd/debian-11.8.0-i386-netinst.iso
для 64-разрядной
wget http://mirror.mephi.ru/debian-cd/current/amd64/iso-cd/debian-12.2.0-amd64-netinst.iso
wget https://cdimage.debian.org/cdimage/archive/11.8.0/amd64/iso-cd/debian-11.8.0-amd64-netinst.iso
2. Записать на флешку с помошью rufus
на 64-разрядной Windows
https://github.com/pbatard/rufus/releases/download/v4.3/rufus-4.3.exe
на 32-разрядной Windows
https://github.com/pbatard/rufus/releases/download/v4.3/rufus-4.3_x86.exe
3. Графическая установка
- Выбор языка - Russian Русский
- Местонахождение - Российская Федерация
- Настройка клавиатуры / Выберите клавиатурную раскладку - Русская
- Настройка клавиатуры / Способ переключения между национальной и латинской раскладкой - Control+Shift
- Настройка сети / Имя компьютера - debian
- Настройка сети / Имя домена - workgroup
- Настройка учетных записей пользователей и паролей / Пароль суперпользователя (2 раза) - Flvby_123
- Настройка учетных записей пользователей и паролей / Введите полное имя нового пользователя - user
- Настройка учетных записей пользователей и паролей / Имя вашей учетной записи - user
- Настройка учетных записей пользователей и паролей / Введите пароль для нового пользователя (2 раза) - user
- Настройка времени / Выберите часовой пояс - Москва+00 - Москва
- Разметка диска / Метод разметки - Авто - использовать весь диск
- Разметка диска / Выберите диск для разметки - (Здесь выбираем рабочий диск)
- Разметка диска / Схема разметки - Все файлы в одном разделе (рекомендуется новичкам)
- Разметка диска / Закончить разметку и записать изменения на диск
- Разметка диска / Записать изменения на диск? - Да
- Разметка диска / Закончить разметку и записать изменения на диск
- Разметка диска / Записать изменения на диск? - Да
- Настройка менеджера пакетов / Просканировать дополнительный установочный носитель? - Нет
- Настройка менеджера пакетов / Страна, в которой расположено зеркало архива Debian: - Российская федерация
- Настройка менеджера пакетов / Зеркало архива Debian: - deb.debian.org
- Настройка менеджера пакетов / Информация о HTTP-прокси (если прокси нет -- не заполняйте) - пусто
- Настраивается popularity-contest / Участвовать в опросе популярности пакетов? - Нет
- Выбор программного обеспечения / Выберите устанавливаемое программное обеспечение: - Снимаем все отметки кроме SSH-сервер
- Установка системного загрузчика GRUB / Установить системный загрузчик GRUB на первичный диск? - Да
- Установка системного загрузчика GRUB / Устройство для установки системного загрузчика - (здесь выбираем ваш рабочий диск, как правило /dev/sda)
- Завершение установки / Установка завершена
----- Извлеките установочный носитель, проверьте порядок загрузки в BIOS, загрузка системы
----- Входим в систему под учетной записью root
debian login: root
Password: Flvby_123
----- Первоначальная настройка компьютера
apt update && apt upgrade -y
dpkg-reconfigure {locales,console-setup,keyboard-configuration}
- Настраивается locales / Локали, которые будут созданы - Выбираем пробелом en_US.UTF-8 UTF-8 и ru_RU.UTF-8 UTF-8, затем клавишей Tab -> Ok
- Настраивается locales / Локаль по умолчанию в системном окружении: - ru_RU.UTF-8, клавишей Tab -> Ok
- Настраивается console-setup / Используемая кодировка в консоли: - UTF-8, клавишей Tab -> Ok
- Настраивается console-setup / Используемая таблица символов - # кириллица - славянские языки (также боснийская и сербская латиница), клавишей Tab -> Ok
- Настраивается console-setup / Консольный шрифт: - Fixed, клавишей Tab -> Ok
- Настраивается console-setup / Размер шрифта: - 8x16, клавишей Tab -> Ok
- Настраивается keyboard-configuration / Модель клавиатуры: - Обычный ПК с 105-клавишной (межд.), клавишей Tab -> Ok
- Настраивается keyboard-configuration / Раскладка клавиатуры - Русская, клавишей Tab -> Ok
- Настраивается keyboard-configuration / Способ переключения между национальной и латинской раскладкой: - Control+Shift, клавишей Tab -> Ok
- Настраивается keyboard-configuration / Временный переключатель между национальной или латинской раскладкой: - нет временного переключателя, клавишей Tab -> Ok
- Настраивается keyboard-configuration / Клавиша, используемая как AltGr: - Раскладка клавиатуры по умолчанию, клавишей Tab -> Ok
- Настраивается keyboard-configuration / Составная клавиша: - нет составной клавиши, клавишей Tab -> Ok

----- Настройка удаленного управления - Входим под учеткой root
apt update && apt install ssh ufw fail2ban -y
ufw allow OpenSSH && ufw logging medium
ufw enable && ufw reload && sudo ufw status verbose
----- Узнать ip адрес и MAC
ip a
И смотрим на своем интерфейсе ip
----- Если по DHCP, то оставляем как есть. При наличии сервера DHCP/DNS настраиваем ip по MAC компьютера. MAC адрес в строке link/ether на своем интерфейсе
----- Если настраиваем статический адрес, то приводим к виду
nano /etc/network/interfaces

# enp0s3 - интерфейс
# address - ip этого компьютера
# gateway - ip роутера
# dns-nameservers - DNS сервера
iface enp0s3 inet static
    address 192.168.0.21/24
    gateway 192.168.0.1
    dns-nameservers 192.168.0.1 1.1.1.1

update-initramfs -u
reboot
----- Далее все делаем на компьютере администратора
----- Проверка удаленного входа по ssh
ssh user@192.168.0.21
exit
----- Создание и копирование ключа на терминальный компьютер
Создание ключа ssh на хосте  
ssh-keygen -t rsa -b 4096 -C "user@mail.ru"  
Добавить ключ агенту
eval $(ssh-agent -s)
ssh-add /home/user/.ssh/id_ed25519 (где созданный ключ)  
Копирование публичного ключа на терминал
ssh-copy-id user@192.168.0.21
Соглашаемся, вводим пароль
----- Установка программного обеспечения и настройка
Удаленное соединение с терминалом
ssh user@192.168.0.21
Вход под root
su root -
(Пароль Flvby_123 при установке)
nano /etc/ssh/sshd_config
В файле ищем строку 
#PasswordAuthentication yes
 и приводим к виду
PasswordAuthentication no
После этих манипуляций вход будет происходить только по ключу ssh
Сохраняем (Control+O) и выходим (Control+X)
Еще раз перезагрузим и войдем для проверки
reboot
ssh user@192.168.0.21
su root -
Отключим ненужные слипы
nano /etc/systemd/logind.conf
В файле раскоментируем и приведем строки
HandleLidSwitch=ignore
HandleLidSwitchDocked=ignore
Сохраняем, выходим
systemctl mask sleep.target suspend.target hibernate.target hybrid-sleep.target
apt update
apt install xinit openbox firefox-esr firefox-esr-l10n-ru -н
Переконфигурируем x-server на Кто угодно
/sbin/dpkg-reconfigure xserver-xorg-legacy
Создаем и редактируем терминальный сервис
systemctl enable getty@.service
mkdir -p /etc/systemd/system/getty@tty1.service.d/
nano /etc/systemd/system/getty@tty1.service.d/override.conf
[Service]
ExecStart=
ExecStart=-/sbin/agetty -o '-p -f -- \\u' --noclear --autologin user %I $TERM

Выходим из root:
exit

mkdir -p .config/openbox/
Создаем файл openbox environment
nano ~/.config/openbox/environment
#####
export PATH=$HOME/bin:$HOME/.local/bin:$PATH
export LANG=ru_RU.UTF-8
#####
Создаем файл openbox autostart
nano ~/.config/openbox/autostart
######
(firefox --fullscreen --private-window https://ya.ru) &

while 1>0
do
ps -A | grep firefox-esr > /dev/null
if [ $? = "1" ]
        then firefox --fullscreen --private-window https://ya.ru
fi
sleep 5
done
######

chmod +x ~/.config/openbox/autostart

cp /etc/X11/xinit/xinitrc ~/.xinitrc
echo 'exec openbox-session' >> ~/.xinitrc

nano .profile
В файл добавляем в конец
if [ -d "$HOME/.cache/mozilla" ] ; then
    rm -vrf $HOME/.cache/mozilla >> /dev/null
fi

if [ -d "$HOME/.mozilla" ] ; then
    rm -vrf $HOME/.mozilla >> /dev/null
fi
Сохраняем, выходим

echo 'startx' >> ~/.bashrc

Перезагружаем и смотрим, что получилось
su root -
/sbin/reboot
