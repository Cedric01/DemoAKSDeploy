FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

ENV ASPNETCORE_URLS=http://+:80

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["CedricDemo-Deploy-AKS/CedricDemo-Deploy-AKS.csproj", "CedricDemo-Deploy-AKS/"]
RUN dotnet restore "CedricDemo-Deploy-AKS/CedricDemo-Deploy-AKS.csproj"
COPY . .
WORKDIR "/src/CedricDemo-Deploy-AKS"
RUN dotnet build "CedricDemo-Deploy-AKS.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "CedricDemo-Deploy-AKS.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "CedricDemo-Deploy-AKS.dll"]
