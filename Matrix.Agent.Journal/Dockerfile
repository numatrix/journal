# prepare final image
FROM mcr.microsoft.com/dotnet/core/aspnet:2.2 AS base
WORKDIR /app
EXPOSE 80

# prepare build image
FROM mcr.microsoft.com/dotnet/core/sdk:2.2 AS build
WORKDIR /src

COPY Matrix.Agent.Journal/Matrix.Agent.Journal.csproj Matrix.Agent.Journal/
COPY Matrix.Agent.Journal.Business/Matrix.Agent.Journal.Business.csproj Matrix.Agent.Journal.Business/
COPY Matrix.Agent.Journal.Database/Matrix.Agent.Journal.Database.csproj Matrix.Agent.Journal.Database/
COPY Matrix.Agent.Journal.Model/Matrix.Agent.Journal.Model.csproj Matrix.Agent.Journal.Model/
COPY . .

# restore
RUN dotnet restore "Matrix.Agent.Journal/Matrix.Agent.Journal.csproj" -s http://nuget.matrix.paramg.com:8888/v3/index.json

# build
WORKDIR /src/Matrix.Agent.Journal
RUN dotnet build "Matrix.Agent.Journal.csproj" -c Release -o /app

# publish
FROM build AS publish
RUN dotnet publish "Matrix.Agent.Journal.csproj" -c Release -o /app

# finalize
FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "Matrix.Agent.Journal.dll"]