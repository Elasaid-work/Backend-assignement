# Use the official Nginx image as base
FROM nginx:alpine

# Remove the default configuration
RUN rm /etc/nginx/conf.d/default.conf

# Copy your custom Nginx config into the container
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Expose port 80 (can be mapped to 8080 on host)
EXPOSE 80
