#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["BlazorDemo/Server/BlazorDemo.Server.csproj", "BlazorDemo/Server/"]
COPY ["BlazorDemo/Client/BlazorDemo.Client.csproj", "BlazorDemo/Client/"]
COPY ["BlazorDemo/Shared/BlazorDemo.Shared.csproj", "BlazorDemo/Shared/"]
RUN dotnet restore "BlazorDemo/Server/BlazorDemo.Server.csproj"
COPY . .
WORKDIR "/src/BlazorDemo/Server"
RUN dotnet build "BlazorDemo.Server.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "BlazorDemo.Server.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "BlazorDemo.Server.dll"]