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