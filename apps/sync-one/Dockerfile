#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/runtime:6.0 AS base

WORKDIR /app

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build

WORKDIR /src

RUN curl -sL https://deb.nodesource.com/setup_16.x | bash - \
  && apt update \
  && apt install -y nodejs

RUN npm i @nx-dotnet/core

COPY ["apps/sync-one/MultiSyncPoc.SyncOne.csproj", "apps/sync-one/"]
RUN dotnet restore "apps/sync-one/MultiSyncPoc.SyncOne.csproj"
COPY . .
WORKDIR "/src/apps/sync-one"
RUN dotnet build "MultiSyncPoc.SyncOne.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "MultiSyncPoc.SyncOne.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "MultiSyncPoc.SyncOne.dll"]
