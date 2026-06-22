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



## Part I

### 1. Создем пустой репозиторий на GitHub.

Создли репозиторий `lab02_HW` на GitHub.

### 2. Выполнили инструкцию по созданию первого коммита, создали файл `hello_world.cpp`.

```bash
$ touch hello_world.cpp
$ git add hello_world.cpp
$ git commit -m "Добавили файл hello_world.cpp"
```

 **Вывод:**
```
[main (корневой коммит) fed4f57] Добавили файл hello_world.cpp
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 hello_world.cpp
```

### 3. Добавили код программы в 'hello_world.cpp'.

```bash
#include <iostream>
using namespace std;

int main()
{
    cout << "Hello world!" << endl;
    return 0;
}
```

### 4. Добавили файл в локальную копию репозитория.

```bash
$ git add hello_world.cpp
```

### 5. Закоммитили изменения с осмысленным сообщением.

```bash
$ git commit -m "Добавили код программы в hello_world.cpp"
```

**Вывод:**
```
[main 2bfd14a] Добавили вывод имени пользователя в hello_world.cpp
 1 file changed, 4 insertions(+), 1 deletion(-)
```

### 6. Изменили исходный код: добавили ввод имени пользователя.

```bash
$ cat > hello_world.cpp << 'EOF'
#include <iostream>
#include <string>
using namespace std;

int main()
{
    string name;
    cout << "Введите имя пользователя: "; cin >> name;
    cout << "Hello world from " << name << "!" << endl;
    return 0;
}
EOF
```

### 7. Закоммитили новую версию 'hello_world.cpp'.

```bash
$ git commit -am "Добавили вывод имени пользователя в hello_world.cpp"
```

**Вывод:**
```
[main 2bfd14a] Добавили вывод имени пользователя в hello_world.cpp
 1 file changed, 4 insertions(+), 1 deletion(-)
```

**Почему не надо добавлять файл повторно `git add`?**  
Файл `hello_world.cpp` уже отслеживается Git. Флаг `-am` автоматически добавляет изменения в отслеживаемых файлах перед коммитом.

### 8. Запушили изменения в удаленный репозиторий.

```bash
$ git push -u origin main
```

**Вывод:**
```
Перечисление объектов: 9, готово.
Подсчет объектов: 100% (9/9), готово.
При сжатии изменений используется до 3 потоков
Сжатие объектов: 100% (5/5), готово.
Запись объектов: 100% (9/9), 1.07 КиБ | 1.07 МиБ/с, готово.
Всего 9 (изменений 0), повторно использовано 0 (изменений 0), повторно использовано пакетов 0
To https://github.com/Nikonnor/lab02_HW
 * [new branch]      main -> main
branch 'main' set up to track 'origin/main'.
```

### 9. Проверка истории коммитов в удаленном репозитории

```bash
$ git log
```

**Вывод:**
```
commit 2bfd14aab7e5192c373399b17b4e4468151dff52 (HEAD -> main, origin/main)
Author: Nikonnor <Arakelyannikita.2007@gmail.com>
Date:   Wed May 6 10:19:07 2026 +0300

    Добавили вывод имени пользователя в hello_world.cpp

commit eeaa75dac6fc36878006e39b6cc8e08e2e2d963a
Author: Nikonnor <Arakelyannikita.2007@gmail.com>
Date:   Wed May 6 10:16:08 2026 +0300

    Добавили код программы в hello_world.cpp

commit fed4f573a033e776fd04d0d7ec329f72add1d6b8
Author: Nikonnor <Arakelyannikita.2007@gmail.com>
Date:   Wed May 6 10:12:51 2026 +0300

    Добавили файл hello_world.cpp
```

---

## Part II

### 1. Создали локальную ветку patch1.

```bash
$ git checkout -b patch1
```

**Вывод:**
```
Переключились на новую ветку «patch1»
```

