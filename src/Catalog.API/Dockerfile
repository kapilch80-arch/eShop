FROM mcr.microsoft.com/dotnet/sdk:10.0 AS build
WORKDIR /src

COPY global.json nuget.config Directory.Build.props Directory.Build.targets Directory.Packages.props ./
COPY src/Catalog.API/Catalog.API.csproj src/Catalog.API/
COPY src/EventBus/EventBus.csproj src/EventBus/
COPY src/EventBusRabbitMQ/EventBusRabbitMQ.csproj src/EventBusRabbitMQ/
COPY src/IntegrationEventLogEF/IntegrationEventLogEF.csproj src/IntegrationEventLogEF/
COPY src/eShop.ServiceDefaults/eShop.ServiceDefaults.csproj src/eShop.ServiceDefaults/
RUN dotnet restore src/Catalog.API/Catalog.API.csproj

COPY src/Catalog.API/ src/Catalog.API/
COPY src/EventBus/ src/EventBus/
COPY src/EventBusRabbitMQ/ src/EventBusRabbitMQ/
COPY src/IntegrationEventLogEF/ src/IntegrationEventLogEF/
COPY src/eShop.ServiceDefaults/ src/eShop.ServiceDefaults/
COPY src/Shared/ src/Shared/

RUN dotnet publish src/Catalog.API/Catalog.API.csproj -c Release --no-restore -o /app/publish

FROM mcr.microsoft.com/dotnet/aspnet:10.0 AS final
WORKDIR /app
COPY --from=build /app/publish .

ENV ASPNETCORE_HTTP_PORTS=8080
EXPOSE 8080

ENTRYPOINT ["dotnet", "Catalog.API.dll"]
