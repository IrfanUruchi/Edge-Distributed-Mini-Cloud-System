# llm-proxy-docker/Dockerfile
FROM nginx:alpine

# Overwrite Nginx’s default config with our proxy config
COPY nginx.conf /etc/nginx/nginx.conf

# Expose port 1234 (same port LM Studio uses)
EXPOSE 1234

# Run Nginx in foreground
CMD ["nginx", "-g", "daemon off;"]
