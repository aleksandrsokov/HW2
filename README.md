# HW2
 apt update
#Установка пакетов для компиляции
apt install -y wget curl build-essential libncurses-dev bison flex libssl-dev libelf-dev fakeroot
#так же пакет для компиляции
apt install -y dwarves
#Скачивание стабильной версии ядра с официального сайта
wget https://cdn.kernel.org/pub/linux/kernel/v6.x/linux-6.6.30.tar.xz
#Извлечение файлов из архива
tar -xf linux-6.6.30.tar.xz
cd linux-6.6.30
#Копирование конфигурационного файла текущего ядра
cp -v /boot/config-$(uname -r) .config
#Просматривает загруженные модули ядра и меняет конфиг так что бы попали только эти модули 
make localmodconfig
#Отключение сертификатов в конфгурации 
scripts/config --disable SYSTEM_TRUSTED_KEYS
scripts/config --disable SYSTEM_REVOCATION_KEYS
scripts/config --set-str CONFIG_SYSTEM_TRUSTED_KEYS ""
scripts/config --set-str CONFIG_SYSTEM_REVOCATION_KEYS ""
#Компиляция ядра с использованием 6 потоков 
fakeroot make -j6
#Установка модулей 
make modules_install
#Установка ядра
make install
reboot