FROM mcr.microsoft.com/dotnet/core/aspnet:3.0 AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/core/sdk:3.0 AS build
WORKDIR /src
COPY ["BlazingComponents/BlazingComponents.csproj", "BlazingComponents/"]
COPY ["BlazingPizza.Client/BlazingPizza.Client.csproj", "BlazingPizza.Client/"]
COPY ["BlazingPizza.ComponentsLibrary/BlazingPizza.ComponentsLibrary.csproj", "BlazingPizza.ComponentsLibrary/"]
COPY ["BlazingPizza.Server/BlazingPizza.Server.csproj", "BlazingPizza.Server/"]
COPY ["BlazingPizza.Shared/BlazingPizza.Shared.csproj", "BlazingPizza.Shared/"]

RUN dotnet restore "BlazingPizza.Server/BlazingPizza.Server.csproj"
COPY . .
WORKDIR "/src/BlazingPizza.Server"
RUN dotnet build "BlazingPizza.Server.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "BlazingPizza.Server.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "BlazingPizza.Server.dll"]