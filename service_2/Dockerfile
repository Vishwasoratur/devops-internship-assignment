# service_2/Dockerfile

# Use the official Python image
# We use python:3.9-slim-buster as it's a good balance of size and compatibility
FROM python:3.9-slim-buster

# Set the working directory inside the container
WORKDIR /app

# Install curl for health checks. 'apt-get update' and 'apt-get install -y curl' are for Debian-based images.
# This needs to be done BEFORE copying requirements.txt, if curl is needed for health checks or other build steps.
RUN apt-get update && apt-get install -y curl && rm -rf /var/lib/apt/lists/*

# Copy the requirements.txt first to leverage Docker layer caching
# If requirements.txt doesn't change, these layers won't be rebuilt
COPY requirements.txt .

# Install Python dependencies
# --no-cache-dir saves space by not caching package wheels
# -r requirements.txt installs packages listed in the file
RUN pip install --no-cache-dir -r requirements.txt

# Copy the rest of the application code
# This copies all files from the service_2 directory into /app
COPY . .

# Expose the port your Python application listens on
EXPOSE 8002

# Command to run the application when the container starts
CMD ["python", "app.py"]

