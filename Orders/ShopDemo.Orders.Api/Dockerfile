FROM mcr.microsoft.com/dotnet/aspnet:10.0 AS base
WORKDIR /app
EXPOSE 8080
ENV ASPNETCORE_URLS=http://+:8080

FROM mcr.microsoft.com/dotnet/sdk:10.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src

# 🌟 CORRECCIÓN: El segundo parámetro (destino) ahora incluye "Orders/"
COPY ["Orders/ShopDemo.Orders.Api/ShopDemo.Orders.Api.csproj", "Orders/ShopDemo.Orders.Api/"]
COPY ["Orders/ShopDemo.Orders.Application/ShopDemo.Orders.Application.csproj", "Orders/ShopDemo.Orders.Application/"]
COPY ["Orders/ShopDemo.Orders.Infraestructure/ShopDemo.Orders.Infraestructure.csproj", "Orders/ShopDemo.Orders.Infraestructure/"]
COPY ["Orders/ShopDemo.Orders.Domain/ShopDemo.Orders.Domain.csproj", "Orders/ShopDemo.Orders.Domain/"]

# Este se queda igual en la raíz de /src
COPY ["ShopDemo.Shared/ShopDemo.Shared.csproj", "ShopDemo.Shared/"]

# 🌟 Ahora sí, la ruta coincide con el destino estructurado arriba
RUN dotnet restore "Orders/ShopDemo.Orders.Api/ShopDemo.Orders.Api.csproj"

COPY . .
WORKDIR "/src/Orders/ShopDemo.Orders.Api"
RUN dotnet publish -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false --no-restore

FROM base AS final
WORKDIR /app
COPY --from=build /app/publish .
USER app
ENTRYPOINT ["dotnet", "ShopDemo.Orders.Api.dll"]
