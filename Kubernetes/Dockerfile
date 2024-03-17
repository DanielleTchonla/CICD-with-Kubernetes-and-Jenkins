

# Use an official web server as a parent image
FROM nginx:alpine

# Set the working directory in the container
WORKDIR /usr/share/nginx/html

# Copy the application files
COPY index.html .
COPY license.txt .
COPY js/ js/
COPY img/ img/
COPY css/ css/
COPY fonts/ fonts/

# Expose the port your app runs on
EXPOSE 80

# Define the command to start the web server
CMD ["nginx", "-g", "daemon off;"]


