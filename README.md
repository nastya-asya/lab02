## Laboratory work II

<a href="https://yandex.ru/efir/?stream_id=vMPJl0nEKr_0"><img src="https://raw.githubusercontent.com/tp-labs/lab02/master/preview.png" width="640"/></a>

Данная лабораторная работа посвещена изучению систем контроля версий на примере **Git**.

```bash
$ open https://git-scm.com
```

## Tasks

- [x] 1. Создать публичный репозиторий с названием **lab02** и с лиценцией **MIT**
- [x] 2. Сгенирировать токен для доступа к сервису **GitHub** с правами **repo**
- [x] 3. Ознакомиться со ссылками учебного материала
- [x] 4. Выполнить инструкцию учебного материала
- [x] 5. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial

```sh
$ export GITHUB_USERNAME=nastya-asya
$ export GITHUB_EMAIL=nastya.asya08@yandex.ru
$ export GITHUB_TOKEN=****************************************
$ alias edit=subl
```

```sh
$ cd ${GITHUB_USERNAME}/workspace
$ source scripts/activate
```

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

```sh
$ mkdir projects/lab02 && cd projects/lab02
$ git init
$ git config --global user.name ${GITHUB_USERNAME}
$ git config --global user.email ${GITHUB_EMAIL}
# check your git global settings
$ git config -e --global
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab02.git
$ git pull origin master

remote: Counting objects: 100% (3/3), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Распаковка объектов: 100% (3/3), готово.
Из https://github.com/nastya-asya/lab02
 * branch            master     -> FETCH_HEAD
 * [новая ветка]     master     -> origin/master


$ touch README.md
$ git status
$ git add README.md
$ git commit -m"added README.md"
$ git push origin master
```

Добавить на сервисе **GitHub** в репозитории **lab02** файл **.gitignore**
со следующем содержимом:

```sh
*build*/
*install*/
*.swp
.idea/
```

```sh
$ git pull origin master
$ git log
```

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

```sh
$ git status
$ git add .
$ git commit -m"added sources"
$ git push origin master
```

## Report

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

## Homework

### Part I

1. Создайте пустой репозиторий на сервисе github.com (или gitlab.com, или bitbucket.com).

2. Выполните инструкцию по созданию первого коммита на странице репозитория, созданного на предыдещем шаге.

```sh
$ mkdir hw02
$ cd hw02
$ echo "# hw02" >> README.md
$ git init
$ git commit -m "first commit"
$ git push origin master
```

3. Создайте файл `hello_world.cpp` в локальной копии репозитория (который должен был появиться на шаге 2). Реализуйте программу **Hello world** на языке C++ используя плохой стиль кода. Например, после заголовочных файлов вставьте строку `using namespace std;`.

```sh
$ cat > hello_world.cpp <<EOF
#include <iostream>
using namespace std;

int main()
{
	cout<<"Hello world"<<endl;
	system ("pause");
	return 0;
}
EOF
```

4. Добавьте этот файл в локальную копию репозитория.

```sh
$ git add .
```

5. Закоммитьте изменения с *осмысленным* сообщением.

```sh
$ git commit -m "added hello_world.cpp"
```

6. Изменитьте исходный код так, чтобы программа через стандартный поток ввода запрашивалось имя пользователя. А в стандартный поток вывода печаталось сообщение `Hello world from @name`, где `@name` имя пользователя.

```sh
$ cat > hello_world.cpp <<EOF
#include <iostream>
using namespace std;

int main()
{
	string name;
	getline(cin, name);
	cout<<"Hello world from"<<name<<endl;
	system ("pause");
	return 0;
}
EOF
```

7. Закоммитьте новую версию программы. Почему не надо добавлять файл повторно `git add`?

```sh
$ git commit -a -m "update hello_world.cpp"
```

8. Запуште изменения в удалёный репозиторий.

```sh
$ git push origin master
```

9. Проверьте, что история коммитов доступна в удалёный репозитории.

### Part II

**Note:** *Работать продолжайте с теми же репоззиториями, что и в первой части задания.*
1. В локальной копии репозитория создайте локальную ветку `patch1`.

```sh
$ git checkout -b patch1
```

2. Внесите изменения в ветке `patch1` по исправлению кода и избавления от `using namespace std;`.

```sh
$ subl hello_world.cpp
$ cat > hello_world.cpp <<EOF
#include <iostream>

int main()
{
	std::string name;
	getline(cin, name);
	std::cout<<"Hello world from"<<name<<std::endl;
	system ("pause");
	return 0;
}
EOF
```

3. **commit**, **push** локальную ветку в удалённый репозиторий.

```sh
$ git commit -m "delete using namespace std"
$ git push origin patch1
```

4. Проверьте, что ветка `patch1` доступна в удалёный репозитории.
5. Создайте pull-request `patch1 -> master`.
6. В локальной копии в ветке `patch1` добавьте в исходный код комментарии.

```sh
$ cat > hello_world.cpp <<EOF
#include <iostream>

int main()
{
	std::string name;
	getline(cin, name);                                   //ввод имени пользователя
	std::cout<<"Hello world from"<<name<<std::endl;      //печать
	system ("pause");
	return 0;
}
EOF
```

7. **commit**, **push**.

```sh
$ git commit -a -m "add comments"
$ git push origin patch1
```

8. Проверьте, что новые изменения есть в созданном на **шаге 5** pull-request
9. В удалённый репозитории выполните  слияние PR `patch1 -> master` и удалите ветку `patch1` в удаленном репозитории.
10. Локально выполните **pull**.

```sh
$ git checkout master
$ git merge patch1
```

11. С помощью команды **git log** просмотрите историю в локальной версии ветки `master`.

```sh
$ git log
```

12. Удалите локальную ветку `patch1`.

```sh
$ git branch -d patch1
```


### Part III

**Note:** *Работать продолжайте с теми же репоззиториями, что и в первой части задания.*
1. Создайте новую локальную ветку `patch2`.

```sh
$ git checkout patch2
```

2. Измените *code style* с помощью утилиты [**clang-format**](http://clang.llvm.org/docs/ClangFormat.html). Например, используя опцию `-style=Mozilla`.

```sh
$ clang-format -style=Mozilla -i hello_world.cpp
```

3. **commit**, **push**, создайте pull-request `patch2 -> master`.

```sh
$ git commit -a -m "new style"
$ git push origin patch2
```

4. В ветке **master** в удаленном репозитории измените комментарии, например, расставьте знаки препинания, переведите комментарии на другой язык.
5. Убедитесь, что в pull-request появились *конфликтны*.
6. Для этого локально выполните **pull** + **rebase** (точную последовательность команд, следует узнать самостоятельно). **Исправьте конфликты**.

```sh
$ git checkout master
$ git pull origin master
$ git checkout patch2
$ git rebase master
$ edit hello_world.cpp
$ git add hello_world.cpp
$ git commit -m "fix conflicts"
$ git rebase --continue
$ git checkout master
$ git merge patch2
```

7. Сделайте *force push* в ветку `patch2`

```sh
$ git push -f origin patch2
```

8. Убедитель, что в pull-request пропали конфликтны. 
9. Вмержите pull-request `patch2 -> master`.

## Links

- [hub](https://hub.github.com/)
- [GitHub](https://github.com)
- [Bitbucket](https://bitbucket.org)
- [Gitlab](https://about.gitlab.com)
- [LearnGitBranching](http://learngitbranching.js.org/)

```
Copyright (c) 2015-2020 The ISC Authors
```
