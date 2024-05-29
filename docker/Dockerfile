FROM python:3.10-slim

# Install necessary packages
RUN apt-get update && apt-get install gcc g++ git make -y && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Create a user and group with specific IDs
RUN useradd -m -u 1000 user && usermod -aG root user

# Set environment variables
ENV HOME=/home/user \
    PATH=/home/user/.local/bin:$PATH

# Create the required directory and set permissions before switching users
RUN mkdir -p $HOME/.cache/langflow && \
    chown -R user:user $HOME/.cache/langflow && \
    chmod -R 755 $HOME/.cache/langflow

# Set the user to the non-root user
USER user

# Set the working directory
WORKDIR $HOME/app

# Copy application files and ensure they have the correct ownership
COPY --chown=user . $HOME/app

# Install the required Python packages
RUN pip install langflow>=0.5.0 -U --user

ENV LANGFLOW_LOG_DIR=/dev/null

# Command to run the application
CMD ["python", "-m", "langflow", "run", "--host", "0.0.0.0", "--port", "7860"]
