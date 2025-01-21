# Оптимизация ядра Linux для игр

## 1. Базовая подготовка

Сначала установите необходимые инструменты:
```bash
sudo apt install build-essential libncurses-dev flex bison openssl libssl-dev dkms libelf-dev libudev-dev libpci-dev libiberty-dev autoconf
```

## 2. Конфигурация ядра

Основные параметры для оптимизации:

### Отключаем:
- CONFIG_BZIP2=n
- CONFIG_GZIP=n
- CONFIG_SQUASHFS=n
- CONFIG_NFS_FS=n
- CONFIG_CIFS=n
- CONFIG_WIRELESS=n (если не нужен WiFi)
- CONFIG_HAMRADIO=n
- CONFIG_BLUETOOTH=n (если не используете)
- CONFIG_PRINTER=n (если нет принтера)

### Включаем:
- CONFIG_PREEMPT=y
- CONFIG_HZ_1000=y
- CONFIG_HZ=1000
- CONFIG_FUTEX=y
- CONFIG_NUMA=y (для многопроцессорных систем)
- CONFIG_SCHED_AUTOGROUP=y

### Оптимизация для игр:
- CONFIG_GAMING_SCHEDULER=y
- CONFIG_NO_HZ_FULL=y
- CONFIG_HIGH_RES_TIMERS=y

## 3. Сборка ядра

```bash
# Копируем текущую конфигурацию
cp /boot/config-$(uname -r) .config

# Запускаем меню конфигурации
make menuconfig

# Компилируем
make -j$(nproc)

# Устанавливаем
sudo make modules_install
sudo make install
```

## 4. Дополнительные оптимизации

В /etc/sysctl.conf добавьте:
```
vm.swappiness=10
vm.vfs_cache_pressure=50
kernel.sched_autogroup_enabled=1
```

## 5. Рекомендации по CPU governor

Добавьте в /etc/default/grub:
```
GRUB_CMDLINE_LINUX_DEFAULT="... intel_pstate=active processor.max_cstate=1"
```

Затем:
```bash
sudo update-grub
```
