FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /src

COPY ["TodoListWebApi.Application/TodoListWebApi.Application.csproj", "TodoListWebApi.Application/"]
RUN dotnet restore "TodoListWebApi.Application/TodoListWebApi.Application.csproj"

COPY ["TodoListWebApi.Domain/TodoListWebApi.Domain.csproj", "TodoListWebApi.Domain/"]
RUN dotnet restore "TodoListWebApi.Domain/TodoListWebApi.Domain.csproj"

COPY ["TodoListWebApi.Repository/TodoListWebApi.Repository.csproj", "TodoListWebApi.Repository/"]
RUN dotnet restore "TodoListWebApi.Repository/TodoListWebApi.Repository.csproj"

COPY ["TodoListWebApi.WebApi/TodoListWebApi.WebApi.csproj", "TodoListWebApi.WebApi/"]
RUN dotnet restore "TodoListWebApi.WebApi/TodoListWebApi.WebApi.csproj"

COPY . .
WORKDIR "/src/TodoListWebApi.WebApi"
RUN dotnet build "TodoListWebApi.WebApi.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "TodoListWebApi.WebApi.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "TodoListWebApi.WebApi.dll"]