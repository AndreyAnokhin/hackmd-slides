---
title: Git
tags: Git, Geekbrains
description: Введение в git
slideOptions:
  transition: none
  progress: false
  slideNumber: true
  theme: white
---

# Введение в Git и GitHub

[![hackmd-github-sync-badge](https://hackmd.io/aT8acuRNS_SyBIluzMtQRQ/badge)](https://hackmd.io/aT8acuRNS_SyBIluzMtQRQ)


---

<p style='text-align: justify;'>
Git — распределённая система контроля версий, которая даёт возможность разработчикам отслеживать изменения в файлах и работать совместно с другими разработчиками
</p>

---

## Подробнее о git

- Pro Git book https://git-scm.com/book/ru/v2
- Git How To https://githowto.com/ru

---

## Установка и настройка git

---

### Установка Git:

Linux:
```shell
sudo apt-get install git
```
Mac:
```shell
brew install git
```
Windows:
[https://git-scm.com/download/win](https://git-scm.com/download/win)

---

### Первичная настройка Git
<br>

```shell
git config --global user.name "username"
git config --global user.email "username.user@example.com"
```

---

### Инициализация репозитория

```shell
git init
```

---

## Работа с Git

---

<img src="https://i.imgur.com/HMgo2L8.png" style="background:none; border:none; box-shadow:none;">

---

### Текущий статус репозитория


```shell
git status
```

---


### Staging
<p style='text-align: justify;'>
Staging — это совокупность файлов, которые будут добавлены в следующий коммит
</p>

Добавить один файл

```shell
git add file_name
```

Добавить все файлы

```shell
git add .
```

---

### Файл .gitignore

<p style='text-align: justify;'>
Как только в репозиторий был добавлен файл .gitignore, файлы, которые указаны в нём, стали игнорироваться.
</p>

---

### Зафиксировать изменения (коммит)

```shell
git commit -m "Add README and .gitignore files"
```

---

## Ветвление в git

---

<img src="https://i.imgur.com/KIhjzPP.jpg" style="background:none; border:none; box-shadow:none;">

[link - git-branching-tutorial](https://techrocks.ru/2020/01/29/git-branching-tutorial/)

---

Посмотреть список всех ветвей:

```shell
git branch
```

Создать новую ветвь:

```
git branch new-branch-name
```

Переключиться на другую ветвь:

```shell
git checkout branch-name
```

Слияние branch-name ветки с текущей веткой:

```shell
git merge branch-name
```

---

<img src="https://i.imgur.com/cx5srsN.png" style="background:none; border:none; box-shadow:none;">

---

#### Удаленный репозиторий


<img src="https://i.imgur.com/3ZMaMOC.png" style="background:none; border:none; box-shadow:none;">


---

<img src="https://mainacademy.ua/wp-content/uploads/2019/02/github-logo.png" style="background:none; border:none; box-shadow:none;">

---

- Зарегистрироваться

- Настроить аутентификация на GitHub по ключам SSH (опционально) [link](https://pyneng.readthedocs.io/ru/latest/book/02_git_github/git_github_auth.html)



---

### Создание нового репозитория

![](https://i.imgur.com/QChjdX9.png)

---

### Клонирование репозитория с GitHub

```shell
git clone https://github.com/AndreyAnokhin/FlaskAPI_Lesson.git
```

---

<p style='text-align: justify;'>
Предыдущая команда не просто скопировала репозиторий чтобы использовать его локально, но и настроила соответствующим образом Git:

- создан каталог .git
- скачаны все данные репозитория
- скачаны все изменения, которые были в репозитории
- репозиторий на GitHub настроен как remote для локального репозитория
</p>

---

### Подключение существующего репозитория

```shell
git remote add origin https://github.com/geekbrains-user/lessons.git
```

Отправим наши файлы на гитхаб

```shell
git push -u origin master
```

---

### Последовательность работы

- перед началом работы, синхронизация локального содержимого с GitHub командой git pull
- изменение файлов репозитория
- добавление изменённых файлов в staging командой git add
- фиксация изменений через коммит командой git commit
- передача локальных изменений в репозитории на GitHub командой git push

---

### Синхронизация локального репозитория с удалённым

```shell
git pull
```


```shell
git push origin master
```

---

## Как сделать pull request на github

---

### Создание ответвлений (fork)

<p style='text-align: justify;'>
Вам необходимо найти проект на github, в который вы хотите внести вклад. Затем уже на странице с ним нажать на кнопку Fork.
</p>

![](https://i.imgur.com/nZ3jhgT.png)


---

<p style='text-align: justify;'>
Pull Request — это запрос на вливание изменений из вашей ветки в основную ветку исходного репозитория
</p> 


![](https://i.imgur.com/QMeTydn.png)



---

![](https://i.imgur.com/ijs6ghl.png)

---

### Thank you!

