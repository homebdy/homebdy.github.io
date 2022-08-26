---
layout: post
title:  "[Docker] Dockerfile - Django && React"
author: yj
category: [ DevOps☁️]
tags: [ Docker ]
---

### <a href="#">Dockerfile</a>
- Docker image를 생성하기 위한 설정 파일
- 도커 파일을 작성 후 빌드하면 파일의 명령어를 차례대로 수행하여 이미지를 생성해줌
- 생성된 이미지는 이후 docker-compose-yml에서 컨테이너로 생성

### Django Dockerfile

```
FROM python:3.9.0 # base image

ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1

WORKDIR /backend # ./backend 폴더로 이동
COPY requirements.txt /backend/

RUN pip install --upgrade pip
RUN pip install -r requirements.txt --no-cache-dir # requirements.txt이 있는 내용(django) pip를 통해 설치
COPY . /backend # 현재 경로(backend 폴더)에 존재하는 파일들을 컨테이너로 복사


CMD ["bash", "-c", "python3 manage.py migrate && python3 manage.py runserver 0.0.0.0:8000"] # django 서버 실행
```

### React Dockerfile

```
FROM node:alpine # base image

WORKDIR /frontend # ./frontend 폴더로 이동
COPY package.json /frontend/ # package.json 파일의 컨테이너 이미지 안으로 복사

RUN npm install -g npm@8.18.0

COPY . /frontend # 현재 경로(frontend 폴더)에 존재하는 파일들을 컨테이너로 복사
CMD ["npm", "run", "start"] # react 서버 실행
```

### docker-compose.yml

```
version: "3.3"

services:
  react:
    image: frontend
    container_name: react
    build: 
      context: ./frontend
      dockerfile: Dockerfile
    ports:
      - 3000:3000
    volumes:
      - ./frontend:/frontend
      - ./frontend/node_modules/:/frontend/node_modules

  django:
    image: backend
    container_name: django
    build: 
      context: ./backend
      dockerfile: Dockerfile
    ports:
      - 8000:8000
    volumes:
      - ./backend:/backend
    environment:
      POSTGRES_NAME: 
      POSTGRES_USER: 
      POSTGRES_PASSWORD: 
    depends_on:
      - postgresql
   
volumes:
  postgres_data_dev: null
  postgre_admin_data_dev: null
```

## 도커 실행
- `docker=compose up --build` 명령어를 통해 도커를 실행하면 정상적으로 작동한다
![image](https://cdn.discordapp.com/attachments/1006837504554569778/1012376023909806151/unknown.png)