# Build stage
FROM python:3.12.2-slim-bullseye as builder

WORKDIR /usr/src/djangobnb_backend

ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1

RUN pip install --upgrade pip
COPY ./requirements.txt .
RUN pip install -r requirements.txt

# Production stage
FROM python:3.12.2-slim-bullseye

WORKDIR /usr/src/djangobnb_backend

ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1

RUN apt-get update && apt-get install -y netcat
RUN pip install --upgrade pip

# Copy only necessary packages from builder stage
COPY --from=builder /usr/local/lib/python3.12/site-packages /usr/local/lib/python3.12/site-packages

COPY ./entrypoint.sh .
RUN sed -i 's/\r$//g' /usr/src/djangobnb_backend/entrypoint.sh
RUN chmod +x /usr/src/djangobnb_backend/entrypoint.sh

COPY . .

ENTRYPOINT [ "/usr/src/djangobnb_backend/entrypoint.sh" ]