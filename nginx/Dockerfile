# nginx/Dockerfile

# Use the official Nginx base image
FROM nginx:alpine

# Remove the default Nginx configuration file
# This is important to ensure our custom config is used
RUN rm /etc/nginx/conf.d/default.conf

# Copy your custom Nginx configuration file into the container
# It will be placed in the directory Nginx looks for configuration snippets
COPY nginx.conf /etc/nginx/conf.d/

# Expose port 80. Docker Compose will map this to localhost:8080.
EXPOSE 80

# Nginx container starts with the default Nginx command, which keeps it running
# CMD ["nginx", "-g", "daemon off;"] # This is usually the default command, so not strictly needed

