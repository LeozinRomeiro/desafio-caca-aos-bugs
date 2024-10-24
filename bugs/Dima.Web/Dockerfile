# Etapa 1: Build
FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /src

# Copiar os arquivos de projeto e restaurar as dependências
COPY ["Dima.Web/Dima.Web.csproj", "Dima.Web/"]

RUN dotnet restore "Dima.Web/Dima.Web.csproj"

# Copiar todos os arquivos e compilar o projeto
COPY . .
WORKDIR "/Dima.Web"
RUN dotnet publish -c Release -o /app/publish

# Etapa 2: Servir com NGINX
FROM nginx:alpine
WORKDIR /usr/share/nginx/html

# Remover arquivos padrão do NGINX
RUN rm -rf ./*

# Copiar os arquivos do build para o diretório NGINX
COPY --from=build /app/publish/wwwroot .

# Expor a porta 80 para o servidor NGINX
EXPOSE 80

# Iniciar o NGINX
CMD ["nginx", "-g", "daemon off;"]
