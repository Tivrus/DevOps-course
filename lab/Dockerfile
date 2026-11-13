# K2-02. Образ учебного сервиса «Мастерская», основа — templates/Dockerfile.example
# и docker/bootstrap.Dockerfile. Сборка из корня репозитория:
#   docker build -f lab/Dockerfile -t devops-student:v1 lab

# Та же база, что у bootstrap: Python 3.13 на Debian bookworm без лишних пакетов.
FROM python:3.13-slim-bookworm

# Метка с кодом студента (без ФИО). Видна в docker image inspect → Config.Labels.
LABEL course.student="STUDENT-CODE"

# Все следующие пути и команды выполняются в /app.
WORKDIR /app

# Логи Python сразу попадают в docker logs, .pyc-файлы в образе не создаются.
ENV PYTHONUNBUFFERED=1 PYTHONDONTWRITEBYTECODE=1

# В образ попадают только два файла приложения из контекста сборки (папка lab).
COPY app.py index.html ./

# /data — папка для базы заявок (в Compose туда монтируется том requests).
# Её владелец — непривилегированный пользователь 10001, иначе api не сможет писать в базу.
RUN mkdir /data && chown 10001:10001 /data

# Приложение работает не от root, а от UID/GID 10001.
USER 10001:10001

# Документирует порт, который слушает приложение внутри контейнера. Наружу его открывает только -p / ports.
EXPOSE 8000

# Команда при запуске контейнера. Роль (web или api) задаётся переменной ROLE.
CMD ["python", "app.py"]
