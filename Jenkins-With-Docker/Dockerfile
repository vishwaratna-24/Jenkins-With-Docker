#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:3.1 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:3.1 AS build
WORKDIR /src
COPY ["Jenkins-With-Docker/Jenkins-With-Docker.csproj", "Jenkins-With-Docker/"]
RUN dotnet restore "Jenkins-With-Docker/Jenkins-With-Docker.csproj"
COPY . .
WORKDIR "/src/Jenkins-With-Docker"
RUN dotnet build "Jenkins-With-Docker.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Jenkins-With-Docker.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Jenkins-With-Docker.dll"]