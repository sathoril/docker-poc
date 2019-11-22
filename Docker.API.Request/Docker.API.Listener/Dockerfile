FROM mcr.microsoft.com/dotnet/core/aspnet:3.0-buster-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 8085
EXPOSE 8082

FROM mcr.microsoft.com/dotnet/core/sdk:3.0-buster AS build
WORKDIR /src
COPY ["Docker.API.Listener/Docker.API.Listener.csproj", "Docker.API.Listener/"]
RUN dotnet restore "Docker.API.Listener/Docker.API.Listener.csproj"
COPY . .
WORKDIR "/src/Docker.API.Listener"
RUN dotnet build "Docker.API.Listener.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Docker.API.Listener.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Docker.API.Listener.dll"]