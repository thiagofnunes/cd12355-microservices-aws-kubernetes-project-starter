# Use a Python slim image as the base image
FROM python:3.9-slim

# Set the working directory inside the container
WORKDIR /app

# Copy the requirements.txt file into the container
COPY analytics/requirements.txt /app/

# Install dependencies
RUN apt-get update -y && apt install -y build-essential libpq-dev && \
    pip install --upgrade pip setuptools wheel && \
    pip install -r requirements.txt

# Copy the rest of the application code into the container
COPY analytics/app.py /app/
COPY analytics/config.py /app/


# Expose the port your app runs on (e.g., port 5153 for Flask apps)
EXPOSE 5152

# Run the application (adjust this command if your application starts differently)
CMD ["python3", "app.py"]