### 2. Внесели изменения в 'hello_world.cpp': удалили `using namespace std;`.

```cpp
#include <iostream>
#include <string>

int main()
{
    std::string name;
    std::cout << "Введите имя пользователя: "; std::cin >> name;
    std::cout << "Hello world from " << name << "!" << std::endl;
    return 0;
}
```

### 3. Коммитим и пушим ветку patch1.

```bash
$ git add hello_world.cpp
$ git commit -m "Удалили 'using namespace std;' из hello_world.cpp"
```

**Вывод:**
```
[patch1 344b1f6] Удалили 'using namespace std' из hello_world.cpp
 1 file changed, 3 insertions(+), 4 deletions(-)
```

```bash
$ git push -u origin patch1
```

**Вывод:**
```
Перечисление объектов: 5, готово.
Подсчет объектов: 100% (5/5), готово.
При сжатии изменений используется до 3 потоков
Сжатие объектов: 100% (2/2), готово.
Запись объектов: 100% (3/3), 473 байта | 473.00 КиБ/с, готово.
Всего 3 (изменений 0), повторно использовано 0 (изменений 0), повторно использовано пакетов 0
remote: 
remote: Create a pull request for 'patch1' on GitHub by visiting:
remote:      https://github.com/Nikonnor/lab02_HW/pull/new/patch1
remote: 
To https://github.com/Nikonnor/lab02_HW
 * [new branch]      patch1 -> patch1
branch 'patch1' set up to track 'origin/patch1'.
```

### 4. Ветка patch1 доступна в удаленном репозитории.

```bash
$ git log
```

**Вывод:**
```
commit 344b1f6689f31b0637ab8ac143df61f13c5c23e3 (HEAD -> patch1, origin/patch1)
Author: Nikonnor <Arakelyannikita.2007@gmail.com>
Date:   Wed May 6 10:26:06 2026 +0300

    Удалили 'using namespace std' из hello_world.cpp

commit 2bfd14aab7e5192c373399b17b4e4468151dff52 (origin/main, main)

...

```

### 5. Создли pull-request №1 (patch1 → main).

На GitHub создан Pull Request №1.

### 6. Добавили комментарии в код 'hello_world.cpp' в ветке patch1.

```cpp
// Вводим необходимые библиотеки
#include <iostream>
#include <string>

int main() // Задаем главную функцию
{
    std::string name; // Задаем новую переменную name типа string
    std::cout << "Введите имя пользователя: "; std::cin >> name; // Задаем переменную name через стандартный поток ввода
    std::cout << "Hello world from " << name << "!" << std::endl; // Выводим результат работы программы
    return 0;
}
```

### 7. Коммитим и пушим.

```bash
$ git add hello_world.cpp
$ git commit -m "Добавили комментарии в hello_world.cpp"
```

**Вывод:**
```
[patch1 3e49635] Добавили комментарии в hello_world.cpp
 1 file changed, 5 insertions(+), 4 deletions(-)
```

```bash
$ git push origin patch1
```

**Вывод:**
```
Перечисление объектов: 5, готово.
Подсчет объектов: 100% (5/5), готово.
При сжатии изменений используется до 3 потоков
Сжатие объектов: 100% (2/2), готово.
Запись объектов: 100% (3/3), 641 байт | 641.00 КиБ/с, готово.
Всего 3 (изменений 0), повторно использовано 0 (изменений 0), повторно использовано пакетов 0
To https://github.com/Nikonnor/lab02_HW
   344b1f6..3e49635  patch1 -> patch1
```

### 8. Новые изменения появились в pull-request №1.

На GitHub в Pull Request №1 отобразились новые коммиты с комментариями.

### 9. Мерджим PR patch1 → main и удаляем ветку patch1 в удаленном репозитории.

На GitHub выполнили merge Pull Request №1, удалили ветку `patch1` в удаленном репозитории.

### 10. Локально выполнили pull.

