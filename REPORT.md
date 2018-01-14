## Laboratory work IX

Данная лабораторная работа посвещена изучению процесса создания пакета на примере **Github Release**

```ShellSession
$ open https://help.github.com/articles/creating-releases/
```

## Tasks

- [x] 1. Создать публичный репозиторий с названием **lab09** на сервисе **GitHub**
- [x] 2. Ознакомиться со ссылками учебного материала
- [x] 3. Получить токен для доступа к репозиториям сервиса **GitHub**
- [x] 4. Сгенерировать GPG ключ и добавить его к аккаунту сервиса **GitHub**
- [x] 5. Выполнить инструкцию учебного материала
- [x] 6. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial

Задаем переменные окружения : имя пользователя гитхаб, токен гитхаба
Устанавливаем gsed=sed с помощью команды alias
```ShellSession
$ export GITHUB_TOKEN=<полученный_токен>
$ export GITHUB_USERNAME=<имя_пользователя>
$ alias gsed=sed # for *-nix system
```

Скачиваем github-release
```ShellSession
$ cd ${GITHUB_USERNAME}/workspace
$ pushd .     - сохраняем текущую директорию в стек
$ source scripts/activate
$ go get github.com/aktau/github-release
```

Создаем папку lab09 и клонируем в нее репозиторий lab08, затем связываем локальный репозиторий с удаленным 
```ShellSession
$ git clone https://github.com/${GITHUB_USERNAME}/lab08 projects/lab09
$ cd projects/lab09
$ git remote remove origin
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab09
```

Заменяем lab08 на lab09  в README.md
```ShellSession
$ gsed -i 's/lab08/lab09/g' README.md
```

Работа с архивацией
```ShellSession
$ cmake -H. -B_build -DCPACK_GENERATOR="TGZ"  - задаем архивирование в TGZ
$ cmake --build _build --target package   - архивируем
``` 

Подклюаем travis
```ShellSession
$ travis login --auto
$ travis enable
```

```ShellSession
$ git tag -s v0.1.0.0  - подписываем метку
$ git tag -v v0.1.0.0  - подвержаем метку
$ git push origin master --tags  - отправляем изменения на удаленный репозиторий
```

Работаем с информацией о релизе
```ShellSession
$ alias github-release=~/danya-fr0st/workspace/projects/lab09/github-release
$ github-release --version  - узнаем версию github-release
$ github-release info -u ${GITHUB_USERNAME} -r lab09  - узнаем информацию о github-release в лр9
$ github-release release \       - вносим информацию о релизе
    --user ${GITHUB_USERNAME} \
    --repo lab09 \
    --tag v0.1.0.0 \
    --name "libprint" \
    --description "my first release"
```

Устанавливаем переменные, выкладываем изменения на сервер
```ShellSession
$ export PACKAGE_OS=`uname -s` PACKAGE_ARCH=`uname -m`  - Устанавливаем переменные PACKAGE_OS и PACKAGE_ARCH
$ export PACKAGE_FILENAME=print-${PACKAGE_OS}-${PACKAGE_ARCH}.tar.gz  - Работаем с переменной PACKAGE_FILENAME
$ github-release upload \     - выкладываем изменения на сервер
    --user ${GITHUB_USERNAME} \
    --repo lab09 \
    --tag v0.1.0.0 \
    --name "${PACKAGE_FILENAME}" \
    --file _build/*.tar.gz
```

```ShellSession
$ github-release info -u ${GITHUB_USERNAME} -r lab09    - Узнаем информацию о релизе ЛР9
$ wget https://github.com/${GITHUB_USERNAME}/lab09/releases/download/v0.1.0.0/${PACKAGE_FILENAME}
$ tar -ztf ${PACKAGE_FILENAME}  -  Распаковываем файл
```

## Report

```ShellSession
$ popd        - возвращаемся в сохраненную директорию
$ export LAB_NUMBER=09       - задаем переменную окружения
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER} - клонируем задание лр9 в директорию tasks/lab09
$ mkdir reports/lab${LAB_NUMBER} - создаем папку lab09 в reports
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md      - копируем из tasks README.md и сохраняем его в reports под названием REPORT.md
$ cd reports/lab${LAB_NUMBER}   - заходим в директорию reports/lab09
$ edit REPORT.md     - редактируем REPORT.md
$ gistup -m "lab${LAB_NUMBER}"     - cоздаем gist
```

## Links

- [Create Release](https://help.github.com/articles/creating-releases/)
- [Get GitHub Token](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/)
- [Signing Commits](https://help.github.com/articles/signing-commits-with-gpg/)
- [Go Setup](http://www.golangbootcamp.com/book/get_setup)
- [github-release](https://github.com/aktau/github-release)

```
Copyright (c) 2017 Братья Вершинины
```
