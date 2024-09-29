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
