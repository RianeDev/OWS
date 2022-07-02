#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /src
COPY ["CharPublicAPI/OWSPublicAPI.csproj", "CharPublicAPI/"]
COPY ["OWSShared/OWSShared.csproj", "OWSShared/"]
COPY ["OWSData/OWSData.csproj", "OWSData/"]
RUN dotnet restore "CharPublicAPI/CharPublicAPI.csproj"
COPY . .
WORKDIR "/src/CharPublicAPI"
RUN dotnet build "CharPublicAPI.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "CharPublicAPI.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "CharPublicAPI.dll"]
