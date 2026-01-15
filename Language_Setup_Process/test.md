# Languages and Framework Setup Process

A comprehensive guide for setting up development environments, virtual environments, and popular Python frameworks for AI/ML development.

## 🐍 Python Virtual Environment Setup

### Creating Virtual Environments

#### Windows
```bash
python -m venv venv
```

#### Mac/Linux
```bash
python3 -m venv venv
```

### Activating Virtual Environments

#### Windows (Command Prompt/PowerShell)
```bash
venv\Scripts\activate
```

#### Mac/Linux
```bash
 source venv/bin/activate
```

### Dependency Management

#### Save Dependencies
```bash
pip freeze > requirements.txt
```

#### Install Dependencies
```bash
pip install -r requirements.txt
```

### Running Python Code
```bash
python main.py
```

## 🎯 Django Project Setup

```powershell
# Install Django
pip install django

# Create new Django project
django-admin startproject projectName

# Navigate to project directory
cd projectName

# Create Django app
python manage.py startapp AppName

# Run development server
python manage.py runserver

# Create migrations
python manage.py makemigrations

# Apply migrations
python manage.py migrate

# Create superuser
python manage.py createsuperuser
```

## 🌊 Streamlit Setup

Streamlit is a fast way to build and share web apps for data science and ML.

### Installation
```bash
pip install streamlit
```

### Running Streamlit Apps
```bash
streamlit run app.py
```

## 🎨 Gradio Setup

Gradio allows you to quickly create UIs for your ML models.

### Installation
```bash
pip install gradio
```

### Running Gradio Apps
```bash
python app.py
```

## 📊 MLflow Setup

MLflow is an open-source platform for managing the end-to-end machine learning lifecycle.

### Installation
```bash
pip install mlflow[extras]
```

### Starting the Tracking UI
```bash
mlflow ui
```

### Basic MLflow Example

Wrap your training code in an `mlflow.start_run()` block:

```python
import mlflow
import mlflow.sklearn
from sklearn.ensemble import RandomForestRegressor
from sklearn.datasets import make_regression
from sklearn.model_selection import train_test_split
from sklearn.metrics import mean_squared_error

# Generate sample data
X, y = make_regression(n_features=2, n_informative=2, random_state=42)
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2)

# Set experiment name (optional but recommended)
mlflow.set_experiment("My First Experiment")

# Start a training run
with mlflow.start_run():
    
    # Train model
    model = RandomForestRegressor(n_estimators=100, random_state=42)
    model.fit(X_train, y_train)
    
    # Make predictions and calculate metrics
    predictions = model.predict(X_test)
    mse = mean_squared_error(y_test, predictions)
    
    # Log parameters
    mlflow.log_param("n_estimators", 100)
    mlflow.log_param("random_state", 42)
    
    # Log metrics
    mlflow.log_metric("MSE", mse)
    
    # Log model
    mlflow.sklearn.log_model(model, "random_forest_model")
    
    print(f"Run logged. MSE: {mse}")
```

### Running MLflow Experiments
```bash
# Run your training script
python train.py

# View results at http://localhost:5000
```

### Advanced: Custom Backend Store (Optional)
```bash
mlflow ui --backend-store-uri sqlite:///mlflow.db
```

## 📝 Notes

- Replace `projectName` and `AppName` with your actual project and app names
- Replace `main.py`, `app.py`, and `train.py` with your actual script filenames
- The MLflow UI runs on `http://localhost:5000` by default
- Always activate your virtual environment before installing packages

## 🔧 Troubleshooting 

- Ensure Python is properly installed and added to PATH
- Check that virtual environment activation shows `(venv)` in terminal prompt
- Verify package installations with `pip list`
- For Django, ensure you're in the correct project directory before running commands