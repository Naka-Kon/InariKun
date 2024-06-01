FROM mcr.microsoft.com/dotnet/runtime:8.0 AS base
USER $APP_UID
WORKDIR /app

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["InariKun.Runtime/InariKun.Runtime.csproj", "InariKun.Runtime/"]
RUN dotnet restore "InariKun.Runtime/InariKun.Runtime.csproj"
COPY . .
WORKDIR "/src/InariKun.Runtime"
RUN dotnet build "InariKun.Runtime.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "InariKun.Runtime.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "InariKun.Runtime.dll"]
