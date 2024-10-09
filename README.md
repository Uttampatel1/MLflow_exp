# 🚀 MLflow: Open-source platform for ML Lifecycle Management 📊📦🚀

MLflow, developed by Databricks, is an open-source platform that simplifies the end-to-end machine learning lifecycle. It enables efficient building, tracking, deployment, and management of machine learning models. 🏗️💻📊

## Table of Contents
- [Features](#features)
- [Installation](#installation)
- [Getting Started](#getting-started)
  - [Running MLflow Locally](#running-mlflow-locally)
  - [Tracking Experiments](#tracking-experiments)
  - [Managing Models](#managing-models)
  - [Using the MLflow API](#using-the-mlflow-api)
- [Advanced Features](#advanced-features)
  - [Model Registry](#model-registry)
  - [Projects](#projects)
  - [Deployment](#deployment)
- [Configuration](#configuration)
- [Common Use Cases](#common-use-cases)
- [Troubleshooting](#troubleshooting)
- [Examples](#examples)
- [Contributing](#contributing)
- [License](#license)



## Features

1.  📝 **Tracking**: Log and track experiments during model development. Record parameters, metrics, and artifacts (e.g., model files) for different runs. Easy comparison and reproducibility are facilitated. 📈📒🔄
    
2.  📦 **Projects**: Package ML code into projects for easy sharing and reproducibility. Versioning and deployment ensure consistency across teams. 📦🔗🚀
    
3.  🗄️ **Models**: Utilize the model registry to store and manage different model versions. Track model lineage and revert to previous versions if needed. 🗝️📁🔗
    
4.  🏭 **Model Serving**: Deploy models for inference with various options (Docker containers, REST API, TensorFlow Serving, Apache Spark). Bring ML to production! 🚀🌐🔮
    
5.  🎁 **Model Packaging**: Package trained models with their dependencies. Simplify deployment across different environments. 📦🚀🌐
6.  **REST API**: Access MLflow functionality programmatically.
    
    
## Installation

To install MLflow, you can use `pip`. Here’s how to install the latest version:

```bash
pip install mlflow
```

### Optional Dependencies

You may also need additional dependencies based on the ML frameworks you plan to use (e.g., TensorFlow, PyTorch). For example:

```bash
pip install mlflow[extras]  # Includes extra dependencies for various libraries
```

## Getting Started

### Running MLflow Locally

1. **Start MLflow Tracking Server**:
   To start the MLflow tracking server locally, run the following command in your terminal:

   ```bash
   mlflow ui

   mlflow server --host 127.0.0.1 --port 8080
   ```

   By default, this will start the MLflow UI at `http://localhost:5000`. You can access it in your web browser.
   `mlflow server` command starts the MLflow server at `http://127.0.0.1:8080`. You can access it in your web browser.

3. **Run MLflow with a Python Script**:
   Create a Python script (e.g., `train.py`) and include the following code to log an experiment:

   ```python
   import mlflow

   # Start MLflow tracking
   mlflow.start_run()

   # Log parameters
   mlflow.log_param("param1", 5)
   mlflow.log_param("param2", "test")

   # Log metrics
   mlflow.log_metric("metric1", 0.85)

   # End the run
   mlflow.end_run()
   ```

   Execute the script:

   ```bash
   python train.py
   ```

### Tracking Experiments

- You can log parameters, metrics, and artifacts during your experiments using the MLflow Python API.
  
- **Example**:

  ```python
  import mlflow

  mlflow.start_run()

  mlflow.log_param("alpha", 0.5)
  mlflow.log_metric("rmse", 0.4)

  # Log a model artifact
  mlflow.log_artifact("model.pkl")

  mlflow.end_run()
  ```

### Managing Models

1. **Log a Model**:
   To log a model, you can use the `mlflow.sklearn.log_model` or relevant functions for other frameworks.

   ```python
   from sklearn.linear_model import LogisticRegression
   import mlflow.sklearn

   model = LogisticRegression()
   mlflow.sklearn.log_model(model, "model_name")
   ```

2. **Load a Model**:
   You can load the model later using:

   ```python
   model = mlflow.sklearn.load_model("model_name")
   ```

### Using the MLflow API

MLflow provides a REST API to interact with the tracking server. You can perform operations like logging experiments, retrieving metrics, and managing models programmatically.

- **Example**: Log an experiment using the API

```bash
curl -X POST http://localhost:5000/api/2.0/mlflow/runs/log-parameter \
  -H 'Content-Type: application/json' \
  -d '{
    "run_id": "YOUR_RUN_ID",
    "key": "param1",
    "value": "value1"
  }'
```

## Advanced Features

### Model Registry

The Model Registry allows you to organize and manage your machine learning models. Key functionalities include:

- **Versioning**: Keep track of multiple versions of a model.
- **Staging**: Move models between different stages like "Staging", "Production", and "Archived".
- **Annotations**: Add comments or notes to model versions for better tracking.

**Example**:

To register a model in the Model Registry, you can use the following code:

```python
from mlflow.tracking import MlflowClient

client = MlflowClient()
model_uri = "runs:/<RUN_ID>/model_name"
client.create_registered_model("MyModel")
client.create_model_version("MyModel", model_uri, "1.0")
```

### Projects

MLflow Projects allow you to package data science code in a reusable and reproducible way. You can define dependencies and environment specifications using a `MLproject` file.

**Example**:

```yaml
name: My Project
conda_env: conda.yaml
entry_points:
  main:
    command: "python train.py"
```

To run the project, use:

```bash
mlflow run .
```

### Deployment

MLflow makes it easy to deploy your models to various platforms, including Docker, Azure ML, and AWS SageMaker. 

- **Example**: To deploy a model to a REST API:

```bash
mlflow models serve -m models:/MyModel/1.0 --port 5001
```

You can then send requests to your model via:

```bash
curl http://127.0.0.1:5001/invocations -H 'Content-Type: application/json' -d '{"columns":["feature1","feature2"],"data":[[1.0,2.0]]}'
```

## Configuration

You can configure MLflow by setting environment variables or modifying the `mlflow.conf` file. Some common configurations include:

- **Tracking URI**: Specify where MLflow should log the tracking data.
- **Artifact Location**: Define the location where artifacts are stored.

### Example of Setting Environment Variables

```bash
export MLFLOW_TRACKING_URI=http://localhost:5000
export MLFLOW_ARTIFACT_URI=/path/to/artifacts
```

## Common Use Cases

1. **Experiment Tracking**: Easily log and visualize your machine learning experiments.
2. **Model Deployment**: Seamlessly deploy models to production environments.
3. **Collaboration**: Share your models and experiments with your team.
4. **Reproducibility**: Track and manage versions of your models and data.

## Troubleshooting

- **MLflow UI Not Loading**: Ensure the tracking server is running and the correct port is used.
- **Error in Logging Parameters**: Verify that the parameters being logged are valid and match expected types.
- **Model Not Found**: Double-check the model URI and ensure that it has been logged correctly.

## Examples

For detailed examples of using MLflow with various machine learning frameworks, check out the [MLflow Documentation](https://www.mlflow.org/docs/latest/index.html) and the [Examples Repository](https://github.com/mlflow/mlflow/tree/master/examples).