```bash
$ git pull origin main
```

**Вывод:**
```
remote: Enumerating objects: 1, done.
remote: Counting objects: 100% (1/1), done.
remote: Total 1 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
Распаковка объектов: 100% (1/1), 947 байтов | 947.00 КиБ/с, готово.
Из https://github.com/Nikonnor/lab02_HW
 * branch            main       -> FETCH_HEAD
   2bfd14a..d881be3  main       -> origin/main
Обновление 2bfd14a..d881be3
Fast-forward
 hello_world.cpp | 10 +++++-----
 1 file changed, 5 insertions(+), 5 deletions(-)
```

### 11. Просматриваем историю в локальной версии ветки main.

```bash
$ git log
```

**Вывод:**
```
commit d881be3499b1198f19c4867adfdb2cf9714683db (HEAD -> main, origin/main)
Merge: 2bfd14a 3e49635
Author: Nikonnor <161149067+Nikonnor@users.noreply.github.com>
Date:   Wed May 6 10:35:17 2026 +0300

    Merge pull request #1 from Nikonnor/patch1
    
    Удалили 'using namespace std' из hello_world.cpp

commit 3e4963518ed9167041c8eb46c7187b3d409a2a7e (origin/patch1, patch1)
Author: Nikonnor <Arakelyannikita.2007@gmail.com>
Date:   Wed May 6 10:33:17 2026 +0300

    Добавили комментарии в hello_world.cpp

commit 344b1f6689f31b0637ab8ac143df61f13c5c23e3
Author: Nikonnor <Arakelyannikita.2007@gmail.com>
Date:   Wed May 6 10:26:06 2026 +0300

    Удалили 'using namespace std' из hello_world.cpp

commit 2bfd14aab7e5192c373399b17b4e4468151dff52
Author: Nikonnor <Arakelyannikita.2007@gmail.com>
Date:   Wed May 6 10:19:07 2026 +0300

    Добавили вывод имени пользователя в hello_world.cpp

commit eeaa75dac6fc36878006e39b6cc8e08e2e2d963a
Author: Nikonnor <Arakelyannikita.2007@gmail.com>
Date:   Wed May 6 10:16:08 2026 +0300

    Добавили код программы в hello_world.cpp

commit fed4f573a033e776fd04d0d7ec329f72add1d6b8
Author: Nikonnor <Arakelyannikita.2007@gmail.com>
Date:   Wed May 6 10:12:51 2026 +0300

    Добавили файл hello_world.cpp
```

### 12. Удалили локальную ветку patch1.

```bash
$ git branch -d patch1
```

**Вывод:**
```
Ветка patch1 удалена (была 3e49635).
```

---

## Part III

### 1. Создали новую локальную ветку patch2.

```bash
$ git checkout -b patch2
```

**Вывод:**
```
Переключились на новую ветку «patch2»
```

### 2. Изменили code style с помощью clang-format.

```bash
$ clang-format -style Mozilla -i hello_world.cpp
```

**Файл после форматирования:**
```cpp
// Вводим необходимые библиотеки
#include <iostream>
#include <string>

int
main() // Задаем главную функцию
{
  std::string name; // Задаем новую переменную name типа string
  std::cout << "Введите имя пользователя: ";
  std::cin >> name; // Задаем переменную name через стандартный поток ввода
  std::cout << "Hello world from " << name << "!"
            << std::endl; // Выводим результат работы программы
  return 0;
}
```

### 3. Коммитим, пушим, создём pull-request №2 (patch2 → main).

```bash
$ git commit -am "Изменили code style с помощью clang-format в 'hello_world.cpp'"
```

**Вывод:**
```
patch2 c492306] Изменили code style с помощью clang-format в hello_world.cpp
 1 file changed, 8 insertions(+), 5 deletions(-)
```

```bash
$ git push origin patch2
```

