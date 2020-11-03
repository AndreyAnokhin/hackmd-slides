---
title: Model Deployment
tags: Flask, ML
description: deploy ML models
slideOptions:
  transition: none
  progress: false
  slideNumber: true
  theme: white
---

[![hackmd-github-sync-badge](https://hackmd.io/IrIK4C0RT4KcW8ZaBcx0Mw/badge)](https://hackmd.io/IrIK4C0RT4KcW8ZaBcx0Mw)


## Деплой моделей машинного обучения

---

<img src="https://i.imgur.com/otgNNMs.png" style="background:none; border:none; box-shadow:none;">


---

### Static vs Dynamic Training

<img src="https://i.imgur.com/5rAVue6.png" style="background:none; border:none; box-shadow:none;">

---


### Static vs Dynamic Training

<img src="https://i.imgur.com/VZkRFLP.png" style="background:none; border:none; box-shadow:none;">

---

### Static vs Dynamic Serving

<img src="https://i.imgur.com/sAW6E1g.png" style="background:none; border:none; box-shadow:none;">

---


### Deployment patterns:
- on user's device
- on server

---

### Dynamic deployment on a server:
- deployment on a virtual machine
- deployment in a container
- serverless deployment

---

### Deployment strategies:
- single deployment
- silent deployment
- canary deployment
- multi-armed bandit

---

## A typical ML workflow

<img src="https://i.imgur.com/dVasgK9.png" style="background:none; border:none; box-shadow:none;">


---

## API JSON

<img src="https://i.imgur.com/OAmBZn2.png" style="background:none; border:none; box-shadow:none;"   width="843"
     height="507">

---

### JSON

<p style='text-align: justify; font-size: 25pt'>
JSON (JavaScript Object Notation) - простой формат обмена данными, удобный для чтения и написания как человеком, так и компьютером.
</p>

![Imgur](https://i.imgur.com/ZsE380O.jpg)

---

### Создание проекта

- Pycharm
- venv
- версия python и requirements.txt

---

### Web-фреймворк для API


<img src="https://i.imgur.com/pcJDuUM.png" style="background:none; border:none; box-shadow:none;">

---

### Обработка ошибок


---

### Логгирование

---

### Тестирование 

По уровню тестирование бывает:
<div style='text-align: justify; font-size: 25pt'>
<ul>
<li>Модульное / юнит-тестирование – проверка корректной работы отдельных единиц ПО. Этот вид тестирования могут выполнять сами разработчики.</li>
<li>Интеграционное тестирование – проверка взаимодействия между несколькими единицами ПО.</li>
<li>Системное – проверка работы всей системы на соответствие заявленным требованиям к программному продукту.</li>
</ul>
</div>

---

### Модульное тестирование

Фреймворк PyTest

```shell
python -m pytest -vv
```

---

### Пример работы с фронт-системой

---

### Production server

<img src="https://i.imgur.com/au1279Y.jpg" style="background:none; border:none; box-shadow:none;">

---

### Gunicorn

<p style='text-align: justify; font-size: 25pt'>Gunicorn 'Green Unicorn' is a Python WSGI HTTP Server for UNIX.</p>

```shell
gunicorn -w 4 -b 127.0.0.1:5000 app:app
```

---

### Тестирование производительности 

ab - Apache HTTP server benchmarking tool

```shell
ab -c 100 -n 1000 http://127.0.0.1:5000/

```
<div style='text-align: justify; font-size: 25pt'>
-c concurrency - количество параллельных запросов в единицу времени.<br>
-n requests - количество запросов, которое необходимо выполнить в течение сессии тестирования.
</div>

---

### Deploy to Heroku

Procfile:
```shell
web: gunicorn app:app
```


Buildpacks:
```shell
heroku/python
https://github.com/heroku/heroku-buildpack-jvm-common.git
```

---


### gRPC

<div style='text-align: justify; font-size: 25pt'>
gRPC - это высокопроизводительный фреймворк разработанный компанией Google для вызов удаленных процедур (RPC), работает поверх HTTP/2.
<br><br>
Protobuf - формат сериализации, использует строгую типизацию полей и бинарный формат для передачи  и данных потребляет значительно меньше ресурсов чем JSON.
</div>


---

### Platforms for managing the machine learning lifecycle

- [MLFlow](https://mlflow.org/)
- [Data Version Control (DVC)](https://dvc.org/doc/use-cases/versioning-data-and-model-files)
- [MetaFlow](https://metaflow.org/)

---

## Thank you