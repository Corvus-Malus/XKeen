* [Руководство по установке 3X-UI: Часть 1](https://telegra.ph/Ne-wireguardom-edinym-08-29)
* [Руководство по установке 3X-UI: Часть 2](https://telegra.ph/Nastrojka-3xui-part-2-09-21)

# Оглавление
- [Установка Entware](#установка-системы-пакетов-репозитория-entware-на-usb-накопитель)
- [Установка XKeen](#установка-xkeen)
- [Предварительные настройки](#предварительные-настройки)
- [Настройка Xray](#настройка-xray)
- [XKeen Config Generator](#как-использовать-генератор-конфига)
- [Настройка DNS-over-TLS и DNS-over-HTTPS](#прокси-серверы-dns-over-tls-и-dns-over-https-для-шифрования-dns-запросов)

<details>
<summary>Опциональные настройки</summary>
   
- [Удаление компонентов IPv6 и Netfilter](#удаление-компонентов-ipv6-и-netfilter)
- [Настройка автоматического обновления файлов geosite](#настройка-автоматического-обновления-файлов-geosite_zkeendat-и-geoip_zkeenipdat)
- [Решение проблем с маршрутизацией при использовании нескольких туннелей](#решение-проблем-с-маршрутизацией-при-использовании-нескольких-туннелей)
- [Исправление проблемы с быстрым обрывом соединений по SSH](#исправление-проблемы-с-быстрым-обрывом-соединений-по-ssh)
- [Исправление проблемы с SSH доступом на Keenetic после установки Entware](#исправление-проблемы-с-ssh-доступом-на-keenetic-после-установки-entware)
- [Обновление ядра XRAY до последней версии](#обновление-ядра-xray-до-последней-версии)
- [Бинарный файл xray для KN-2910 и KN-1212](#используйте-этот-бинарный-файл-xray-если-у-вас-kn-2910-или-kn-1212)
- [Настройка BBR через 3X-UI Panel Management Script](#настройка-bbr-через-3x-ui-panel-management-script)
- [Как отключить двухсторонний пинг в Linux](#как-отключить-двухсторонний-пинг-в-linux)
</details>

<details>
<summary>Консольные команды XKeen</summary>
   
   - [Установка](#установка)
   - [Обновление](#обновление)
   - [Включение или изменения правил обновления](#включение-или-изменения-правил-обновления)
   - [Регистрация в системе](#регистрация-в-системе)
   - [Удаление автоматических обновлений](#удаление-автоматических-обновлений)
   - [Удаление утилит и компонентов](#удаление-утилит-и-компонентов)
   - [Удаление регистраций](#удаление-регистраций)
   - [Порты с которыми работает прокси-клиент](#порты-с-которыми-работает-прокси-клиент)
   - [Порты которые будут исключены из работы прокси-клиента](#порты-которые-будут-исключены-из-работы-прокси-клиента)
   - [Обновление регистраций](#обновление-регистраций)
   - [Переустановка](#переустановка)
   - [Создание резервных копий](#создание-резервных-копий)
   - [Восстановление последних резервных копий](#восстановление-последних-резервных-копий)
   - [Проверки](#проверки)
   - [Управление прокси-клиентом](#управление-прокси-клиентом)
   - [Удаляем XKeen](#удаляем-xray--xkeen--конфигурации--резервные-копии)
</details>

---

> [!NOTE]
> Эта инструкция поможет вам настроить XKeen на вашем роутере. Пожалуйста, следуйте шагам внимательно, чтобы избежать ошибок.
>
> Если вы уже пытались настроить XKeen по инструкции с Хабра или других источников, рекомендуем сбросить роутер до заводских настроек, отформатировать флешку в файловой системе EXT4 и начать настройку заново, используя эту инструкцию. Это поможет избежать возможных проблем и упростит процесс настройки.

# Установка системы пакетов репозитория Entware на USB-накопитель

1. Подключите жесткий диск к ПК и подготовьте его разделы. Для работы менеджера пакетов OPKG диск должен быть отформатирован в файловой системе **EXT4**.

Отформатировать можно воспользоваться бесплатной версией программы **Paragon Partition Manager Free** или **AOMEI Partition Assistant Standard Edition**.

Приведем пример форматирования накопителя в **Paragon Partition Manager Free**:

  <a href="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Light/Paragon-Partition-Manager-Free-Light.png" target="_blank" rel="noopener noreferrer">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Light/Paragon-Partition-Manager-Free-Light.png">
      <img src="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Light/Paragon-Partition-Manager-Free-Light.png">
    </picture>
  </a>

  <a href="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Light/Paragon-Partition-Manager-Free-Light.png" target="_blank" rel="noopener noreferrer">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Light/Paragon-Partition-Manager-Free-2-Light.png">
      <img src="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Light/Paragon-Partition-Manager-Free-2-Light.png">
    </picture>
  </a>

После форматирования подключите уже подготовленный накопитель c файловой системой **EXT4** к USB-порту роутера. Диск должен отобразиться на странице "Приложения" в разделе "Диски и принтеры". Если USB-накопитель не определился в роутере, проверьте установлен ли компонент операционной системы "**Файловая система Ext**".

> **Важно!** *Накопитель с файловой системой EXT4 нельзя использовать в ОС Windows. Если нужно подключить накопитель с EXT4 в Windows, можно воспользоваться специальным драйвером ext2fsd, разработанным сообществом открытого программного обеспечения для файловых систем семейства ext.*

<br>
Перед установкой OPKG и XKeen рекомендуется сделать резервную копию прошивки и настроек роутера.<br><br>

  <a href="http://192.168.1.1/system" target="_blank" rel="noopener noreferrer">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Dark/Keenetic-backup-Dark.jpg">
      <img width="100%" height="100%" src="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Light/Keenetic-backup-Light.jpg">
    </picture>
  </a>

Видеоинструкция от 24 авг. 2024 г. (автор Kasper): https://youtu.be/_QkGq8SLcpE

Скачать можно по этой ссылке, если YouTube не загружается:
* https://disk.yandex.ru/i/aK8ScigR9UWnvA
* https://www.icloud.com/iclouddrive/013pnd8NLJa8Ax1c5tcXyPmyQ

<br>

2. В роутере Keenetic установите нужные компоненты [OPKG](https://help.keenetic.com/hc/ru/articles/213968029-Установка-внешних-Opkg-пакетов-для-версий-NDMS-2-11-и-более-ранних). Основным и обязательным является компонент "**Поддержка открытых пакетов**".

- [x] Интерфейс USB
- [x] Файловая система Ext
- [x] Общий доступ к файлам и принтерам по протоколу SMB
- [x] Поддержка открытых пакетов
- [x] Прокси-сервер DNS-over-TLS
- [x] Прокси-сервер DNS-over-HTTPS
- [x] Протокол IPv6
- [x] Модули ядра подсистемы Netfilter

<p align="center">
  <a href="http://192.168.1.1/system/components" target="_blank" rel="noopener noreferrer">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Dark/Keenetic-components-Dark.jpg">
      <img width="100%" height="100%" src="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Light/Keenetic-components-Light.jpg">
    </picture>
  </a>
</p>

<br>

3. Теперь нужно установить репозиторий системы пакетов [Entware](https://forum.keenetic.net/topic/4299-entware/).

> [!NOTE]
> **Справка**: Для моделей 4G (KN-1212), Omni (KN-1410), Extra (KN-1710/1711/1713), Giga (KN-1010/1011), Ultra (KN-1810), Viva (KN-1910/1912), Giant (KN-2610), Hero 4G (KN-2310), Hopper (KN-3810) и Zyxel Keenetic II / III, Extra, Extra II, Giga II / III, Omni, Omni II, Viva, Ultra, Ultra II используйте для установки архив **mipsel** — [mipsel-installer.tar.gz](https://bin.entware.net/mipselsf-k3.4/installer/mipsel-installer.tar.gz)
>
> Для моделей Ultra SE (KN-2510), Giga SE (KN-2410), DSL (KN-2010), Duo (KN-2110), Ultra SE (KN-2510), Hopper DSL (KN-3610) и Zyxel Keenetic DSL, LTE, VOX используйте для установки архив **mips** — [mips-installer.tar.gz](https://bin.entware.net/mipssf-k3.4/installer/mips-installer.tar.gz)
>
> Для моделей Peak (KN-2710), Ultra (KN-1811) используйте архив **aarch64** — [aarch64-installer.tar.gz](https://bin.entware.net/aarch64-k3.10/installer/aarch64-installer.tar.gz)

<br>

4. В нашем примере рассмотрим установку архива **mipsel**.

Подключите уже подготовленный накопитель c файловой системой [EXT4](https://help.keenetic.com/hc/ru/articles/115005875145-Использование-файловой-системы-EXT4-на-USB-накопителях) к USB-порту роутера. Диск должен отобразиться на странице "Приложения" в разделе "Диски и принтеры".

<p align="center">
  <a href="http://192.168.1.1/apps" target="_blank" rel="noopener noreferrer">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Dark/Keenetic-apps-Dark.png">
      <img width="100%" height="100%" src="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Light/Keenetic-apps-Light.jpg">
    </picture>
  </a>
</p>

На компьютере с помощью файлового менеджера подключитесь к диску по сети (в ОС Windows можно использовать Проводник). В настройках роутера предварительно должно быть включено приложение "[Сервер SMB](https://help.keenetic.com/hc/ru/articles/360000812220-Сервер-SMB-доступ-к-файлам-и-принтерам)" для доступа к подключаемым USB-дискам по сети.

В корне раздела диска создайте директорию **install**, куда положите файл **mipsel-installer.tar.gz**.

<p align="center">
  <a href="http://192.168.1.1/apps/device/Media0" target="_blank" rel="noopener noreferrer">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Dark/Explorer-install-Dark.jpg">
      <img width="100%" height="100%" src="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Light/Explorer-install-Light.jpg">
    </picture>
  </a>
</p>

<br>

5. В веб-интерфейсе роутера перейдите на страницу OPKG для выбора накопителя и добавления скрипта initrc.
6. Для Keenetic с версией KeeneticOS 2.12 и выше, перейдите на страницу **OPKG** и выполните следующие настройки:

* В поле "Накопитель" выберите диск OPKG (метка EXT4-раздела)

Нажмите **Сохранить**.

> *Если после сохранения в поле "**Сценарий initrc**" не появится путь `/opt/etc/init.d/rc.unslung`, введите его вручную в этом поле.*

<p align="center">
  <a href="http://192.168.1.1/opkg" target="_blank" rel="noopener noreferrer">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Dark/Keenetc-opkg-Dark.jpg">
      <img width="100%" height="100%" src="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Light/Keenetic-OPKG-Light.jpg">
    </picture>
  </a>
</p>

7. Перейдите на страницу "Диагностика" и откройте Системный журнал роутера. В нем вы должны увидеть следующие записи при установке системы пакетов Entware:

> I [Aug 26 16:21:43] ndm: Opkg::Manager: init script reset to default: /opt/etc/initrc.
> 
> I [Aug 26 16:21:44] installer: [**1/5**] Начало установки системы пакетов "Entware"...
> 
> I [Aug 26 16:21:53] installer: Info: Создание каталогов...
> 
> I [Aug 26 16:21:53] installer: [**2/5**] Загрузка и установка основных пакетов...
> 
> I [Aug 26 16:22:43] installer: Info: Установка пакетов прошла успешно!
> 
> I [Aug 26 16:22:43] installer: [**3/5**] Генерация SSH-ключей...
> 
> I [Aug 26 16:22:51] installer: Info: Ключ "ed25519" создан.
> 
> I [Aug 26 16:22:52] installer: [**4/5**] Настройка сценария запуска,
> 
> I [Aug 26 16:22:52] installer: Можно открыть SSH-сессию для соединения с устройством (логин - root, пароль - keenetic, порт - 222).
> 
> I [Aug 26 16:22:52] installer: **[**5/5**] Установка системы пакетов "Entware" завершена! Не забудьте сменить пароль и номер порта!**

<br>

8. Скачайте терминальную программу [Putty](https://www.putty.org/) для работы с протоколами SSH и Telnet.

9. Запустите Putty, выберите тип подключения **SSH**, впишите **IP-адрес** роутера в домашнем сегменте Home (по умолчанию 192.168.1.1), укажите **22**-й порт и нажмите кнопку Open.

> [!NOTE]
**Важно!** ***222**-й порт используется, если в роутере установлен компонент "**Сервер SSH**". Если он не установлен, используйте **22**-й порт для подключения к **Entware**.*

<p align="center">
  <a href="root@192.168.111.1" target="_blank" rel="noopener noreferrer">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Light/Putty-Light.png">
      <img src="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Light/Putty-Light.png">
    </picture>
  </a>
</p>

<br>

Подтвердите добавление ключа безопасности в кэш программы Putty для продолжения установки соединения.

<p align="center">
  <a href="root@192.168.111.1" target="_blank" rel="noopener noreferrer">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Light/Putty-Accept-Light.png">
      <img src="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Light/Putty-Accept-Light.png">
    </picture>
  </a>
</p>

При загрузке подтвердите вход, нажав **Accept**.

Далее перейдите в настройки роутера при помощи протокола Secure Shell (SSH).

Для авторизации в **Entware** используйте следующие данные:

**login as**: `root`

**root@192.168.111.1's password**: `keenetic`

<p align="left">
  <a href="root@192.168.111.1" target="_blank" rel="noopener noreferrer">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Dark/Putty-1-Dark.png">
      <img width="661px" src="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Dark/Putty-1-Dark.png">
    </picture>
  </a>
</p>

Можно установить свой пароль. Для этого введите команду **passwd**:

**New password**: *впишите свой пароль*

**Retype password**: *подтвердите пароль*

```bash
passwd
```

<p align="left">
  <a href="root@192.168.111.1" target="_blank" rel="noopener noreferrer">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Dark/Putty-2-Dark.png">
      <img width="661px" src="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Dark/Putty-2-Dark.png">
    </picture>
  </a>
</p>

<br>

10. При успешной авторизации вы окажетесь в оболочке BusyBox v1.27.2 () built-in shell (ash). Теперь нужно обновить opkg-пакет, для этого введите команду **opkg update** и **opkg upgrade**:

```bash
opkg update
```

<p align="left">
  <a href="root@192.168.111.1" target="_blank" rel="noopener noreferrer">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Dark/Putty-3-Dark.png">
      <img width="661px" src="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Dark/Putty-3-Dark.png">
    </picture>
  </a>
</p>

Далее можно приступать к установке нужного OpenWRT пакета.

# Установка XKeen
> Выполнять от пользователя root

```bash
opkg install curl
```

```bash
curl -sOfL https://raw.githubusercontent.com/Skrill0/XKeen/main/install.sh
```

```bash
chmod +x ./install.sh
```

```bash
./install.sh
```

<p align="left">
  <a href="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Dark/Putty-4-Dark.png" target="_blank" rel="noopener noreferrer">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Dark/Putty-4-Dark.png">
      <img src="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Dark/Putty-4-Dark.png">
    </picture>
  </a>
</p>

<br>

> Выбираем `1. Установить отсутствующие GeoIP`

<p align="left">
  <a href="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Dark/Putty-6-Dark.png" target="_blank" rel="noopener noreferrer">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Dark/Putty-6-Dark.png">
      <img src="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Dark/Putty-6-Dark.png">
    </picture>
  </a>
</p>

Выберите номер или номера действий для GeoIP  

- [ ] 0. Пропустить
- [x] 1. Установить отсутствующие GeoIP
- [ ] 2. Нет доступных GeoIP для обновления
- [ ] 3. Установить AntiFilter
- [ ] 4. Установить v2fly
- [ ] 99. Нет установленных GeoIP для удаления

**Ваш выбор: 1**

<br>

> Выбираем `1. Установить отсутствующие GeoSite`

<p align="left">
  <a href="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Dark/Putty-7-Dark.png" target="_blank" rel="noopener noreferrer">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Dark/Putty-7-Dark.png">
      <img src="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Dark/Putty-7-Dark.png">
    </picture>
  </a>
</p>

Выберите номер или номера действий для GeoSite  

- [ ] 0. Пропустить
- [x] 1. Установить отсутствующие GeoSite
- [ ] 2. Нет доступных GeoSite для обновления
- [ ] 3. Установить v2fly
- [ ] 4. Установить AntiFilter
- [ ] 5. Установить AntiZapret
- [ ] 6. Установить Zkeen
- [ ] 99. Нет установленных GeoSite для удаления

**Ваш выбор: 1**

<br>

> Включаем автоматическое обновление для всех (`1`)

<p align="left">
  <a href="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Dark/Putty-8-Dark.png" target="_blank" rel="noopener noreferrer">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Dark/Putty-8-Dark.png">
      <img src="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Dark/Putty-8-Dark.png">
    </picture>
  </a>
</p>

Выберите номер или номера действий для автоматических обновлений  
  
- [ ] 0. Пропустить
- [x] 1. Включить отсутствующие задачи автоматического обновления
- [ ] 2. Обновить включенные задачи автоматического обновления
- [ ] 3. Обновить Xkeen
- [ ] 4. Обновить Xray
- [ ] 5. Включить GeoSite
- [ ] 6. Обновить GeoIP
- [ ] 99. Выключить все

**Ваш выбор: 1**

<br>

> Устанавливаем обновление, например ежедневно в 00:00

<p align="left">
  <a href="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Dark/Putty-9-Dark.png" target="_blank" rel="noopener noreferrer">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Dark/Putty-9-Dark.png">
      <img src="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Dark/Putty-9-Dark.png">
    </picture>
  </a>
</p>

Время автоматического обновления для всех задач:  
  
 Выберите день  
- [ ] 0. Отмена
- [ ] 1. Понедельник
- [ ] 2. Вторник
- [ ] 3. Среда
- [ ] 4. Четверг
- [ ] 5. Пятница
- [ ] 6. Суббота
- [ ] 7. Воскресенье
- [ ] 8. Ежедневно  

<br>

```
Cron остановлен
Cron запущен

Выполняется очистка временных файлов после работы Xkeen`
Очистка временных файлов успешно выполнена

Перед использованием Xray настройте конфигураций по пути «/opt/etc/xray/configs»
Установка окончена
```

<p align="left">
  <a href="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Dark/Putty-10-Dark.png" target="_blank" rel="noopener noreferrer">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Dark/Putty-10-Dark.png">
      <img src="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Dark/Putty-10-Dark.png">
    </picture>
  </a>
</p>

<br>

# Предварительные настройки

* Перейти в Web роутера *(стандартный адрес [192.168.1.1](http://192.168.1.1/policies/interface-priorities))*
* Перейти в раздел **`Приоритеты подключений > Политики доступа в интернет`**
* Создать политику **`XKeen`**
* Выбрать способ доступа к интернету **`Отметить провайдера или нескольких`**
  
> *Доступна «Многопутевая передача». Используйте её, если у вас два провайдера.*

* Перейти в раздел **`Приоритеты подключений > Применение политик`**
* Добавить в созданную политику цели **`Клиент | Сеть`**

<p align="center">
  <a href="http://192.168.1.1/policies/interface-priorities" target="_blank" rel="noopener noreferrer">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Dark/Keenetic-interface-priorities-Dark.png">
      <img width="100%" height="100%" src="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Light/Keenetic-interface-priorities-Light.png">
    </picture>
  </a>
</p>

<p align="center">
  <a href="http://192.168.1.1/policies/policy-consumers" target="_blank" rel="noopener noreferrer">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Dark/Keenetic-policy-consumers-Dark.png">
      <img width="100%" height="100%" src="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Light/Keenetic-policy-consumers-Light.png">
    </picture>
  </a>
</p>

**Перенести сервисы Keenetic с 443 порта**

* Перейти в CLI роутера *(стандартный адрес [192.168.1.1/a](http://192.168.1.1/a))*

> **Примечание**: *Сервисы, такие как KeenDNS, будут доступны на новом порте после переноса. Например, если вы перенесли с 443 на 8443, доступ к KeenDNS будет по адресу `xxxx.keenetic.link:8443`.*

* Перенести сервисы на любой из следующих портов

`| 5083 | 5443 | 8083 | 8443 | 65083 |`

* Команда переноса
```bash
ip http ssl port {port}
```
* Пример записи
```bash
ip http ssl port 8443
```

<p align="center">
  <a href="http://192.168.1.1/webcli/parse" target="_blank" rel="noopener noreferrer">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Dark/Keenetic-webcli-Dark.jpg">
      <img width="100%" height="100%" src="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Light/webcli-Light.jpg">
    </picture>
  </a>
</p>

* Сохранить изменения
```bash
system configuration save
```

<p align="center">
  <a href="http://192.168.1.1/webcli/parse" target="_blank" rel="noopener noreferrer">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Dark/Keenetic-webcli-save-Dark.jpg">
      <img width="100%" height="100%" src="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Light/webcli-save-Light.jpg">
    </picture>
  </a>
</p>

<br>

# Настройка Xray
Перейти в директорию `/etc/xray/configs/`

<p align="center">
  <a href="http://192.168.1.1/apps/device/Media0" target="_blank" rel="noopener noreferrer">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Dark/Explorer-configs-Dark.png">
      <img width="100%" height="100%" src="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Light/Explorer-configs-Light.jpg">
    </picture>
  </a>
</p>

Нас интересуют только 3 файла: `03_inbounds.json`, `04_outbounds.json` и `05_routing.json`

* **03_inbounds.json**: https://github.com/Corvus-Malus/XKeen/releases/latest/download/03_inbounds.json

* **04_outbounds.json**: https://github.com/Corvus-Malus/XKeen/releases/latest/download/04_outbounds.json

<br>

Выберите один из вариантов маршрутизации `05_routing.json`

* **05_routing.json**: https://github.com/Corvus-Malus/XKeen/releases/latest/download/05_routing.json

> VPS-подключение используется для указанных IP-адресов и доменных имен (например, Google, Twitter, TikTok и др.).
> 
> Прямое подключение используется для всего остального трафика, кроме заблокированных доменов и уязвимых UDP-портов.

или

* **05_routing.json**: https://github.com/Corvus-Malus/XKeen-docs/releases/latest/download/05_routing.json

> Прямое подключение используется для доменов в зоне .ru, .su, .рф и других, а также для торрентов.
> 
> VPS-подключение применяется ко всем остальным запросам, кроме заблокированных UDP-портов.

<br>

**03_inbounds.json** - `/etc/xray/configs/03_inbounds.json`

<p align="center">
  <a href="http://192.168.1.1/policies/interface-priorities" target="_blank" rel="noopener noreferrer">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Dark/03-inbounds-Dark.png">
      <img src="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Dark/03-inbounds-Dark.png">
    </picture>
  </a>
</p>

<br>

**04_outbounds.json** - `/etc/xray/configs/04_outbounds.json`

<p align="center">
  <a href="http://192.168.1.1/policies/interface-priorities" target="_blank" rel="noopener noreferrer">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Dark/04-outbounds-Dark.pngg">
      <img src="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Dark/04-outbounds-Dark.png">
    </picture>
  </a>
</p>

> [!NOTE]
***04_inbounds.json*** *можно настроите используя [XKeen Config Generator](#как-использовать-генератор-конфига).*

<br>

`tag` - тег соединения, пусть будет "vless-reality"

`protocol` - обязательно "vless"

`address` - "IP вашего арендованного VPS сервера"

`port` - "443"

`fingerprint` - то что указывали в настройках 3X-UI "chrome"

`serverName` - тоже такие же как в 3X-UI "yahoo.com"

`id`, `publicKey`, `shortId` - смотрим в инфо соединения на **3X-UI**

<p align="center">
  <a href="https://disk.yandex.ru/d/hAo0GJYMrZTRjg" target="_blank" rel="noopener noreferrer">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Dark/3X-UI-Dark.png">
      <img src="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Dark/3X-UI-Dark.png">
    </picture>
  </a>
</p>

> Если у вас несколько пользователей, то Short ID будет отображаться в виде списка значений, разделённых запятыми. Значения идут в том же порядке, что и ваши пользователи. Выберите нужное.
<br>

<p align="center">
  <a href="https://corvus-malus.github.io/XKeen-Config-Generator/" target="_blank" rel="noopener noreferrer">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Dark/3X-UI-2-Dark.png">
      <img src="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Dark/3X-UI-2-Dark.png">
    </picture>
  </a>
</p>

<br>

Инфо соединения также можно взять из URL

<p align="center">
  <a href="https://corvus-malus.github.io/XKeen-Config-Generator/" target="_blank" rel="noopener noreferrer">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Dark/3X-UI-3-Dark.png">
      <img src="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Dark/3X-UI-3-Dark.png">
    </picture>
  </a>
</p>

>pbk=publicKey, fp=fingerprint, sni=serverName, sid=shortId

<br>

> *Если у вас возникают трудности при заполнении конфигурационного файла вручную, вы можете воспользоваться генератором конфига.
> Следуйте этим шагам:*

<br>

# Как использовать Генератор Конфига

1. Перейдите в панель 3X-UI.

<p align="center">
  <a href="https://corvus-malus.github.io/XKeen-Config-Generator/" target="_blank" rel="noopener noreferrer">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Dark/3X-UI-4-Dark.png">
      <img src="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Dark/3X-UI-4-Dark.png">
    </picture>
  </a>
</p>

<br>

2. Найдите и скопируйте ссылку подключения, которая необходима для генерации конфигурационного файла.

<p align="center">
  <a href="https://corvus-malus.github.io/XKeen-Config-Generator/" target="_blank" rel="noopener noreferrer">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Dark/3X-UI-5-Dark.png">
      <img src="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Dark/3X-UI-5-Dark.png">
    </picture>
  </a>
</p>

<br>

3. Перейдите по ссылке https://corvus-malus.github.io/XKeen-Config-Generator

<p align="center">
  <a href="https://corvus-malus.github.io/XKeen-Config-Generator/" target="_blank" rel="noopener noreferrer">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Dark/XKeen-Config-Generator-Dark.png">
      <img src="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Dark/XKeen-Config-Generator-Dark.png">
    </picture>
  </a>
</p>

<br>

4. Вставьте скопированную ссылку из 3X-UI в соответствующее поле генератора.
5. Нажмите кнопку для генерации конфигурационного файла.
6. После завершения генерации, файл `04_outbounds` будет доступен для сохранения на вашем компьютере.

<p align="center">
  <a href="https://corvus-malus.github.io/XKeen-Config-Generator/" target="_blank" rel="noopener noreferrer">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Dark/XKeen-Config-Generator-2-Dark.png">
      <img src="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Dark/XKeen-Config-Generator-2-Dark.png">
    </picture>
  </a>
</p>

> **Примечание**: *Генератор конфига автоматизирует процесс создания конфигурационного файла, что может значительно упростить настройку и избежать ошибок.*

<br>

**05_routing.json** - `/etc/xray/configs/05_routing.json`

<p align="center">
  <img src="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Dark/05-routing-Dark.png" alt="Example Image" width="400"/>
</p>

<p align="center">
<b>Примечание</b>: Способы с GeoIP / GeoSite — в некотором смысле автоматические.
Т.е. это целые базы адресов, которые используются для выборочного обхода. К примеру, GeoIP AntiFilter — все адреса из листа AntiFilter.<br>
При этом правила автоматически обновляются через xkeen.
<br><br>
Частичное совпадение<br>
"vk.com" = "vk.com.ru", "music.vk.com.ru", "www.vk.com/im" ≠ vk.ru
<br><br>
Регулярное выражение<br>
Пример записи: "regexp:\\.ya.*\\.ru$" = "www.yandex.ru", "mail.yandex.ru" ≠ "ya.ru"
Обязательно начинается с "regexp:"
<br><br>
Поддомен<br>
Пример записи: "domain:keenetic.com" = "forum.keenetic.com" ≠ "forum.keenetic12345.com"
<br><br>
Точное совпадение<br>
Пример записи: "full:keenetic.com" = "keenetic.com" ≠ "www.keenetic.com", "keenetic123.com"
</p>

<br>

* Запускаем xkeen
```bash
xkeen -start
```

<p align="center">
  <a href="http://192.168.1.1/system/components" target="_blank" rel="noopener noreferrer">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Dark/xkeen-start-Dark.png">
      <img width="100%" height="100%" src="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Dark/xkeen-start-Dark.png">
    </picture>
  </a>
</p>

### [Прокси-серверы DNS-over-TLS и DNS-over-HTTPS для шифрования DNS-запросов](https://telegra.ph/DoT-DoH-08-12)

* Quad9 DOT: `9.9.9.9` `dns.quad9.net`; `149.112.112.112` `dns.quad9.net`
* Quad9 DOH: `https://dns.quad9.net/dns-query`

<p align="center">
  <a href="http://192.168.1.1/internet-filter/dns-configuration" target="_blank" rel="noopener noreferrer">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Dark/Keenetic-dns-configuration-Dark.png">
      <img width="100%" height="100%" src="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Light/Keenetic-dns-configuration-Light.png">
    </picture>
  </a>
</p>

### Отключить DNS интернет-провайдера
- [x] Игнорировать DNSv4 интернет-провайдера
- [x] Игнорировать DNSv6 интернет-провайдера

<p align="center">
  <a href="http://192.168.1.1/wired/GigabitEthernet1" target="_blank" rel="noopener noreferrer">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Dark/Keenetic-GigabitEthernet-Dark.png">
      <img width="100%" height="100%" src="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Light/Keenetic-GigabitEthernet1-Light.png">
    </picture>
  </a>
</p>

> **Примечание**: *Если вам всё-таки важны результаты замеров скорости, то для достоверного результата необходимо соблюдать, как минимум, два условия:
— не ограничивать порты проксирования 80 и 443;
— не использовать роутинг (временно удалить routing.json и перезапустить xkeen).*

<br>

### [FAQ по XKeen от jameszero](https://jameszero.net/faq-xkeen.htm)

> *FAQ по XKeen (в процессе наполнения) предназначен для тех, у кого возникли дополнительные вопросы после внимательного прочтения инструкции к XKeen*

<p align="center">
END
</p>

---

<br>

# Опциональные настройки

### :large_blue_diamond: Удаление компонентов IPv6 и Netfilter

> *Если установленные компоненты **IPv6** и **Netfilter** вам не нужны и были установлены только для **XKeen**, вы можете их удалить, выполнив следующие шаги:*

* Подключиться к Entware по SSH под root
* Выполнить команду: **`xkeen -modules`**
* Перейти в Web роутера (стандартный адрес [192.168.1.1](http://192.168.1.1/system/components))
* Перейти в раздел **`Параметры системы > Изменить набор компонентов`**
* Снять отметки для удаления
1. **Протокол IPv6**
2. **Модули ядра подсистемы Netfilter**

<br><br>

### :white_square_button: Настройка автоматического обновления файлов geosite_zkeen.dat и geoip_zkeenip.dat

> *По умолчанию файл `geosite_zkeen.dat` не включен в автообновление, поэтому настройка расписания для обновления данных выполняется вручную.*

<br>

**Шаг 1: Подключение к Entware по SSH и установка редактора**
* Подключиться к Entware по SSH под пользователем root.
* Установить текстовый редактор nano:
```bash
opkg install nano
```
<br>

**Шаг 2: Настройка nano как редактора по умолчанию**
* Открыть файл `/etc/profile` для редактирования:
```bash
nano /etc/profile
```
* Добавить в конец файла следующие строки:
```bash
export VISUAL="nano"
export EDITOR="nano"
```
* Сохранить изменения в nano: нажмите `Ctrl + O`, затем `Enter`.
* Закрыть nano: нажмите `Ctrl + X`.
* Перезагрузить роутер

<br>

**Шаг 3: Настройка crontab для автообновления файлов**
* Выполнить команду для редактирования расписания cron:
```bash
crontab -e
```
* Добавить следующие строки для автоматического обновления файлов каждый понедельник в полночь:
```bash
0 0 * * 1 /opt/bin/curl -L -o /opt/etc/xray/dat/geosite_zkeen.dat https://github.com/jameszeroX/zkeen-domains/releases/latest/download/zkeen.dat
5 0 * * 1 /opt/bin/curl -L -o /opt/etc/xray/dat/geoip_zkeenip.dat https://github.com/jameszeroX/zkeen-ip/releases/latest/download/zkeenip.dat && xkeen -restart
```
* Сохранить изменения в crontab: нажмите `Ctrl + O`, затем `Enter`.
* Закрыть редактор: нажмите `Ctrl + X`.

<br>

**Шаг 4: Проверка расписания**
* Чтобы убедиться, что расписание crontab сохранено правильно, выполнить:
```bash
crontab -l
```

<br><br>

### :heavy_minus_sign: Решение проблем с маршрутизацией при использовании нескольких туннелей

Если у вас возникают проблемы с интернет-соединением при одновременном использовании нескольких прокси-серверов или туннелей, например, когда клиент на телефоне отключается при подключении через роутер, добавьте IP-адрес сервера с маской /32 в исключения маршрутизации. Это поможет избежать конфликтов между прокси-серверами и вашим интернет-соединением.

Если у вас несколько туннелей, добавьте IP-адреса всех серверов в эту строку.

Для этого отредактируйте файл `/opt/etc/init.d/S24xray` и найдите строку, которая начинается с `ipv4_exclude=`. Внутри кавычек добавьте IP-адрес вашего VPS с маской /32. Например:

<mark>ipv4_exclude="255.255.255.255/32 0.0.0.0/8 10.0.0.0/8 100.64.0.0/10 127.0.0.0/8 169.254.0.0/16 172.16.0.0/12 192.0.0.0/24 192.0.2.0/24 192.168.0.0/16 198.18.0.0/15 198.51.100.0/24 203.0.113.0/24 224.0.0.0/4 240.0.0.0/4 <b>199.199.199.199/32</b>"</mark>

> *Здесь **`199.199.199.199/32`** — это пример IP-адреса вашего VPS, который добавлен в исключения.*

После внесения изменений сохраните файл и перезагрузите сервис Xray, чтобы настройки вступили в силу. Для этого выполните команду:

```
xkeen -restart
```

Теперь ваш сервер должен корректно работать с несколькими туннелями без конфликтов в маршрутизации.

<br><br>

### :diamond_shape_with_a_dot_inside: Исправление проблемы с быстрым обрывом соединений по SSH

Отредактируйте файл `/opt/etc/config/06_policy.json`, увеличив значение параметра `connIdle`. Стандартное значение, указанное в документации XRay, составляет **300**. Увеличение этого значения может повысить нагрузку на роутер. 

> *В качестве альтернативного решения добавьте IP-адрес сервера в исключения маршрутизации (см. раздел "Решение проблем с маршрутизацией при использовании нескольких туннелей").*

<br><br>

### :beginner: Исправление проблемы с SSH доступом на Keenetic после установки Entware

Если после установки Entware на Keenetic не удается подключиться по SSH на порт 222 с логином `root` и паролем `keenetic`, возможно, пароль `root` не установлен или установлен некорректно.

Подключитесь к CLI через SSH на порт 22, используя логин и пароль от админки роутера. Не перепутайте с Entware.

**Выполните следующие команды:**

```sh
exec sh
```

```sh
exec /opt/etc/init.d/S51dropbear restart
```

<br><br>

### :radio_button: Обновление ядра XRAY до последней версии

**Подключитесь к Entware по SSH под пользователем root и выполните следующие команды:**

1. Выполните команду, чтобы скачать скрипт установки:

```sh
curl -L -O https://github.com/Corvus-Malus/XKeen-docs/raw/main/Installer/install_xray.sh
```

2. Сделайте скрипт исполняемым:

```sh
chmod +x install_xray.sh
```

3. Выполните скрипт с параметром **install** для обновления:

```sh
./install_xray.sh install
```

<br>

**Восстановление оригинального файла xray (*откат обновления*)**

1. Выполните скрипт с параметром **recover**:

```sh
./install_xray.sh recover
```

**Команды**

`./install_xray.sh {install|recover|help} [version]`

> `[version]` - Опциональный параметр. Номер версии Xray для установки (например, 1.8.4). Если версия не указана, будет загружена последняя доступная версия.

<br><br>

### [Возможные решения проблем с доступом к ChatGPT и другим сайтам](https://telegra.ph/QUIC-Enabled---Disabled-08-26)

<p align="center">
  <a href="http://192.168.1.1/firewall/Bridge0" target="_blank" rel="noopener noreferrer">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Dark/Keenetic-Bridge0-Dark.jpg">
      <img width="100%" height="100%" src="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Light/Keenetic-Bridge0-Light.png">
    </picture>
  </a>
</p>

<br><br>

### Используйте этот бинарный файл [xray](https://github.com/Corvus-Malus/XKeen-docs/releases/download/24.09.15/xray), если у вас KN-2910 или KN-1212:

* Keenetic Skipper 4G (KN-2910)
* Keenetic 4G (KN-1212)

1. Заменить файл **[xray](https://github.com/Corvus-Malus/XKeen-docs/releases/download/24.09.15/xray)** в папке **sbin**.
2. Установите для него права **755**:

```
chmod 755 /opt/sbin/xray
```

3. Запустите xkeen командой:

```
xkeen -start
```
<br><br>

### Настройка BBR через 3X-UI Panel Management Script

* Подключитесь к вашему VPS серверу через терминал.
* Введите команду `x-ui` и нажмите **Enter**.

<p align="">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Dark/Enable-BBR-Dark.png">
      <img src="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Light/Enable-BBR-Light.png">
    </picture>
</p>

* В меню выберите пункт **Enable BBR**, введя соответствующую цифру.
* Подтвердите выбор для активации **BBR**.

<p align="center">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Dark/Enable-BBR-2-Dark.png">
      <img src="https://github.com/Corvus-Malus/XKeen-docs/raw/main/images/Light/Enable-BBR-2-Light.png">
    </picture>
</p>

* Убедитесь, что интерфейс отображает сообщение об успешной активации BBR:

`BBR has been enabled successfully.`

<br><br>

### Как отключить двухсторонний пинг в Linux

**Отключение двухстороннего пинга:**

1. Чтобы отключить двухсторонний пинг, выполните следующую команду:

```bash
echo "net.ipv4.icmp_echo_ignore_all=1" | sudo tee -a /etc/sysctl.conf && echo "net.ipv4.icmp_echo_ignore_broadcasts=1" | sudo tee -a /etc/sysctl.conf
```

2. Примените изменения командой:

```bash
sudo sysctl -p
```

**Включение двухстороннего пинга:**

1. Чтобы вернуть пинг обратно, выполните следующую команду:

```bash
echo "net.ipv4.icmp_echo_ignore_all=0" | sudo tee -a /etc/sysctl.conf && echo "net.ipv4.icmp_echo_ignore_broadcasts=0" | sudo tee -a /etc/sysctl.conf
```

2. Снова примените изменения:

```bash
sudo sysctl -p
```

<br><br>

### [AdGuard Home Keenetic для прошивки 4.2 beta 3](https://github.com/Corvus-Malus/AdGuardHome-Keenetic)

> ***AdGuard Home** – это DNS-сервер, блокирующий рекламу и трекинг. Его цель – дать вам возможность контролировать всю вашу сеть и все подключённые устройства. Он не требует установки клиентских программ.*

<br>

# Консольные команды XKeen

### Установка

`xkeen -i`: Необходимые пакеты, Xray и сервисы XKeen

### Обновление

`xkeen -ux`: Xray

`xkeen -uk`: XKeen

`xkeen -ugs`: GeoSite

`xkeen -ugi`: GeoIP

### Включение или изменения правил обновления

`xkeen -uac`: Xray, XKeen, GeoSite, GeoIP

`xkeen -uxc`: Xray

`xkeen -ukс`: XKeen

`xkeen -ugsc`: GeoSite

`xkeen -ugic`: GeoIP

### Регистрация в системе

`xkeen -rx`: Xray

`xkeen -rk`: XKeen

`xkeen -ri`: Автоматический запуск Xray средствами init

### Удаление автоматических обновлений

`xkeen -dac`: Xray, XKeen, GeoSite, GeoIP

`xkeen -dxc`: Xray

`xkeen -dkc`: XKeen

`xkeen -dgsc`: GeoSite

`xkeen -dgic`: GeoIP

### Удаление утилит и компонентов

`xkeen -dx`: Xray

`xkeen -dk`: XKeen

`xkeen -dgs`: GeoSite

`xkeen -dgi`: GeoIP

`xkeen -dc`: Конфигурационные файлы Xray

`xkeen -dt`: Временные файлы

### Удаление регистраций

`xkeen -dr`: Xray

`xkeen -drk`: XKeen

### Порты с которыми работает прокси-клиент

`xkeen -ap 443,80`: Добавить порты для работы (можно указать один или несколько портов через запятую)

`xkeen -dp 443`: Удалить 443 порт из рабочих портов (можно удалить один или несколько портов через запятую; если не указать конкретный порт, будут удалены все)

`xkeen -cp`: Показать с какими портами сейчас работает прокси-клиент

### Порты которые будут исключены из работы прокси-клиента

`xkeen -ape 443,80`: Добавить порты для исключения (можно указать один или несколько портов через запятую)

`xkeen -dpe 443`: Удалить 443 порт из исключенных портов (можно удалить один или несколько портов через запятую; если не указать конкретный порт, будут удалены все)

`xkeen -cpe`: Показать с какими портами сейчас не работает прокси-клиент

### Обновление регистраций

`xkeen -rrx`: Xray

`xkeen -rrk`: XKeen

### Переустановка

`xkeen -x`: Xray

`xkeen -k`: XKeen

`xkeen -rc`: Конфигурационные файлы прокси-клиента

### Создание резервных копий

`xkeen -xb`: Xray

`xkeen -kb`: XKeen

`xkeen -cb`: Конфигурационные файлы прокси-клиента

### Восстановление последних резервных копий

`xkeen -xbr`: Xray

`xkeen -kbr`: XKeen

`xkeen -cbr`: Конфигурационные файлы прокси-клиента

### Проверки

`xkeen -tpx`: Порты, шлюз и протокол прокси-клиента

`xkeen -v`: Версия XKeen

### Управление прокси-клиентом

`xkeen -start`: Запуск

`xkeen -stop`: Остановка

`xkeen -restart`: Перезапуск

`xkeen -status`: Проверка работы

`xkeen -auto`: Смена режима автозапуска

`xkeen -d 4`: Изменить стандартное минимальное время автозапуска (вместо «4» можно указать любое значение в секундах)

`xkeen -diag`: Создание файла диагностики

`xkeen -fixed`: Исправление регистраций от ошибок Entware (пользовательские настройки автозапуска будут утеряны)

### Удаляем Xray | XKeen | Конфигурации | Резервные копии

`opkg remove xkeen`: Удаляем XKeen

`opkg remove xray`: Удаляем Xray и его конфигурации

`rm -rf /opt/backups`: Удаляем резервные копии Xray | XKeen | Конфигураций

<br>

- [Настройка TCP BBR](https://telegra.ph/Nastrojka-TCP-BBR-08-15)
- [AdGuard Home Keenetic 4.2 beta 3](https://github.com/Corvus-Malus/AdGuardHome-Keenetic)
- [Обновление Xray — Настройка Балансировки и Ротации Трафика](https://telegra.ph/Balansirovka-i-Rotaciya-Trafika-08-20)
- [Полезные сервисы и скрипты](https://telegra.ph/Poleznye-servisy-i-skripty-08-16)

---

* [Инструкция](https://xskrill.notion.site/XKeen-c9f0f2a5018743b59eb81bd6fccdf25a) | От [автора](https://t.me/Skrill_zerro) XKeen | Для продвинутой настройки
* [Инструкция ядра](https://xtls.github.io/ru/config/features/multiple.html#пример-конфигурации) | В переводе от Nikita Korotaev
* [Project VLESS](https://t.me/projectVless) | Русскоязычный чат
* https://forum.keenetic.com/topic/16899-xkeen/
* [Телеграм чат XKeen](https://t.me/+SZWOjSlvYpdlNmMy)

Автор XKeen [@Skrill_zerro](https://t.me/Skrill_zerro)
