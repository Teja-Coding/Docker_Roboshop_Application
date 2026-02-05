# FROM node:20
# WORKDIR /opt/server
# EXPOSE 8080
# COPY package.json .
# COPY *.js .
# RUN npm install
# ENV REDIS_HOST="redis" \
#     CATALOGUE_HOST="catalogue" \
#     CATALOGUE_PORT="8080"
# CMD [ "node","server.js" ]  

# Stage 1: Build stage (used only to install dependencies)
FROM node:20.19.5-alpine3.21 AS build

# Set working directory inside container
WORKDIR /opt/server

# Copy only package.json first
# This helps Docker cache npm install
COPY package.json .

# Copy application source files
COPY *.js .

# Install node dependencies
# This stage will NOT be part of final image
RUN npm install 


# Stage 2: Runtime stage (final image)
FROM node:20.19.5-alpine3.21

# Set working directory
WORKDIR /opt/server

# Create non-root user and group (security best practice)
# -S = system user/group (no login shell)
RUN addgroup -S roboshop && \
    adduser -S roboshop -G roboshop && \
    chown -R roboshop:roboshop /opt/server

# Application listens on port 8080
EXPOSE 8080

# Image metadata (just information, no effect on runtime)
LABEL com.project="roboshop" \
      component="catalogue" \
      created_by="sivakumar"

# Environment variables used by the application
ENV MONGO="true" \
    MONGO_URL="mongodb://mongodb:27017/catalogue"

# Copy app + node_modules from build stage
# --chown makes roboshop user the owner
COPY --from=build --chown=roboshop:roboshop /opt/server /opt/server

# Switch to non-root user
USER roboshop

# ENTRYPOINT = main command (node)
ENTRYPOINT ["node"]

# CMD = default arguments (script name)
CMD ["server.js"]


