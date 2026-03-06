# FROM python:3.9
# EXPOSE 8080
# WORKDIR /opt/server
# COPY requirements.txt .
# COPY *.py .
# COPY payment.ini .
# RUN pip3 install -r requirements.txt
# CMD ["uwsgi", "--ini", "payment.ini"]



# # Stage 1: Build
# FROM python:3.9.23-alpine3.22 AS builder
# WORKDIR /build
# # Install build dependencies
# RUN apk add --no-cache python3-dev build-base linux-headers pcre-dev
# COPY requirements.txt .
# RUN pip3 install --no-cache-dir --prefix=/install -r requirements.txt


# # Stage 2: Final image
# FROM python:3.9.23-alpine3.22
# EXPOSE 8080
# WORKDIR /opt/server
# # Runtime dependencies only
# RUN apk add --no-cache pcre
# # Create app user
# RUN addgroup -S roboshopgroup && adduser -S roboshop -G roboshopgroup
# USER roboshop
# # Copy installed Python packages from builder
# COPY --from=builder /install /usr/local
# # Copy application code
# COPY --chown=roboshop:roboshopgroup payment.ini .
# COPY --chown=roboshop:roboshopgroup *.py .
# CMD ["uwsgi", "--ini", "payment.ini"]


FROM python:3.9-slim
WORKDIR /opt/server

# 1. Copy requirements FIRST
COPY requirements.txt .

# 2. Install FULL build environment
RUN apt-get update && apt-get install -y \
    build-essential \
    python3-dev \
    gcc \
    g++ \
    libc6-dev \
    libpcre2-dev \
    pkg-config

# 3. Install Python packages
RUN pip install --no-cache-dir -r requirements.txt

# 4. Clean build deps
RUN apt-get purge -y --auto-remove \
    build-essential python3-dev gcc g++ libc6-dev libpcre2-dev pkg-config && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /root/.cache

# 5. Create non-root user
RUN groupadd -r -g 1001 roboshopgroup && \
    useradd -r --uid 1001 -g roboshopgroup roboshop

# 6. Switch to root for permissions
USER root
RUN chown -R 1001:1001 /opt/server

# 7. Copy app files
COPY --chown=1001:1001 payment.ini *.py ./

# 8. Run as non-root
USER 1001:1001
EXPOSE 8080

# 9. DIRECT CMD - NO INI FILE!
CMD ["uwsgi", "--http", ":8080", "--wsgi-file", "payment.py", "--callable", "app", "--processes", "4", "--master", "--enable-threads"]







