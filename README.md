# Online Shopping Flask Application

This repository contains a Flask application for online shopping with product details including ID, Name, Price, Type, and online images. The application runs on a Flask server and includes a CSS file for better UI. The database is stored in a text file.

## Table of Contents

- [Setup Environment](#setup-environment)
- [Create Flask Project](#create-flask-project)
- [Set Up Flask Application](#set-up-flask-application)
- [Add CSS for Better UI](#add-css-for-better-ui)
- [Add Products to Text File](#add-products-to-text-file)
- [Run the Application](#run-the-application)
- [Push to GitHub](#push-to-github)
- [Deploy to Google Cloud Platform](#deploy-to-google-cloud-platform)
- [Deploy to Microsoft Azure](#deploy-to-microsoft-azure)

## Setup Environment

1. **Install Python**: Download and install Python from [python.org](https://www.python.org/).

2. **Install Visual Studio**: Download and install Visual Studio from [visualstudio.microsoft.com](https://visualstudio.microsoft.com/).

3. **Install Flask**: Open your terminal and run:
   ```bash
   pip install flask
# Create Flask Project

## Open Visual Studio and select Create a new project

1. Select **Python Application** and click **Next**.
2. Configure your new project:
   - **Project name**: `OnlineShopping`
   - **Location**: Choose your desired directory
   - Click **Create**

## Set Up Flask Application

### Create a Virtual Environment

1. Open your terminal and navigate to your project directory.
2. Run the following command to create a virtual environment:

    ```bash
    python -m venv venv
    ```

3. Activate the virtual environment:

    **Windows:**

    ```bash
    .\venv\Scripts\activate
    ```

    **macOS/Linux:**

    ```bash
    source venv/bin/activate
    ```

4. Install Flask within the virtual environment:

    ```bash
    pip install flask
    ```

### Create the Flask App

1. In `OnlineShopping.py`, add the following code:

    ```python
    from flask import Flask, render_template, request, redirect, url_for
    import os

    app = Flask(__name__)

    # Function to read products from a text file
    def read_products():
        products = []
        if os.path.exists('products.txt'):
            with open('products.txt', 'r') as file:
                for line in file:
                    parts = line.strip().split(',')
                    products.append({
                        'id': parts[0],
                        'name': parts[1],
                        'price': parts[2],
                        'type': parts[3],
                        'image': parts[4]
                    })
        return products

    @app.route('/')
    def index():
        products = read_products()
        return render_template('index.html', products=products)

    @app.route('/add', methods=['GET', 'POST'])
    def add():
        if request.method == 'POST':
            id = request.form['id']
            name = request.form['name']
            price = request.form['price']
            type = request.form['type']
            image = request.form['image']
            with open('products.txt', 'a') as file:
                file.write(f"{id},{name},{price},{type},{image}\n")
            return redirect(url_for('index'))
        return render_template('add.html')

    if __name__ == '__main__':
        app.run(debug=True)
    ```

### Create Templates Folder

1. In the project directory, create a folder named `templates`.
2. Inside `templates`, create two files: `index.html` and `add.html`.

You are now ready to start your Flask application by running `OnlineShopping.py`.
