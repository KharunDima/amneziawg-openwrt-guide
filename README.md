# 🛡️ Руководство по сборке AmneziaWG для OpenWrt

![OpenWrt](https://img.shields.io/badge/OpenWrt-24.10.0-green)
![AmneziaWG](https://img.shields.io/badge/AmneziaWG-1.0-blue)
![License](https://img.shields.io/badge/License-MIT-yellow)
![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen)

> Полное руководство по сборке и настройке AmneziaWG (форк WireGuard) для OpenWrt

## 📋 Содержание
- [🚀 Быстрый старт](#быстрый-старт)
- [📦 Предварительные требования](#предварительные-требования)
- [🔧 Пошаговая инструкция](#пошаговая-инструкция)
- [⚙️ Скрипты автоматизации](#скрипты-автоматизации)
- [🛠️ Решение проблем](#решение-проблем)
- [📚 Дополнительные материалы](#дополнительные-материалы)

## 🚀 Быстрый старт

### Вариант 1: Автоматическая сборка (рекомендуется)

```bash
## Клонируем репозиторий
git clone https://github.com/ваш-ник/amneziawg-openwrt-guide.git
cd amneziawg-openwrt-guide

# Устанавливаем зависимости
sudo bash scripts/setup_dependencies.sh

# Запускаем сборку для Xiaomi Mi Router 4C
bash scripts/build_amnezia.sh --device mi4c
Вариант 2: Ручная сборка
bash
git clone https://github.com/openwrt/openwrt.git -b v24.10.0
cd openwrt
echo "src-git amnezia https://github.com/Slava-Shchipunov/awg-openwrt.git" >> feeds.conf.default
./scripts/feeds update -a
./scripts/feeds install -a
cp ../amneziawg-openwrt-guide/configs/mi_router_4c.config .config
make -j$(nproc) V=sc
📦 Предварительные требования
Аппаратные:
Процессор: x86_64, 4+ ядер

Память: 8+ ГБ RAM

Диск: 30+ ГБ свободного места

Интернет: Стабильное соединение

Программные (Ubuntu/Debian):
bash
# Полный список в requirements.txt
sudo apt update
sudo apt install -y build-essential git python3 libncurses5-dev
🔧 Пошаговая инструкция
Шаг 1: Подготовка системы
bash
# Установите все зависимости
sudo bash scripts/setup_dependencies.sh
Шаг 2: Клонирование OpenWrt
bash
git clone https://github.com/openwrt/openwrt.git -b v24.10.0
cd openwrt
Шаг 3: Добавление AmneziaWG
bash
echo "src-git amnezia https://github.com/Slava-Shchipunov/awg-openwrt.git" >> feeds.conf.default
Шаг 4: Обновление feeds
bash
./scripts/feeds update -a
./scripts/feeds install -a
./scripts/feeds update amnezia
./scripts/feeds install -a -p amnezia
Шаг 5: Настройка конфигурации
Важно: AmneziaWG требует оригинальный WireGuard!

Выберите конфигурацию для вашего устройства:

configs/mi_router_4c.config - Xiaomi Mi Router 4C

configs/mi_router_3g.config - Xiaomi Mi Router 3G

configs/archer_c7.config - TP-Link Archer C7

bash
# Пример для Mi Router 4C
cp ../amneziawg-openwrt-guide/configs/mi_router_4c.config .config
Шаг 6: Сборка
bash
# Загрузка исходников
make download -j$(nproc)

# Сборка прошивки
make -j$(nproc) V=sc 2>&1 | tee build.log
Шаг 7: Проверка результатов
bash
# Используйте наш скрипт проверки
bash ../amneziawg-openwrt-guide/scripts/check_build.sh

# Или проверьте вручную
find bin -type f \( -name "*amnezia*" -o -name "*awg*" \) 2>/dev/null
⚙️ Скрипты автоматизации
Основные скрипты:
scripts/build_amnezia.sh - Полная автоматическая сборка

scripts/build_simple.sh - Упрощенная сборка

scripts/check_build.sh - Проверка результатов сборки

scripts/setup_dependencies.sh - Установка зависимостей

Пример использования:
bash
# Сборка для разных устройств
bash scripts/build_amnezia.sh --device mi4c
bash scripts/build_amnezia.sh --device mi3g
bash scripts/build_amnezia.sh --device archer

# Только загрузка исходников
bash scripts/build_amnezia.sh --download-only

# Очистка
bash scripts/build_amnezia.sh --clean
🛠️ Решение проблем
Частые ошибки:
Ошибка: amneziawg depends on wireguard
Решение: Убедитесь, что оба пакета включены в конфигурации

Ошибка: Нет места на диске

bash
# Добавьте swap
sudo fallocate -l 8G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
Ошибка: Проблемы с menuconfig

bash
# Используйте автоматическую конфигурацию
cp configs/mi_router_4c.config .config
Полный список проблем и решений в docs/troubleshooting.md

📚 Дополнительные материалы
Примеры конфигураций - Примеры awg.conf файлов

FAQ - Ответы на частые вопросы

Расширенные настройки - Для опытных пользователей

🤝 Вклад в проект
Мы приветствуем ваш вклад! Пожалуйста, прочитайте CONTRIBUTING.md перед созданием pull request.

📄 Лицензия
Этот проект распространяется под лицензией MIT. Подробнее в файле LICENSE.

🙏 Благодарности
Разработчикам OpenWrt

Slava Shchipunov за AmneziaWG

Сообществу за тестирование и отзывы

Если это руководство было полезным, поставьте ⭐ на GitHub!
