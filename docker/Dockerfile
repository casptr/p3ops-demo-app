
# sdk image
FROM mcr.microsoft.com/dotnet/sdk:6.0 AS builder

WORKDIR /src
COPY src .

RUN ls
# RUN dotnet restore src/Server/Server.csproj
# RUN dotnet publish -c Release -o /publish src/Server/Server.csproj

# app image
FROM mcr.microsoft.com/dotnet/aspnet:6.0

EXPOSE 80
ENTRYPOINT ["dotnet", "Server.dll"]

WORKDIR /app
COPY --from=builder /publish/ .
