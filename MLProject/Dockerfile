# Use Python 3.8 as base image
FROM python:3.8-slim

# Set working directory
WORKDIR /app

# Copy requirements file
COPY conda.yaml .

# Install system dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    curl \
    && rm -rf /var/lib/apt/lists/*

# Install conda
RUN curl -L -o ~/miniconda.sh https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh \
    && chmod +x ~/miniconda.sh \
    && ~/miniconda.sh -b -p /opt/conda \
    && rm ~/miniconda.sh

# Add conda to path
ENV PATH /opt/conda/bin:$PATH

# Create conda environment
RUN conda env create -f conda.yaml

# Activate conda environment
SHELL ["conda", "run", "-n", "stunting-prediction", "/bin/bash", "-c"]

# Copy project files
COPY . .

# Copy model and data files
COPY artifacts/ /app/artifacts/

# Set environment variables
ENV MLFLOW_TRACKING_URI="https://dagshub.com/agummds/Stunting-Toddler.mlflow"

# Command to run the application
CMD ["python", "modelling.py"] 