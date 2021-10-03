FROM azure-functions:dotnet-4-arm64 AS base
WORKDIR /home/site/wwwroot
EXPOSE 80

FROM mcr.microsoft.com/dotnet/sdk:6.0-bullseye-slim AS build

WORKDIR /src
COPY ["Function/Function.csproj", "Function/"]
RUN dotnet restore "Function/Function.csproj"
COPY . .
WORKDIR "/src/Function"
RUN dotnet build "Function.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Function.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /home/site/wwwroot
COPY --from=publish /app/publish .
ENV AzureWebJobsScriptRoot=/home/site/wwwroot \
    AzureFunctionsJobHost__Logging__Console__IsEnabled=true