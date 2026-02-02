
FROM mcr.microsoft.com/dotnet/sdk:7.0 AS builder

WORKDIR /app

# Copy solution + source
COPY cartservice.sln .
COPY src ./src

# Restore using correct path
RUN dotnet restore src/cartservice.csproj -r linux-musl-x64

# Publish
RUN dotnet publish src/cartservice.csproj \
    -p:PublishSingleFile=true \
    -r linux-musl-x64 \
    --self-contained true \
    -c Release \
    -o /cartservice

FROM mcr.microsoft.com/dotnet/runtime-deps:7.0-alpine

WORKDIR /app

COPY --from=builder /cartservice .

EXPOSE 7070

ENV ASPNETCORE_URLS=http://*:7070

ENTRYPOINT ["/app/cartservice"]
# rebuild trigger
