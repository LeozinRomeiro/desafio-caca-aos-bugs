FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER app
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /
COPY ["Dima.Api/Dima.Api.csproj", "Dima.Api/"]
COPY ["Dima.Core/Dima.Core.csproj", "Dima.Core/"]
RUN dotnet restore "./Dima.Api/Dima.Api.csproj"
COPY . .
WORKDIR "Dima.Api"
RUN dotnet build "./Dima.Api.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./Dima.Api.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Dima.Api.dll"]