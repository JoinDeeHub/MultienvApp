# backend/prod/Dockerfile
FROM python:3.11-slim

# system deps (add build-essential if any packages need building)
RUN apt-get update && apt-get install -y --no-install-recommends \
    ca-certificates \
    && rm -rf /var/lib/apt/lists/*

# Create app user (non-root)
RUN useradd --create-home --shell /bin/bash appuser
WORKDIR /app

# Copy only requirements first for layer caching
COPY requirements.txt .

# Install python deps
RUN pip install --no-cache-dir -r requirements.txt

# Copy application code
COPY --chown=appuser:appuser . .

# Use non-root user
USER appuser

# Ensure logs appear immediately
ENV PYTHONUNBUFFERED=1
ENV FLASK_APP=app.py
ENV FLASK_ENV=production

EXPOSE 3002

# Use flask run for dev (ok). In prod use gunicorn.
CMD ["gunicorn", "-w", "3", "-b", "0.0.0.0:3002", "app:app"]