**Вывод:**
```
Перечисление объектов: 5, готово.
Подсчет объектов: 100% (5/5), готово.
При сжатии изменений используется до 3 потоков
Сжатие объектов: 100% (2/2), готово.
Запись объектов: 100% (3/3), 402 байта | 402.00 КиБ/с, готово.
Всего 3 (изменений 1), повторно использовано 0 (изменений 0), повторно использовано пакетов 0
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
remote: 
remote: Create a pull request for 'patch2' on GitHub by visiting:
remote:      https://github.com/Nikonnor/lab02_HW/pull/new/patch2
remote: 
To https://github.com/Nikonnor/lab02_HW
 * [new branch]      patch2 -> patch2
```

Создали Pull Request №2 (patch2 → main) на GitHub.

### 4. Исправили пунктуацию комментариев в 'hello_world.cpp' ветке main.

```cpp
// Вводим необходимые библиотеки.
#include <iostream>
#include <string>

int main() // Задаем главную функцию.
{
    std::string name; // Задаем новую переменную 'name' типа 'string'.
    std::cout << "Введите имя пользователя: "; std::cin >> name; // Задаем переменную 'name' через стандартный поток ввода.
    std::cout << "Hello world from " << name << "!" << std::endl; // Выводим результат работы программы.
    return 0;
}
```

```bash
$ git commit -am "Исправили пунктуацию комментариев в hello_world.cpp"
$ git push origin main
```

**Вывод:**
```
[main 61a8cea] Исправили пунктуацию комментариев в hello_world.cpp
 1 file changed, 5 insertions(+), 5 deletions(-)
Перечисление объектов: 5, готово.
Подсчет объектов: 100% (5/5), готово.
При сжатии изменений используется до 3 потоков
Сжатие объектов: 100% (2/2), готово.
Запись объектов: 100% (3/3), 388 байтов | 388.00 КиБ/с, готово.
Всего 3 (изменений 1), повторно использовано 0 (изменений 0), повторно использовано пакетов 0
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To https://github.com/Nikonnor/lab02_HW
   d881be3..61a8cea  main -> main
```

### 5. В pull request №2 появились конфликты.

На GitHub в pull request №2 отобразилось сообщение о конфликте.

### 6. Локально выполнили pull + rebase, исправили конфликты.

```bash
$ git checkout patch2
$ git rebase main
```

**Вывод:**
```
Переключились на ветку «patch2»
Автослияние hello_world.cpp
КОНФЛИКТ (содержимое): Конфликт слияния в hello_world.cpp
error: не удалось применить коммит c492306... Изменили code style с помощью clang-format в hello_world.cpp
подсказка: Resolve all conflicts manually, mark them as resolved with
подсказка: "git add/rm <conflicted_files>", then run "git rebase --continue".
подсказка: You can instead skip this commit: run "git rebase --skip".
подсказка: To abort and get back to the state before "git rebase", run "git rebase --abort".
Не удалось применить коммит c492306... Изменили code style с помощью clang-format в hello_world.cpp
```

**Конфликт в файле:**
```cpp
// Вводим необходимые библиотеки.
#include <iostream>
#include <string>

<<<<<<< HEAD
int main() // Задаем главную функцию.
{
	std::string name; // Задаем новую переменную 'name' типа 'string'.
	std::cout << "Введите имя пользователя: "; std::cin >> name; // Задаем переменную 'name' через стандартный поток ввода.
	std::cout << "Hello world from " << name << "!" << std::endl; // Выводим результат работы программы.
	return 0;
=======
int
main() // Задаем главную функцию
{
  std::string name; // Задаем новую переменную name типа string
  std::cout << "Введите имя пользователя: ";
  std::cin >> name; // Задаем переменную 'name' через стандартный поток ввода
  std::cout << "Hello world from " << name << "!"
            << std::endl; // Выводим результат работы программы
  return 0;
>>>>>>> c492306 (Изменили code style с помощью clang-format в hello_world.cpp)
}
```

