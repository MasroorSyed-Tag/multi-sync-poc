#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/runtime:6.0 AS base

WORKDIR /app

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build

WORKDIR /src

RUN curl -sL https://deb.nodesource.com/setup_16.x | bash - \
  && apt update \
  && apt install -y nodejs

RUN npm i @nx-dotnet/core

COPY ["apps/seeder/MultiSyncPoc.Seeder.csproj", "apps/seeder/"]
RUN dotnet restore "apps/seeder/MultiSyncPoc.Seeder.csproj"
COPY . .
WORKDIR "/src/apps/seeder"
RUN dotnet build "MultiSyncPoc.Seeder.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "MultiSyncPoc.Seeder.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "MultiSyncPoc.Seeder.dll"]
