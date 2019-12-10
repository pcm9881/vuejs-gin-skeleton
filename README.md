# Project Structure

## layout
```
.
├── client             # vue.js  
├── server             # golang
└── docker-compose.yml # docker-compose
```

## Requirement
1. vue-cli
2. golang 1.12

### Create vue project (ex: client)

```
$vue create client

Vue CLI v4.1.1
? Please pick a preset:
❯ default (babel, eslint)
  Manually select features
```

### Create Client Dockerfile

``` Dockerfile
FROM node:13-alpine as build-stage
WORKDIR /app
COPY . .
RUN yarn install
```

### Create Server 
```
$ mkdir server
$ cd server
```

### go mod init && go get gin
```
go mod init github.com/{Your Github ID}/{Your Repository Name}
$ go get -u github.com/gin-gonic/gin
```

### Write main.go

``` go
package main // import "server"

import "github.com/gin-gonic/gin"

func main() {
	r := gin.Default()
	r.GET("/", func(c *gin.Context) {
		c.JSON(200, gin.H{
			"message": "ping",
		})
	})
	r.Run(":8888")
}

```

### Create Server Dockerfile

``` Dockerfile
FROM golang:latest

LABEL maintainer="pcm9881 <pcm9881@gmail.com>"

WORKDIR /api

COPY go.mod go.sum ./

RUN go mod download

COPY . .

RUN go build -o main .

EXPOSE 8888

CMD ["./main"]

```

### Create docker-compose 

``` yml
version: '3'

services:
  client:
    build: ./client
    ports:
      - 8080:8080
    volumes:
      - ./client:/app
    command: yarn serve

  server:
    build: ./server
    ports:
      - 8888:8888
    volumes:
      - ./server:/api
```
