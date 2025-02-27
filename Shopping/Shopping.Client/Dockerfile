# 베이스 이미지 설정
FROM mcr.microsoft.com/dotnet/aspnet:5.0-buster-slim AS base
WORKDIR /app
EXPOSE 80

# 빌드 환경 설정
FROM mcr.microsoft.com/dotnet/sdk:5.0-buster-slim AS build
WORKDIR /src

# 의존성 복원 및 캐시 최적화
COPY ["Shopping.Client.csproj", "Shopping.Client/"]
WORKDIR /src/Shopping.Client
RUN dotnet restore "Shopping.Client.csproj" --disable-parallel --verbosity detailed

# 소스 코드 전체 복사
COPY . .

# 빌드
RUN dotnet build "Shopping.Client.csproj" -c Release -o /app/build --no-incremental --verbosity detailed

# 퍼블리시 (출시용 빌드)
FROM build AS publish
RUN dotnet publish "Shopping.Client.csproj" -c Release -o /app/publish --no-restore

# 최종 이미지 설정
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .

# 애플리케이션 시작
ENTRYPOINT ["dotnet", "Shopping.Client.dll"]
