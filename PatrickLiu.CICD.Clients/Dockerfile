#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:5.0-buster-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:5.0-buster-slim AS build
WORKDIR /src
COPY ["PatrickLiu.CICD.Clients/PatrickLiu.CICD.Clients.csproj", "PatrickLiu.CICD.Clients/"]
COPY ["PatrickLiu.CICD.Services/PatrickLiu.CICD.Services.csproj", "PatrickLiu.CICD.Services/"]
COPY ["PatrickLiu.CICD.Interfaces/PatrickLiu.CICD.Interfaces.csproj", "PatrickLiu.CICD.Interfaces/"]
COPY ["PatrickLiu.CICD.Models/PatrickLiu.CICD.Models.csproj", "PatrickLiu.CICD.Models/"]
RUN dotnet restore "PatrickLiu.CICD.Clients/PatrickLiu.CICD.Clients.csproj"
COPY . .
WORKDIR "/src/PatrickLiu.CICD.Clients"
RUN dotnet build "PatrickLiu.CICD.Clients.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "PatrickLiu.CICD.Clients.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "PatrickLiu.CICD.Clients.dll"]