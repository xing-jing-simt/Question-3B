FROM mcr.microsoft.com/dotnet/sdk:6.0.200 AS base
WORKDIR /app
EXPOSE 5000
ENV RABBITMQ_HOST localhost
ENV RABBITMQ_PORT 5672 
ENV ASPNETCORE_URLS=http://+:5000

FROM mcr.microsoft.com/dotnet/sdk:6.0.200 AS build
WORKDIR /src
COPY ["rabbitconsumer.csproj", "."]
RUN dotnet restore "./rabbitconsumer.csproj"
COPY . .
WORKDIR "/src/."
RUN dotnet build "rabbitconsumer.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "rabbitconsumer.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "rabbitconsumer.dll"]
