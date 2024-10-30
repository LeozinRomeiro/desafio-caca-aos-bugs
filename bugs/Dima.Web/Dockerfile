# Etapa 1: Build do Blazor WebAssembly
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build-web
WORKDIR /

# Copiar arquivos do projeto WebAssembly e restaurar as dependências
COPY ["Dima.Web/Dima.Web.csproj", "Dima.Web/"]
COPY ["Dima.Core/Dima.Core.csproj", "Dima.Core/"]
RUN dotnet restore "./Dima.Web/Dima.Web.csproj"

# Copiar todos os arquivos do projeto WebAssembly
COPY . .
WORKDIR "Dima.Web"

# Compilar e publicar o projeto WebAssembly
RUN dotnet publish -c Release -o /app/publish

# Etapa 2: Servir o Blazor WebAssembly com NGINX
FROM nginx:alpine AS serve-web
WORKDIR /usr/share/nginx/html

# Remover arquivos padrão do NGINX
RUN rm -rf ./*

# Copiar os arquivos do build para o diretório NGINX
COPY --from=build-web /app/publish/wwwroot .

# Expor a porta 80 para o servidor NGINX
EXPOSE 80

# Iniciar o NGINX
CMD ["nginx", "-g", "daemon off;"]
