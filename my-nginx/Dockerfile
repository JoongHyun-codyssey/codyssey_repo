FROM nginx:alpine

LABEL org.opencontainers.image.title="custom-nginx-practice"
LABEL org.opencontainers.image.description="Dockerfile practice with nginx alpine"

ENV APP_ENV=dev

COPY index.html /usr/share/nginx/html/index.html

EXPOSE 80