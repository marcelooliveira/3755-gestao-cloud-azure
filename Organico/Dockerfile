#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /src
COPY ["Organico/Organico.csproj", "Organico/"]
COPY ["Organico.Library/Organico.Library.csproj", "Organico.Library/"]
RUN dotnet restore "Organico/Organico.csproj"
COPY . .
WORKDIR "/src/Organico"
RUN dotnet build "Organico.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Organico.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Organico.dll"]