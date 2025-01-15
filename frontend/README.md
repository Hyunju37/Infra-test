# React + Nginx 도커커컨테이너 설정

### React Only
```dockerFile
FROM node:22 #노드 버전
WORKDIR /src
COPY package.json . #의존 패키지 담겨있는 파일 복사
RUN npm install #의존 패키지 모두 설치
COPY . . #내용물 복사
EXPOSE 3000 #포트 노출
CMD ["npm", "start"] #서버 실행
```

도커 명령어
1. 이미지 만들기\
`docker build -t 이미지이름 도커파일경로`
2. 컨테이너 만들기\
`docker run --name 컨테이너이름 -p 3000:3000 이미지이름`

- **첫 번째 `3000`**: 호스트(컴퓨터)의 포트 번호입니다.
- **두 번째 `3000`**: 컨테이너 내부의 포트 번호입니다.\
첫 번째 포트 번호 수정 가능 → localhost:xxxx 이렇게 접속하면 화면 보인다.

### React + Nginx Version
```dockerFile
FROM node:22 AS build
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build
FROM nginx
COPY --from=build /app/build /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```
