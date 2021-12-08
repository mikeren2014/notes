# [Console App](https://docs.microsoft.com/en-us/dotnet/core/docker/build-container?tabs=windows)

## Create image

        docker build -t counter-image -f Dockerfile .

## Create a container

        docker create --name core-counter counter-image

## Manager the container

        docker start core-counter

        docker stop core-counter

## Connect to a container

        docker attach --sig-proxy=false core-counter
        **--sig-proxy=false** parameter ensures that Ctrl+C will not stop the process in the container

        docker exec ??

## Delete a container

        docker rm core-counter

## Single run

        docker run -it -rm counter-image
        docker run -it -rm counter-image 3 //[pass in the parameter]

## Change the ENTRYPOINT

        docker run -it --rm --entrypoint "bash" counter-image


## Remvoe image

        docker rmi counter-image:latest

# [Asp.net Core](https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/docker/building-net-docker-images?view=aspnetcore-6.0)

## Build image


        docker build --pull -t mywebapp .

## Run

    docker run -it --rm -p 5000:80 --name aspnetcore_sample aspnetapp

    docker exec aspnetcore_sample ipconfig // Get ipaddress from Windows container

## [HTTPS](https://github.com/dotnet/dotnet-docker/blob/main/samples/run-aspnetcore-https-development.md)

### Certification

    dotnet dev-certs https -ep $env:USERPROFILE\.aspnet\https\aspnetapp.pfx -p crypticpassword
        dotnet dev-certs https --trust

  - __-ep__ Export
  - __$env:USERPROFILE__ User profilePath. Only works in Powershell, in command prompt, use __%USERPROFILE%__
  - __-p__ Password

### User Secrets

    dotnet user-secrets -p aspnetapp\aspnetapp.csproj set "Kestrel:Certificates:Development:Password" "crypticpassword"

### Run the container

    docker run --rm -it -p 8010:80 -p 8011:443 -e ASPNETCORE_URLS="https://+;http://+" -e ASPNETCORE_HTTPS_PORT=8001 -e ASPNETCORE_ENVIRONMENT=Development -v $env:APPDATA\microsoft\UserSecrets\:/root/.microsoft/usersecrets -v $env:USERPROFILE\.aspnet\https:/root/.aspnet/https/ mywebapp

  For production

    docker run --rm -it -p 8000:80 -p 8001:443 -e ASPNETCORE_URLS="https://+;http://+" -e ASPNETCORE_HTTPS_PORT=8001 -e ASPNETCORE_Kestrel__Certificates__Default__Password="crypticpassword" -e ASPNETCORE_Kestrel__Certificates__Default__Path=/https/aspnetapp.pfx -v %USERPROFILE%\.aspnet\https:/https/ mcr.microsoft.com/dotnet/samples:aspnetapp

# Misc

### [Visual Studio Container Tools](https://docs.microsoft.com/en-us/visualstudio/containers/tutorial-multicontainer?view=vs-2022)

# Kubernetes

## Articals

  - [Asp.net core & Kubernetes](https://andrewlock.net/deploying-asp-net-core-applications-to-kubernetes-part-1-an-introduction-to-kubernetes/)

  - [Mount network folder](https://stackoverflow.com/questions/40049282/mounting-a-network-folder-to-a-docker-container-on-windows-10)

  - [MS Docker Document](https://docs.microsoft.com/en-us/dotnet/core/docker/introduction)