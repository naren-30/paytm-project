# Use official nginx base image
FROM nginx:alpine

# Copy static site files into nginx html directory
COPY . /usr/share/nginx/html

# Expose port 80
EXPOSE 80

# Run nginx in foreground
CMD ["nginx", "-g", "daemon off;"]

