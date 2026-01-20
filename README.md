# 📖 Простое руководство по сборке AmneziaWG для OpenWrt

## 📦 1. Скачайте исходники OpenWrt

```bash
git clone https://github.com/openwrt/openwrt.git -b v24.10.0
```
```bash
cd openwrt
```
```bash
./scripts/feeds update -a
```
```bash
./scripts/feeds install -a
```
## 🔧 2. Добавьте репозиторий AmneziaWG
```bash
echo "src-git amnezia https://github.com/Slava-Shchipunov/awg-openwrt.git" >> feeds.conf.default
```
Проверьте что добавилось:
```bash
cat feeds.conf.default
```
Должно быть:

text
src-git packages https://git.openwrt.org/feed/packages.git

src-git luci https://git.openwrt.org/project/luci.git

src-git routing https://git.openwrt.org/feed/routing.git

src-git telephony https://git.openwrt.org/feed/telephony.git

src-git amnezia https://github.com/Slava-Shchipunov/awg-openwrt.git

## 📥 3. Установите пакеты AmneziaWG
```bash
./scripts/feeds update -a

./scripts/feeds install -a -p amnezia
```
Проверьте установленные пакеты:
```bash
./scripts/feeds list -r amnezia
```
Должно показать:

text
amneziawg-tools                 - AmneziaWG userspace control program (awg)

kmod-amneziawg                  - AmneziaWG VPN Kernel Module

luci-i18n-amneziawg-ru         - luci-proto-amneziawg - ru translation

luci-proto-amneziawg           - Support for AmneziaWG VPN

## ⚙️ 4. Настройте сборку для Mi Router 4C
```bash
make menuconfig
```

🎮 Управление в menuconfig:

- Esc - назад

- Space - изменить выбор

- / - поиск пакета

- Enter - войти в подменю

- F12 - сохранить скриншот

## 🎯 Выберите устройство:


**Target System:** MediaTek Ralink MIPS   ←  (архитектура процессора)

**Subtarget:** MT76x8 based boards        ←  (семейство чипсетов)

**Target Profile:** Xiaomi Mi Router 4C   ←  (конкретная модель роутера)

⚡ Что означают символы:

[ ] - Не собирать пакет

[M] - Собрать отдельный .ipk модуль

[*] - Встроить в прошивку

## 🔑 5. Включите необходимые пакеты
В меню **Kernel modules → Network Support:**

[*] kmod-amneziawg

[*] kmod-wireguard

В меню **Network → VPN:**


[*] amneziawg-tools

[*] wireguard-tools

В меню **LuCI → Protocols:**


[*] luci-proto-amneziawg

[*] luci-proto-wireguard

В меню **Kernel modules → Cryptographic API modules:**


[*] kmod-crypto-chacha20poly1305

## 🚀 6. Запустите сборку
```bash
make -j$(nproc) V=sc
```
Первая сборка займет от 30м до 4ч

## ✅ 7. Проверьте результаты
Найдите собранную прошивку:
```bash
find bin/targets -name "*.bin" -type f
```
Найдите пакеты AmneziaWG:
```bash
find bin -type f \( -name "*amnezia*" -o -name "*awg*" \) 2>/dev/null
```
💾 Готовые файлы будут здесь:
```
openwrt/
├── bin/
│   ├── targets/ramips/mt76x8/
│   │   └── openwrt-*.bin          ← ВАША ПРОШИВКА
│   └── packages/mipsel_24kc/       
│       ├── base/
│       │   ├── kmod-amneziawg_*.ipk  ← ПАКЕТЫ
│       │   ├── kmod-wireguard_*.ipk
│       │   └── amneziawg-tools_*.ipk
│       └── luci/
│           └── luci-proto-amneziawg_*.ipk
├── .config                        ← СОХРАНИТЕ ЭТОТ ФАЙЛ!
└── build.log                      ← Лог сборки
```
## ⚠️ Важное: kmod-пакеты

**kmod-пакеты из официальных репозиториев OpenWrt не установятся!**

После своей сборки ядро имеет уникальный хэш → будут ошибки совместимости.

**Сохраните ВСЕ `.ipk` файлы** из `bin/packages/` — это ваши единственные совместимые пакеты.

**Не удаляйте исходники** — они нужны для сборки дополнительных kmod.

## 🎯 Кратко:
Скачали OpenWrt

Добавили AmneziaWG

Настроили под Mi Router 4C

Включили AmneziaWG + WireGuard

Собрали прошивку

Установили на роутер

Готово! У вас есть OpenWrt с AmneziaWG! 🎉
