#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["freepoll/freepoll.csproj", "freepoll/"]
RUN dotnet restore "freepoll/freepoll.csproj"
COPY . .
WORKDIR "/src/freepoll"
RUN dotnet build "freepoll.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "freepoll.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
WORKDIR /publish
CMD ASPNETCORE_URLS=http://*:$port dotnet freepoll.dll