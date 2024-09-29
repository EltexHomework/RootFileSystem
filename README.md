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

- `git clone git@github.com:mirror/busybox.git`

- `ARCH=arm make defconfig`

- `ARCH=arm make menuconfig`

- `make -j6` 

- `make install` 

- `find . | cpio -o -H newc | gzip > initramfs.cpio.gz`
