FROM mcr.microsoft.com/dotnet/core/sdk:3.1.100 AS sdk
# To use the debug build configuration pass --build-arg Configuration=Debug
ARG Configuration=Release
ENV DOTNET_SKIP_FIRST_TIME_EXPERIENCE=true \
    DOTNET_CLI_TELEMETRY_OPTOUT=true
WORKDIR /src
COPY "ApiTemplate.sln" "."
COPY "Source/ApiTemplate/*.csproj" "Source/ApiTemplate/"
COPY "Tests/ApiTemplate.IntegrationTest/*.csproj" "Tests/ApiTemplate.IntegrationTest/"
RUN dotnet restore
COPY . .
RUN dotnet build --configuration $Configuration --no-restore
RUN dotnet test --configuration $Configuration --no-build
RUN dotnet publish "Source/ApiTemplate/ApiTemplate.csproj" --configuration $Configuration --no-build --output /app

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1.0-alpine AS runtime
# Open Container Initiative (OCI) labels (See https://github.com/opencontainers/image-spec/blob/master/annotations.md).
LABEL org.opencontainers.image.title="PROJECT-TITLE" \
      org.opencontainers.image.description="PROJECT-DESCRIPTION" \
      org.opencontainers.image.vendor="PROJECT-AUTHOR"
WORKDIR /app
EXPOSE 80 443
COPY --from=sdk /app .
ENTRYPOINT ["dotnet", "ApiTemplate.dll"]

# Build
# docker buildx build --progress plain --file "Source\ApiTemplate\Dockerfile" --tag apitemplate .
