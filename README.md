# alpine-noctalia
## Зачем это всё надо
В репозиториях alpine отсутствует noctalia shell и композитор umbriel, поэтому я решил занятся исправлением ситуации, в этом репо вы найдете как готовые [.apk пакеты](https://github.com/remboplay7/alpine-hyprland-noctalia/tree/main/x86_64), так и APKBUILD файлы которые вы можете сами сбилдить.
## Для казуалов
Подключаем репозиторий дабы просто установить то что получилось сбилдить у меня.

Первой строкой в файле `/etc/apk/repositories` вставляем `https://raw.githubusercontent.com/remboplay7/alpine-hyprland-noctalia/main`  (либо клонируем гит репо себе и подключаем как локальный репозиторий)

Обновляем репо `doas apk update --allow-untrusted`

`--allow-untrusted` нужно писать ибо мне лень все эти ключи туда сюда, по тем же причинам его нужно писать и при каждой установке пакетов из этого репо тоже.

Проверяем пакеты `apk policy --allow-untrusted <интересующий_пакет>`, вы должны увидеть строчки вроде:
```
doas apk policy noctalia --allow-untrusted
noctalia policy:
  5.6.0-r0:
    lib/apk/db/installed
```
Можно приступать к установке `doas apk add noctalia --allow-untrusted` для noctalia.
И `doas apk add umbriel --allow-untrusted` для noctalia соответственно.

Дальнейший запуск и пользование не отличаются от любых других дистрибутивов с OpenRC.
## Для ноулайферов
Подготавливаем окружение.

`doas apk add alpine-sdk abuild apk-tools`

`doas adduser $USER abuild`

Что бы применить изменения в этой сессии (или просто перезагрузите пк):

`newgrp abuild`

abuild нужна пара RSA ключей, генерируем:

`abuild-keygen -a -i`

Там еще компиляторы нужны, линкеры, ninja, meson, make и прочая шняга, но я думаю разберётесь.
### Непосредственно сборка

Переходим в директорию где лежит файл APKBUILD.

Пишем `abuild checksum`

Потом `abuild -r -c`

Ну или `abuild checksum && abuild -r -c`

После успешной сборки обязательно `doas apk update --allow-untrusted`


Если всё собралось как надо, готовые пакеты вы можете найти примерно по такому пути (часть директории  `abuild/alpine-noctalia` у вас может отличатся если корневая директория проекта была переименована):

`~/.local/share/abuild/alpine-noctalia/x86_64`

Подключаем локальный репозиторий подобно подключению удалённых репо, первой строкой в `/etc/apk/repositories` пишем `~/.local/share/abuild/alpine-noctalia`

Ссылки на github репозитории представленных пакетов.

[Noctalia Shell](https://github.com/noctalia-dev/noctalia)

[Umbriel](https://github.com/noctalia-dev/umbriel)

[xdg-desktop-portal-umbriel](https://github.com/noctalia-dev/xdg-desktop-portal-umbriel)






