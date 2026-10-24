# Sample Python Flask Application

## Step-01: Overview

This sample python flask application details are as follows:

- **Execution flow**: src/app.py >> templates/index.html
- **Dependencies file**: requirements.txt
- **CSS for templates/index.html**: static/style.css
- **App listens on port**: 8080

## Step-02: How to run the flask application

### 2.1: Download & Install `Python`

```
# To check installed version of Python
python --version

[if the precending command says there is no python program present, proceed with the next command else skip & move to #2.2]

# Python download link
https://www.python.org/downloads/
```

### 2.2: Install `pip` package manager

```
# pip download link
https://bootstrap.pypa.io/get-pip.py

# Install pip
python get-pip.py

# To check current installed version of pip
pip --version

# (optional) To update pip
python.exe -m pip install --upgrade pip
```

### 2.3: Install `Flask` python module

```
pip install flask
```

### 2.4: Run the Application

```
# Switch to flask project folder
cd <your_flask_application_folder>

# Run the application
python .\src\app.py
```

## Step-03: Access the Application

- Navigate to your local system's browser and visit **localhost:8080**.
- You should see the Python flask app webpage.
