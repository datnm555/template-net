FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["src/TemplateNet.Api/TemplateNet.Api.csproj", "src/TemplateNet.Api/"]
RUN dotnet restore "src/TemplateNet.Api/TemplateNet.Api.csproj"
COPY . .
WORKDIR "/src/src/TemplateNet.Api"
RUN dotnet build "TemplateNet.Api.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "TemplateNet.Api.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "TemplateNet.Api.dll"]
