FROM mcr.microsoft.com/dotnet/sdk:7.0.400@sha256:926b9337622ccc594ed051d3559f1c4544686c9fd5f4656f09d1fe37d9a5ace2 as builder
WORKDIR /app
COPY cartservice.csproj .
RUN dotnet restore cartservice.csproj -r linux-musl-x64
COPY . .
RUN dotnet publish cartservice.csproj -p:PublishSingleFile=true -r linux-musl-x64 --self-contained true -p:PublishTrimmed=True -p:TrimMode=Link -c release -o /cartservice --no-restore

# https://mcr.microsoft.com/v2/dotnet/runtime-deps/tags/list
FROM mcr.microsoft.com/dotnet/runtime-deps:7.0.10-alpine3.18-amd64@sha256:0a0082af70195406fab26bcdbc9313e27a5869082bcf7d16d84e1b2c0eb1b624

WORKDIR /app
COPY --from=builder /cartservice .
EXPOSE 7070
ENV DOTNET_EnableDiagnostics=0 \
    ASPNETCORE_URLS=http://*:7070
USER 1000
ENTRYPOINT ["/app/cartservice"]
