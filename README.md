# Laboratory work II

Данная лабораторная работа посвещена изучению систем контроля версий на примере **Git**.

## Tasks

**Настраиваем необходимые переменные окружения:**
```sh
$ export GITHUB_USERNAME=<имя_пользователя>
$ export GITHUB_EMAIL=<адрес_почтового_ящика>
$ export GITHUB_TOKEN=<сгенирированный_токен>
$ alias edit=<nano|vi|vim|subl>
```
**Что делают эти команды:**
- `export` - устанавливает переменные окружения для текущей сессии терминала
- `GITHUB_USERNAME` - хранит имя пользователя GitHub
- `GITHUB_EMAIL` - хранит email, связанный с аккаунтом GitHub
- `GITHUB_TOKEN` - хранит токен доступа к GitHub API
- `alias edit=subl` - создает псевдоним `edit` для текстового редактора `subl`

---

**Перемещаемся в рабочую директорию:**

```sh
$ cd ${GITHUB_USERNAME}/workspace
$ source scripts/activate
```
**Что делают эти команды:**
- `cd` - переходит в директорию workspace
- `source scripts/activate` - активирует окружение (выполняет скрипт настройки)

---

**Настраиваем конфиг `hub`:**

```sh
$ mkdir ~/.config
$ cat > ~/.config/hub <<EOF
github.com:
- user: ${GITHUB_USERNAME}
  oauth_token: ${GITHUB_TOKEN}
  protocol: https
EOF
$ git config --global hub.protocol https
```

**Что делают эти команды:**
- `mkdir ~/.config` - создает директорию для конфигурационных файлов (если не существует)
- `cat > ~/.config/hub <<EOF` - создает файл с конфигурацией hub, подставляя значения переменных
- `github.com: - user:` - указывает username для GitHub
- `oauth_token:` - сохраняет токен для аутентификации
- `protocol: https` - устанавливает протокол HTTPS
- `git config --global hub.protocol https` - добавляет настройку протокола в глобальный конфиг Git

---

**Создаем локальный репозиторий и настраиваем его:**

```sh
$ mkdir projects/lab02 && cd projects/lab02
$ git init
$ git config --global user.name ${GITHUB_USERNAME}
$ git config --global user.email ${GITHUB_EMAIL}
$ git config -e --global
```

**Что делают эти команды:**
- `mkdir projects/lab02` - создает директорию для проекта
- `cd projects/lab02` - переходит в созданную директорию
- `git init` - инициализирует пустой Git репозиторий (создает папку .git)
- `git config --global user.name` - устанавливает имя пользователя для всех коммитов
- `git config --global user.email` - устанавливает email для всех коммитов
- `git config -e --global` - открывает глобальный конфиг для проверки

---

**Подключаемся к удаленному репозиторию:**

```sh
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab02.git
$ git pull origin main
```

**вывод команды:**

```
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
Распаковка объектов: 100% (3/3), 1.44 КиБ | 1.44 МиБ/с, готово.
Из https://github.com/Nikonnor/lab02
 * branch            main       -> FETCH_HEAD
 * [новая ветка]     main       -> origin/main
```

---

**Создаем, коммитим и пушим `README.md`:**

```sh
$ touch README.md
$ git status
$ git add README.md
$ git commit -m "added README.md"
$ git push origin main
```

**Что делают эти команды:**
- `touch README.md` - создает файл README.md
- `git status` - показывает состояние рабочей директории
- `git add README.md` - добавляет файл в staging area
- `git commit -m "added README.md"` - создает коммит с сообщением
- `git push origin main` - отправляет изменения на GitHub
---

**Добавляем на сервисе **GitHub** в репозитории **lab02** файл **.gitignore**
со следующем содержимом:**

```sh
*build*/
*install*/
*.swp
.idea/
```

---

**Далее вытаскиваем с `GitHub` добавленный `.gitignore`:**

```sh
$ git pull origin main
$ git log
```

**Вывод команды:**

```
remote: Enumerating objects: 4, done.
remote: Counting objects: 100% (4/4), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
Распаковка объектов: 100% (3/3), 986 байтов | 986.00 КиБ/с, готово.
Из https://github.com/Nikonnor/lab02
 * branch            main       -> FETCH_HEAD
   fa16854..a06696b  main       -> origin/main
Обновление fa16854..a06696b
Fast-forward
 .gitignore | 4 ++++
 1 file changed, 4 insertions(+)
 create mode 100644 .gitignore
```

---

**Создаем необходимые файлы `.cpp`:**

```sh
$ mkdir sources
$ mkdir include
$ mkdir examples
$ cat > sources/print.cpp <<EOF
#include <print.hpp>

void print(const std::string& text, std::ostream& out)
{
  out << text;
}

void print(const std::string& text, std::ofstream& out)
{
  out << text;
}
EOF
```

```sh
$ cat > include/print.hpp <<EOF
#include <fstream>
#include <iostream>
#include <string>

void print(const std::string& text, std::ofstream& out);
void print(const std::string& text, std::ostream& out = std::cout);
EOF
```

```sh
$ cat > examples/example1.cpp <<EOF
#include <print.hpp>

int main(int argc, char** argv)
{
  print("hello");
}
EOF
```

```sh
$ cat > examples/example2.cpp <<EOF
#include <print.hpp>

#include <fstream>

int main(int argc, char** argv)
{
  std::ofstream file("log.txt");
  print(std::string("hello"), file);
}
EOF
```

```sh
$ edit README.md
```

**Затем коммитим и пушим новые файлы:**

```sh
$ git status
$ git add .
$ git commit -m "added sources"
$ git push origin main
```

## Создаем отчет:

```sh
$ cd ~/workspace/
$ export LAB_NUMBER=02
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER}.git tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gist REPORT.md
```

**Что делают эти команды:**
- `cd ~/workspace/` - переходит в рабочую директорию
- `export LAB_NUMBER=02` - задает номер текущей лабораторной работы
- `git clone` - клонирует шаблон отчета
- `mkdir` - создает директорию для отчета
- `cp` - копирует шаблон в директорию отчета
- `edit REPORT.md` - открывает отчет для редактирования

---

## Homework



### Part I



### Part II



### Part III
