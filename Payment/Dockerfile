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

# Install runtime deps only
RUN apt-get update && apt-get install -y --no-install-recommends \
    libpcre2-8-0 && \
    rm -rf /var/lib/apt/lists/* && \
    apt-get clean

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Create non-root user WITH FIXED numeric IDs
RUN groupadd -r -g 1001 roboshopgroup && \
    useradd -r -u 1001 -g roboshopgroup roboshop

# Copy files as non-root user
COPY --chown=1001:1001 payment.ini *.py ./

# Switch to non-root BEFORE runtime
USER 1001:1001
EXPOSE 8080
CMD ["uwsgi", "--ini", "payment.ini"]




