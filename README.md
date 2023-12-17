Терминал с браузером Firefox
Подойдет для установки на убогих компьютерах.
Данное "творение"  появилось после посещения детской библиотеки не в самом последнем городе РФ, а скорее во втором.
Вроде нигде не ошибся, проверял на vbox с 1G и 1 ядре. После всех манипуляций настроен автовход user с запуском firefox в приватном окне. При входе очищаются папки firefox. При закрытии firefox он снова открывается. Если какая-то заморочка, то выключаем компьютер (5 сек на кнопку включения) и снова включаем или по SSH. Вместо винта, можно установливать на флешку, которую подключить по USB, засунуть в системник и настроить BIOS на запуск с флешки.
ОЗУ жрет после установки порядка 500М, площадь на диске - около 2G
Разбил на части - много текстовки

1. debian-base Базовая установка системы на терминал
2. remote-ssh Настройка удаленного управления
3. 
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
