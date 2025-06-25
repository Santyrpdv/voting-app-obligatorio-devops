# Etapa de build
FROM --platform=${BUILDPLATFORM} mcr.microsoft.com/dotnet/sdk:7.0 AS build

ARG TARGETPLATFORM
ARG TARGETARCH
ARG BUILDPLATFORM

RUN echo "Building on $BUILDPLATFORM for $TARGETPLATFORM"

WORKDIR /source

# Copiar el archivo .csproj y restaurar dependencias
COPY *.csproj . 
RUN dotnet restore -a $TARGETARCH

# Copiar el resto del código fuente y compilar
COPY . .
RUN dotnet publish -c Release -o /app -a $TARGETARCH --self-contained false --no-restore

# Etapa final: imagen liviana con solo el runtime
FROM mcr.microsoft.com/dotnet/runtime:7.0

WORKDIR /app

# Copiar certificado raíz de Amazon RDS para conexiones seguras
RUN mkdir -p /certs
COPY certs/global-bundle.pem /certs/global-bundle.pem

# Copiar la aplicación compilada desde la etapa de build
COPY --from=build /app .

# Ejecutar el worker
ENTRYPOINT ["dotnet", "Worker.dll"]
