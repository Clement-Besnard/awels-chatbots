# Base image
FROM node:18

# Define a non-root user with root group (gid 0)
RUN groupadd -r appgroup && useradd -r -g appgroup -d /app -s /sbin/nologin appuser

# Set the working directory
WORKDIR /app

# Copy the application code
COPY backend ./backend
COPY frontend ./frontend
COPY deploy ./deploy

# Fix permissions for OpenShift's random user uid with gid 0
RUN chown -R appuser:0 /app && chmod -R g+rw /app

# Switch to the non-root user
USER appuser

# Install dependencies and build the app
RUN cd frontend && \
    npm install -D tailwindcss postcss autoprefixer && \
    npx tailwindcss init && \
    npm run build
RUN cd backend && \
    ln -s ../frontend/ frontend && \
    npm install --production

# Expose the port
EXPOSE 3001

# Start the app
CMD ["sh", "-c", "node backend/server.js"]
