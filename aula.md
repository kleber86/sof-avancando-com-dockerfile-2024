# Criando uma imagem docker com flask pelo Dockerfile
```
FROM python:3.7.4-alpine3.9

ENV FLASK_APP app.py
ENV FLASK_RUN_HOST 0.0.0.0
RUN apk add --no-cache gcc musl-dev linux-headers
WORKDIR /code
COPY req.txt req.txt
RUN pip install -r req.txt
COPY . .
EXPOSE 5000

RUN addgroup -S son && adduser -S son -G son

USER son

CMD [ "flask", "run" ]
```

# Criando uma imagem docker-compose, com rede, volume e dependencia
```
version: '3'
services:
  web:
    build: .
    ports:
      - 5000:5000
    volumes:
      - .:/code
    environment:
      - FLASK_RUN_HOST=127.0.0.1
    depends_on:
      - redis
    networks:
      - webnet
      - sonnet
  redis:
    image: redis
    networks:
      - webnet
    volumes:
      - dbdata:/data
  ubuntu:
    build:
      context: ./demo
    command: ["ping", "localhost"]
    networks:
      - sonnet
networks:
  webnet:
  sonnet:
volumes:
  dbdata:
```