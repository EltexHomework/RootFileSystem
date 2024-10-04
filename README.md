# Задание 18 - Корневая файловая система 
## Задания
1) Для собранного ядра из Задания №19 Кросс-компиляция добавить корневую файловую систему с процессом init.
2) Для собранного ядра из Задания №19 Кросс-компиляция добавить корневую файловую систему собранную при помощи BusyBox.
## Используемые цели и утилиты 
- `arm-linux-gnueabihf-gcc -static init.c -o init` - сборка бинарника для процесса init
- `file ./init` - проверка собранного бинарника
- `echo init | cpio -o -H newc | gzip > initramfs.cpio.gz` - упаковка бинарника в архив
- `git clone git@github.com:mirror/busybox.git` - клонирование репозитория Busybox
- `ARCH=arm make defconfig` - создание стандартной конфигурации BusyBox
- `ARCH=arm make menuconfig` - открытие меню конфигурации BusyBox
- `make -j6` - сборка BusyBox
- `make install` - установка BusyBox в _install
- `find . | cpio -o -H newc | gzip > initramfs.cpio.gz` - упаковка файловой системы BusyBox в архив
## Результаты вывода команд 
- `arm-linux-gnueabihf-gcc -static init.c -o init`
- `file ./init`
- `echo init | cpio -o -H newc | gzip > initramfs.cpio.gz`
![Screenshot from 2024-09-29 21-02-18](https://github.com/user-attachments/assets/65c3469a-02cd-4662-9ab9-0becc0f3ca42)
- `git clone git@github.com:mirror/busybox.git`
- `ARCH=arm make defconfig`
![Screenshot from 2024-09-29 22-07-30](https://github.com/user-attachments/assets/70137a5a-f449-4b13-9e12-da2f190b0293)
- `ARCH=arm make menuconfig`
- `make -j6` 
![Screenshot from 2024-09-29 22-14-44](https://github.com/user-attachments/assets/37993e1f-04d8-4dac-8584-bcba9f979bca)
- `make install` 
![Screenshot from 2024-09-29 22-17-42](https://github.com/user-attachments/assets/63935121-becd-4c94-af14-bebaf8cf55da)
- `find . | cpio -o -H newc | gzip > initramfs.cpio.gz`
![Screenshot from 2024-09-29 22-19-35](https://github.com/user-attachments/assets/d7ff96f8-3704-430e-9657-7f7dde75ba11)
## Запуски ядра
- Запуск с простым Init процессом
![1](https://github.com/user-attachments/assets/e4303d3c-ed48-43a4-a3c3-6ef1bb01266b)
- Настройки конфигурации
![settings](https://github.com/user-attachments/assets/3ac4059f-0136-4af1-90b8-7eedbae777b7)
- Запуск без командного интерпретатора
![no_comm_interp](https://github.com/user-attachments/assets/50c4ebaf-da7f-45ad-b093-21b8e61cd033)
- Запуск с командным интерпретатором
![f](https://github.com/user-attachments/assets/ec891366-ed77-457c-92bc-de3aad217e3a)
# Кросс-компиляция OpenSSH и добавление в корневую файловую систему BusyBox
## Кросс-компиляция zlib
В процессе кросс-компиляции zlib были использованы следующие команды:
- `CC=arm-linux-gnueabihf-gcc ./configure --static --prefix=$PWD/_install` - создание конфигурации zlib
![ZLIB CONFIGURE](https://github.com/user-attachments/assets/997d1695-92be-4b81-9827-02c7cb910aee)
- `make -j6` - сборка zlib
- `make install` - установка zlib в `./_install`
![ZLIB INSTALL](https://github.com/user-attachments/assets/42e83b24-95fd-4768-84f0-6090ce85e4a4)
## Кросс-компиляция OpenSSL
В процессе кросс-компиляции OpenSSL были использованы следующие команды:
- `./Configure linux-generic32 no-asm shared --prefix=$PWD/_install --cross-compile-prefix=arm-linux-gnueabihf` - создание конфигурации OpenSSL без ассемблерного кода с динамическими библиотеками
![OPENSSL CONFIGURE](https://github.com/user-attachments/assets/dfd1d377-2d37-4140-899a-2ad9d8928a0b)
- `make INSTALL_PREFIX=$PWD/_install install` - установка OpenSSL в `./_install`
![OPENSSL INSTALL](https://github.com/user-attachments/assets/90429f5d-80a8-4a14-abf6-17a911f36b29)
## Кросс-компиляция OpenSSH
В процессе кросс-компиляции OpenSSH были использованы следующие команды:
- `./configure --prefix=$PWD/_install --with-zlib=$PWD/../zlib/_install --with-ssl-dir=$PWD/../openssl/_install --disable-strip --host=arm-linux-gnueabihf` - создание конфигурации OpenSSH
![OPENSSH CONFIGURE](https://github.com/user-attachments/assets/19a1b2d4-9b77-4158-bdd4-c52a49edc1b9)
- `make` - сборка OpenSSH
![OPENSSH MAKE](https://github.com/user-attachments/assets/19b4789c-7019-4745-9917-70f6a64dd84d)
- `make install` - установка OpenSSH
![OPENSSH INSTALL](https://github.com/user-attachments/assets/3c086430-5fa7-4c5d-9309-fe0896465a00)
- `readelf -d ./ssh` - проверка необходимых динамических библиотек
![READELF SSH](https://github.com/user-attachments/assets/e2774678-8412-4145-b9da-ad4ab0d82da0)
## Результаты кросс-компиляции
В результате кросс-компиляции библиотек были получены их файлы в директориях `_install`, все файлы из данных директорий были скопированы в корневую файловую систему BusyBox, собранную ранее.
## Запуск 
- `QEMU_AUDIO_DRV=none qemu-system-arm -M vexpress-a9 -kernel zImage -initrd initramfs.cpio.gz -dtb vexpress-v2p-ca9.dtb -append "console=ttyAMA0 rdinit=/bin/ash" -nographic` - запуск эмулятора qemu
![QEMU_STARTUP](https://github.com/user-attachments/assets/40b1643c-9f6c-4cc7-a2c8-35c0df6bb02d)
- Результат запуска ssh
![Result](https://github.com/user-attachments/assets/4fad2595-2a30-438b-884b-b93049f75b5e)
## Настройка Root пользователя
Изначально утилиты `ssh` и `ssh-keygen` не запускались, выдавая следующую ошибку `Couldn't open /dev/null: No such file or directory`. Для её решения был создан `/dev/null` следующей командой `sudo mknod -m 666 dev/null c 1 3`.
После была попытка запуска утилит, но появилась новая ошибка `No user exists for uid 0`, которая сигнализирует об отсутстви Root пользователя, для исправления данной ошибки были использованы следующие команды:
- `echo "root:x:0:0:root:/root:/bin/ash" > etc/passwd` - создание Root пользователя с `/root` домашней директорией и `/bin/ash` стандартной оболочкой
- `echo "root:x:0:" > etc/group` - создание Root группы
После использования данных команд утилиты смогли успешно запуститься.
