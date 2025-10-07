# Base image
FROM nginx:latest

# Copy static content or app files into the default NGINX directory
COPY ./index.html /usr/share/nginx/html/index.html

# Expose port 80
EXPOSE 80

# No need for CMD — the nginx base image already defines it
