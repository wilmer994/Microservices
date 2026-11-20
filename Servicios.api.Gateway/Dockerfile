# Consulte https://aka.ms/customizecontainer para aprender a personalizar su contenedor de depuración y cómo Visual Studio usa este Dockerfile para compilar sus imágenes para una depuración más rápida.

# Este segmento se refiere a la descarga del Runtime de .NET Core 5
FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS base
WORKDIR /app
EXPOSE 80


# Este segmento se refiere a la descarga del SDK de .NET Core 5 e incluye la directiva que señala la ubicacion al interior del contain en donde se almacenaran
# los archivos del código fuente del microservicio.
FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["Servicios.api.Gateway/Servicios.api.Gateway.csproj", "Servicios.api.Gateway/"]
RUN dotnet restore "./Servicios.api.Gateway/Servicios.api.Gateway.csproj"
COPY . .
WORKDIR "/src/Servicios.api.Gateway"
RUN dotnet build "./Servicios.api.Gateway.csproj" -c $BUILD_CONFIGURATION -o /app/build

# Este segmento se refiere al proceso de compilación y publicación del microservicio al interior del container
FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./Servicios.api.Gateway.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Servicios.api.Gateway.dll"]