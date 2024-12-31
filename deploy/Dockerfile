# Base image
FROM node:18

# Set the working directory
WORKDIR /app

# Copy the application code
COPY backend ./backend
COPY frontend ./frontend
COPY deploy ./deploy

# Ensure /app/data directory exists and set proper permissions
RUN mkdir -p /app/data && chmod -R 775 /app

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
