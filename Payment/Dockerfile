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


# Stage 1: Ultra-minimal build (no build-essential)
FROM python:3.9-slim AS builder
WORKDIR /build

# Pre-clean + minimal deps only + immediate cleanup
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        gcc \
        libc6-dev \
    && pip install --no-cache-dir --user uwsgi \
    && apt-get purge -y gcc libc6-dev \
    && apt-get autoremove -y \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* \
    && pip install --user --no-cache-dir -r requirements.txt

# Extract just the installed packages
RUN mkdir /install \
    && cp -r /root/.local/* /install/ \
    && rm -rf /root/.local

# Stage 2: Final runtime image
FROM python:3.9-slim
EXPOSE 8080
WORKDIR /opt/server

# Minimal runtime only (no dev packages)
RUN apt-get update && apt-get install -y --no-install-recommends \
        && rm -rf /var/lib/apt/lists/* \
    && apt-get clean

# Non-root user
RUN groupadd -r roboshopgroup && useradd -r -g roboshopgroup roboshop

USER roboshop
COPY --chown=roboshop:roboshopgroup --from=builder /install /usr/local/lib/python3.9/site-packages
ENV PATH="/usr/local/lib/python3.9/site-packages/bin:${PATH}"

COPY --chown=roboshop:roboshopgroup payment.ini .
COPY --chown=roboshop:roboshopgroup *.py .

CMD ["uwsgi", "--ini", "payment.ini"]


