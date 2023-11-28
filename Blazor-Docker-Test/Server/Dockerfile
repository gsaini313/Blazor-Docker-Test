#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["Blazor-Docker-Test/Server/Blazor-Docker-Test.Server.csproj", "Blazor-Docker-Test/Server/"]
COPY ["Blazor-Docker-Test/Client/Blazor-Docker-Test.Client.csproj", "Blazor-Docker-Test/Client/"]
COPY ["Blazor-Docker-Test/Shared/Blazor-Docker-Test.Shared.csproj", "Blazor-Docker-Test/Shared/"]
RUN dotnet restore "./Blazor-Docker-Test/Server/./Blazor-Docker-Test.Server.csproj"
COPY . .
WORKDIR "/src/Blazor-Docker-Test/Server"
RUN dotnet build "./Blazor-Docker-Test.Server.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./Blazor-Docker-Test.Server.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Blazor-Docker-Test.Server.dll"]