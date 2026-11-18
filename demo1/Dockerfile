# ===== STAGE 1: BUILD =====
FROM gradle:8.5-jdk17 AS builder

WORKDIR /app

# Copy file cấu hình trước để tận dụng cache
COPY build.gradle .
COPY settings.gradle .

# Tải dependencies (tách riêng để cache tốt hơn)
RUN gradle dependencies --no-daemon

# Copy toàn mã nguồn
COPY src ./src

# Build ứng dụng
RUN gradle build -x test --no-daemon

# ===== STAGE 2: RUNTIME =====
FROM eclipse-temurin:17-jre-alpine

WORKDIR /app

# Copy file .jar từ stage builder
COPY --from=builder /app/build/libs/*.jar app.jar

# Cổng ứng dụng
EXPOSE 8080

# Lệnh khởi động
ENTRYPOINT ["java", "-jar", "app.jar"]
