# Stage 1: Build the React app
FROM node:18-alpine AS builder

WORKDIR /app

# Copy package.json and lockfile
COPY package*.json ./

# Install dependencies
RUN npm ci

# Copy the rest of the application code
COPY . .

# Build the app for production
RUN npm run build

# Stage 2: Serve the app with Node.js (using serve)
FROM node:18-alpine

WORKDIR /app

# Copy only the built app from the builder stage
COPY --from=builder /app/dist ./dist

# Copy package.json (for the serve dependency)
COPY package*.json ./

# Install only production dependencies (including serve)
RUN npm ci --omit=dev
RUN npm install serve -g

# Expose the port your app runs on (usually 3000, 5000, or 8080)
EXPOSE 8080

# Start the server
CMD ["serve", "dist", "-p", "8080"]