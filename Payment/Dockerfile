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


# Stage 1: Build - CORRECT ORDER
FROM python:3.9-slim AS builder
WORKDIR /build

# Copy requirements FIRST
COPY requirements.txt .

# Minimal build deps + IMMEDIATE cleanup
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        gcc \
        libc6-dev \
    && pip install --no-cache-dir --user -r requirements.txt \
    && apt-get purge -y gcc libc6-dev \
    && apt-get autoremove -y \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /root/.cache

# Package up installed Python modules
RUN mkdir -p /install \
    && cp -r /root/.local /install/ 2>/dev/null || true \
    && find /usr/local -name '*.so' -exec cp {} /install/ \; 2>/dev/null || true

# Stage 2: Final image
FROM python:3.9-slim
EXPOSE 8080
WORKDIR /opt/server

# Ultra-minimal - no extra runtime deps needed
RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Create non-root user
RUN groupadd -r roboshopgroup && useradd -r -g roboshopgroup roboshop

USER roboshop
# Copy Python packages
COPY --chown=roboshop:roboshopgroup --from=builder /install /tmp/install
RUN cp -r /tmp/install/* /usr/local/ 2>/dev/null || true && rm -rf /tmp/install

# Copy app
COPY --chown=roboshop:roboshopgroup payment.ini .
COPY --chown=roboshop:roboshopgroup *.py .

CMD ["uwsgi", "--ini", "payment.ini"]



