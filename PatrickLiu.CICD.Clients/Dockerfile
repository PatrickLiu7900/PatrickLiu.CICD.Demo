#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM 192.168.127.143/patrickliu.microservices/aspnet-core5:v1 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM  192.168.127.143/patrickliu.microservices/sdk-core5:v1 AS build
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