**Исправление конфликта:**

**Исправленный файл:**
```cpp
// Вводим необходимые библиотеки.
#include <iostream>
#include <string>

int
main() // Задаем главную функцию.
{
  std::string name; // Задаем новую переменную 'name' типа 'string'.
  std::cout << "Введите имя пользователя: ";
  std::cin >> name; // Задаем переменную 'name' через стандартный поток ввода.
  std::cout << "Hello world from " << name << "!"
            << std::endl; // Выводим результат работы программы.
  return 0;
}
```

```bash
$ git add hello_world.cpp
$ git rebase --continue
```

**Вывод:**
```
[отделённый HEAD 0fd83b1] Изменили code style с помощью clang-format в hello_world.cpp
 1 file changed, 8 insertions(+), 5 deletions(-)
Успешно перемещён и обновлён refs/heads/patch2.
```

### 7. Выполнили force push в ветку patch2.

```bash
$ git push --force origin patch2
```

**Вывод:**
```
Перечисление объектов: 5, готово.
Подсчет объектов: 100% (5/5), готово.
При сжатии изменений используется до 3 потоков
Сжатие объектов: 100% (2/2), готово.
Запись объектов: 100% (3/3), 408 байтов | 408.00 КиБ/с, готово.
Всего 3 (изменений 1), повторно использовано 0 (изменений 0), повторно использовано пакетов 0
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To https://github.com/Nikonnor/lab02_HW
 + c492306...0fd83b1 patch2 -> patch2 (forced update)
```

### 8. Конфликты в pull-request исчезли.

На GitHub в Pull Request №2 конфликты больше не отображаются.

### 9. Выполнен merge pull-request №2 (patch2 → main).

На GitHub выполнен merge Pull Request №2.

```bash
$ git log
```

**Вывод:**
```
commit 61a8cea29d94ea27c2bc4c62141dca3bd256399e (HEAD -> main, origin/main)
Author: Nikonnor <Arakelyannikita.2007@gmail.com>
Date:   Wed May 6 10:49:40 2026 +0300

    Исправили пунктуацию комментариев в hello_world.cpp

commit d881be3499b1198f19c4867adfdb2cf9714683db
Merge: 2bfd14a 3e49635
Author: Nikonnor <161149067+Nikonnor@users.noreply.github.com>
Date:   Wed May 6 10:35:17 2026 +0300

    Merge pull request #1 from Nikonnor/patch1
    
    Удалили 'using namespace std' из hello_world.cpp

commit 3e4963518ed9167041c8eb46c7187b3d409a2a7e (origin/patch1)
Author: Nikonnor <Arakelyannikita.2007@gmail.com>
Date:   Wed May 6 10:33:17 2026 +0300

    Добавили комментарии в hello_world.cpp

commit 344b1f6689f31b0637ab8ac143df61f13c5c23e3
Author: Nikonnor <Arakelyannikita.2007@gmail.com>
Date:   Wed May 6 10:26:06 2026 +0300

    Удалили 'using namespace std' из hello_world.cpp

commit 2bfd14aab7e5192c373399b17b4e4468151dff52
Author: Nikonnor <Arakelyannikita.2007@gmail.com>
Date:   Wed May 6 10:19:07 2026 +0300

    Добавили вывод имени пользователя в hello_world.cpp

commit eeaa75dac6fc36878006e39b6cc8e08e2e2d963a
Author: Nikonnor <Arakelyannikita.2007@gmail.com>
Date:   Wed May 6 10:16:08 2026 +0300

    Добавили код программы в hello_world.cpp
```

---

## Выводы

В ходе выполнения лабораторной работы я:
1. Создал локальный и удаленный Git-репозитории.
2. Научился делать коммиты и пушить изменения на GitHub.
3. научился создавать ветки (`patch1`, `patch2`).
4. Научился создавать pull request-ы.
