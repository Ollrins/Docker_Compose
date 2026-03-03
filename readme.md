# Домашнее задание по Docker

## Задание 1: FastAPI приложение с MySQL


### 1. Создание Dockerfile.python (single stage)
<p align="center">
  <img src="screenshots2/dockerfile-single.png" alt="Dockerfile.python single stage" width="800"/>
  <br>
  <em>Рисунок 1 - Создание Dockerfile.python с базовым образом python:3.12-slim и конструкцией COPY .</em>
</p>

### 2. Создание .dockerignore
<p align="center">
  <img src="screenshots2/dockerignore.png" alt=".dockerignore файл" width="700"/>
  <br>
  <em>Рисунок 2 - Файл .dockerignore для исключения ненужных файлов</em>
</p>

### 3. Сборка и тестирование single stage образа
<p align="center">
  <img src="screenshots2/build-single.png" alt="Сборка single stage образа" width="900"/>
  <br>
  <em>Рисунок 3 - Сборка образа командой docker build -f Dockerfile.python -t test-app .  Проверка работы приложения через curl http://localhost:5000 (получено предупреждение о неверном порте)</em>
</p>

### 4. Multistage сборка
<p align="center">
  <img src="screenshots2/dockerfile-multi.png" alt="Dockerfile.python multistage" width="800"/>
  <br>
  <em>Рисунок 4 - Изменение Dockerfile.python на multistage сборку</em>
</p>

<p align="center">
  <img src="screenshots2/build-multi.png" alt="Сборка multistage образа" width="900"/>
  <br>
  <em>Рисунок 5 - Сборка multistage образа test-app-multi</em>
</p>

### 5. Запуск через docker-compose
<p align="center">
  <img src="screenshots2/compose-yaml.png" alt="compose.yaml файл" width="800"/>
  <br>
  <em>Рисунок 6 - Файл compose.yaml для запуска MySQL и приложения</em>
</p>
### 3.Проверка работы приложения через curl 
<p align="center">
  <img src="screenshots2/curl-5000.png" alt="Сборка single stage образа" width="900"/>
  <br>
  <em>Рисунок 7 -  Проверка работы приложения через curl http://localhost:5000 (получено предупреждение о неверном порте)</em>
</p>


### 6. ✨ Запуск с venv (без Docker)
<p align="center">
  <img src="screenshots2/mysql-container.png" alt="Запуск MySQL контейнера" width="900"/>
  <br>
  <em>Рисунок 12 - Запуск MySQL в контейнере для локальной разработки. Создание и активация виртуального окружения, установка зависимостей. </em>
</p>

<p align="center">
  <img src="screenshots2/venv-run.png" alt="Запуск приложения через venv" width="900"/>
  <br>
  <em>Рисунок 14 - Запуск приложения через uvicorn на порту 5001 (подключение к БД успешно)</em>
</p>

<p align="center">
  <img src="screenshots2/venv-curl.png" alt="Проверка через curl" width="800"/>
  <br>
  <em>Рисунок 15 - Проверка работы приложения через curl (получено предупреждение)</em>
</p>

### 7. ✨ Добавление ENV переменной DB_TABLE
<p align="center">
  <img src="screenshots2/main-py-config.png" alt="Изменение конфигурации в main.py" width="850"/>
  <br>
  <em>Рисунок 16 - Добавление переменной db_table в секцию конфигурации</em>
</p>

<p align="center">
  <img src="screenshots2/custom-table-start.png" alt="Запуск с кастомной таблицей" width="900"/>
  <br>
  <em>Рисунок 18 - Запуск приложения с DB_TABLE='my_custom_table' (таблица создана)</em>
</p>

<p align="center">
  <img src="screenshots2/custom-table-curl.png" alt="Проверка с кастомной таблицей" width="800"/>
  <br>
  <em>Рисунок 19 - Проверка работы с кастомной таблицей через curl</em>
</p>

### 8. Итоговые результаты
<p align="center">
  <img src="screenshots2/works-5002.png" alt="Успешная работа на порту 5002" width="850"/>
  <br>
  <em>Рисунок 20 - Успешная работа приложения на порту 5002 с кастомной таблицей</em>
</p>
