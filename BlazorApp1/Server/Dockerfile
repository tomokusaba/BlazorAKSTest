#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["BlazorCoreHosted/Server/BlazorCoreHosted.Server.csproj", "BlazorCoreHosted/Server/"]
COPY ["BlazorCoreHosted/Client/BlazorCoreHosted.Client.csproj", "BlazorCoreHosted/Client/"]
COPY ["BlazorCoreHosted/Shared/BlazorCoreHosted.Shared.csproj", "BlazorCoreHosted/Shared/"]
RUN dotnet restore "BlazorCoreHosted/Server/BlazorCoreHosted.Server.csproj"
COPY . .
WORKDIR "/src/BlazorCoreHosted/Server"
RUN dotnet build "BlazorCoreHosted.Server.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "BlazorCoreHosted.Server.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "BlazorCoreHosted.Server.dll"]
