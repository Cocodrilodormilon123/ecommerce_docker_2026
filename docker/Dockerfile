FROM eclipse-temurin:17-jdk-jammy

# 1. Configuración del directorio de trabajo
ENV APP_HOME=/ecommerce-app
RUN mkdir -p $APP_HOME
WORKDIR $APP_HOME

# 2. Instalación de dependencias necesarias (para el HEALTHCHECK)
RUN apt-get update && apt-get install -y curl && rm -rf /var/lib/apt/lists/*

# 3. Puerto a exponer
EXPOSE 8084

# 4. Copiar el JAR generado por Maven
COPY target/ecommerce-*.jar app.jar

# 5. Configuración de seguridad (Usuario No-Root)
RUN useradd -m appuser && chown -R appuser:appuser $APP_HOME
USER appuser

# 6. Monitoreo de salud interno
HEALTHCHECK --interval=30s --timeout=3s --start-period=40s \
    CMD curl -f http://localhost:8084/actuator/health || exit 1

# 7. Comando de arranque
ENTRYPOINT ["java", "-jar", "app.jar"]