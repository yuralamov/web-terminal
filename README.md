Терминал с браузером Firefox
Подойдет для установки на убогих компьютерах.
Данное "творение"  появилось после посещения детской библиотеки не в самом последнем городе РФ, а скорее во втором.
Вроде нигде не ошибся, проверял на vbox с 1G и 1 ядре. После всех манипуляций настроен автовход user с запуском firefox в приватном окне. При входе очищаются папки firefox. При закрытии firefox он снова открывается. Если какая-то заморочка, то выключаем компьютер (5 сек на кнопку включения) и снова включаем или по SSH. Вместо винта, можно установливать на флешку, которую подключить по USB, засунуть в системник и настроить BIOS на запуск с флешки.
ОЗУ жрет после установки порядка 500М, площадь на диске - около 2G
Разбил на части - много текстовки

1. debian-base Базовая установка системы на терминал
2. remote-ssh Настройка удаленного управления
3. disable-sleep Отключим ненужные слипы

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
