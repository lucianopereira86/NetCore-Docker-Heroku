# NetCore-Docker-Heroku
Simple guide to upload a .Net Core web API to Heroku by using Docker.

## Technologies
- .Net Core 2.2
- Docker
- Heroku

## Before start

This guide is directed for those who have a mininum experience with Docker and a proper environment to run it.
Also, the .Net Core 2.2 SDK must be installed in the machine.

## Heroku

Heroku is a cloud platform that allows applications be hosted at will. It's mostly used for web APIs. 
Although it has some costs, in this example we will use its free features.

Go to [Heroku website](https://signup.heroku.com/login) and sign up or sign in.

In the Dashboard, click on "Create new app"

![heroku01](/docs/heroku01.JPG)

Name the app and click on "Create app"

![heroku02](/docs/heroku02.JPG)

This is your app dashboard.

![heroku03](/docs/heroku03.JPG)

Install the last version of Heroku CLI [here](https://devcenter.heroku.com/articles/heroku-cli)

## .Net Core 

Download .Net Core project template from this [Github](https://github.com/estevaobraga/apimodelo-netcore)

In the root of the project (where the "apimodelo.netcore.sln" is present), run this command in a terminal:
```batch
dotnet publish -c Release
```

The packages will be downloaded and the project will be built for release. Go to the publish folder:
```batch
cd "<ROOT>\apimodelo.netcore.presentation.webapi\bin\Release\netcoreapp2.2\publish\"
```

Create manually a file called "Dockerfile" without extension inside the folder, edit it with a notepad and copy/paste these lines:
```batch
FROM microsoft/dotnet:2.2.0-aspnetcore-runtime
WORKDIR /app
COPY . .
CMD ASPNETCORE_URLS=http://*:$PORT dotnet apimodelo.netcore.presentation.webapi.dll
```

## Docker

Do you remember the name of the app you have created in the Heroku website?
It's the time to use it. 
Replace <MYAPP> by your app's name in the following commands.
Inside the publish folder, run in the terminal:

```batch
docker build -t <MYAPP> .
```

This will build a container for the .Net Core project with the same name of your app.
To authenticate with Heroku, run these commands and follow the instructions:

```batch
heroku login
```

```batch
heroku container:login
```

It's time to registry the app to receive the container

```batch
docker tag <MYAPP> registry.heroku.com/<MYAPP>/web
```

Now, upload the container to Heroku

```batch
heroku container:push web -a <MYAPP>
```

Finally, release it so the app can be updated

```batch
heroku container:release web -a <MYAPP>
```

Access the URL of your app with Swagger

```batch
https://<MYAPP>.herokuapp.com/swagger/index.html
```

It's Done!

## Conclusion

It was really easy to interact with Heroku.
You just had to have basic knowledge about Docker and .Net Core.