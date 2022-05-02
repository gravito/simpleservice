# simple-service

`simple-service` is an 'as easy as possible' golang HTTP api for examples and tests.

It has only a `/live` endpoint answering `text/plain; charset=utf-8`. The following responses are possibly:
- `Well done :)`: if the application was able to connect with a Postgres database
- `Running`: if some error occurred during the connection with the database

Check the [`config`](/config/) package for more details on how to configure the database connection.

## Building

`simple-service` uses [go modules](https://github.com/golang/go/wiki/Modules). To build the application you can use the following command:

```
GO111MODULE=on go build
```

The command above will generate a binary `simple-service` that starts the application.

## How to Run App locally with Docker Compose

Simply run:
```
	docker-compose up -d 
```

This will create the app. Access it by using CURL:
```
	curl http://localhost:8080/live	
```

If you need to edit the Dockerfile and rework, run following first to tear down:
```
	docker-compose down --remove-orphans --volumes 
```

Then run following to build again:
```
	docker-compose up --build
```

NOTE: All environment values are picked from .env file. Modify accordingly.

DockerHub Image: gravito/simple-service:v1