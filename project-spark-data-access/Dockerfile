# Use the latest Ubuntu image
FROM ubuntu:latest

# Install Python
RUN apt-get update && \
    apt-get install -y python3

# Copy the Python script into the container
COPY src/access-data-spark.py access-data-spark.py

# Set the default command to execute the Python script
CMD ["python3", "/access-data-spark.py"]
