#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["EnterpriseApplication/Server/EnterpriseApplication.Server.csproj", "EnterpriseApplication/Server/"]
COPY ["EnterpriseApplication/Client/EnterpriseApplication.Client.csproj", "EnterpriseApplication/Client/"]
COPY ["EnterpriseApplication/Shared/EnterpriseApplication.Shared.csproj", "EnterpriseApplication/Shared/"]
RUN dotnet restore "EnterpriseApplication/Server/EnterpriseApplication.Server.csproj"
COPY . .
WORKDIR "/src/EnterpriseApplication/Server"
RUN dotnet build "EnterpriseApplication.Server.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "EnterpriseApplication.Server.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "EnterpriseApplication.Server.dll"]