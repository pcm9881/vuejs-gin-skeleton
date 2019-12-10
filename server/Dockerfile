FROM golang:latest

LABEL maintainer="pcm9881 <pcm9881@gmail.com>"

WORKDIR /app

COPY go.mod go.sum ./

RUN go mod download

COPY . .

RUN go build -o main .

EXPOSE 8888

CMD ["./main"]