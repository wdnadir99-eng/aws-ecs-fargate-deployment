# Use a lightweight web server image
FROM nginx:alpine

# Copy our custom HTML file to the NGINX web root
COPY index.html /usr/share/nginx/html/index.html

# The NGINX image automatically starts the server on port 80
# Expose port 80
EXPOSE 80
