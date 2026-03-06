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


# Stage 1: Build - Minimal dependencies only
FROM python:3.9-slim AS builder
WORKDIR /build

# Clean apt cache first, install minimal deps, clean immediately
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        gcc \
        g++ \
        python3-dev \
        libpcre2-dev \
    && pip3 install --no-cache-dir --prefix=/install uwsgi \
    && apt-get purge -y --auto-remove gcc g++ python3-dev libpcre2-dev \
    && apt-get autoremove -y \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

COPY requirements.txt .
RUN pip3 install --no-cache-dir --prefix=/install -r requirements.txt

# Stage 2: Final image - Ultra minimal
FROM python:3.9-slim
EXPOSE 8080
WORKDIR /opt/server

# No runtime PCRE needed - uWSGI statically links or uses system libs
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        libpcre2-8-0 \
    && rm -rf /var/lib/apt/lists/* \
    && apt-get clean \
    && rm -rf /tmp/* /var/tmp/*

# Create non-root user
RUN groupadd -r roboshopgroup && useradd -r -g roboshopgroup roboshop

USER roboshop
COPY --from=builder --chown=roboshop:roboshopgroup /install /usr/local
COPY --chown=roboshop:roboshopgroup payment.ini .
COPY --chown=roboshop:roboshopgroup *.py .

CMD ["uwsgi", "--ini", "payment.ini"]

