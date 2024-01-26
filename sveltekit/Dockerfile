# Stage 1: Install dependencies and build the SvelteKit application
FROM node:18-alpine AS build

# Set the working directory
WORKDIR /app

# Copy the package.json and package-lock.json (or yarn.lock)
COPY package*.json ./
# If using yarn with a yarn.lock file, uncomment the next line
# COPY yarn.lock ./

# Install dependencies
RUN npm install
# If using yarn, uncomment the next line
# RUN yarn install

# Copy the rest of the application source code
COPY . .

# Build the app
RUN npm run build
# If using yarn, uncomment the next line
# RUN yarn build

# Stage 2: Serve the app using a static server
FROM nginx:alpine AS serve

# Set working directory to nginx resources directory
WORKDIR /usr/share/nginx/html

# Remove default nginx static assets
RUN rm -rf ./*

# Copy static assets from builder stage
COPY --from=build /app/build .

# Containers run nginx with global directives and daemon off
ENTRYPOINT ["nginx", "-g", "daemon off;"]
