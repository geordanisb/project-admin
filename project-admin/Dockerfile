#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /src
COPY ["project-admin/project-admin.csproj", "project-admin/"]
COPY ["core/core/core.csproj", "core/core/"]
RUN dotnet restore "project-admin/project-admin.csproj"
COPY . .
WORKDIR "/src/project-admin"
RUN dotnet build "project-admin.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "project-admin.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "project-admin.dll"]
