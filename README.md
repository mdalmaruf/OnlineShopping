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

## Create index.html

1. Inside `templates`, create `index.html` with the following content:

    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Online Shopping</title>
        <link rel="stylesheet" href="{{ url_for('static', filename='styles.css') }}">
    </head>
    <body>
        <h1>Products</h1>
        <div class="product-list">
            {% for product in products %}
            <div class="product">
                <img src="{{ product.image }}" alt="{{ product.name }}">
                <h2>{{ product.name }}</h2>
                <p>Price: {{ product.price }}</p>
                <p>Type: {{ product.type }}</p>
            </div>
            {% endfor %}
        </div>
        <a href="{{ url_for('add') }}">Add a new product</a>
    </body>
    </html>
    ```

## Create add.html

1. Inside `templates`, create `add.html` with the following content:

    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Add Product</title>
        <link rel="stylesheet" href="{{ url_for('static', filename='styles.css') }}">
    </head>
    <body>
        <h1>Add a New Product</h1>
        <form action="{{ url_for('add') }}" method="post">
            <label for="id">ID:</label>
            <input type="text" id="id" name="id" required><br>
            <label for="name">Name:</label>
            <input type="text" id="name" name="name" required><br>
            <label for="price">Price:</label>
            <input type="text" id="price" name="price" required><br>
            <label for="type">Type:</label>
            <input type="text" id="type" name="type" required><br>
            <label for="image">Image URL:</label>
            <input type="text" id="image" name="image" required><br>
            <input type="submit" value="Add Product">
        </form>
        <a href="{{ url_for('index') }}">Back to Products</a>
    </body>
    </html>
    ```

## Add CSS for Better UI

### Create Static Folder

1. In the project directory, create a folder named `static`.
2. Inside `static`, create a file named `styles.css` with the following content:

    ```css
    body {
        font-family: Arial, sans-serif;
        margin: 0;
        padding: 0;
        background-color: #f4f4f4;
    }
    h1 {
        text-align: center;
        margin-top: 20px;
    }
    .product-list {
        display: flex;
        flex-wrap: wrap;
        justify-content: center;
        padding: 20px;
    }
    .product {
        background-color: white;
        border: 1px solid #ddd;
        border-radius: 5px;
        box-shadow: 0 2px 5px rgba(0,0,0,0.1);
        margin: 10px;
        padding: 20px;
        width: 200px;
        text-align: center;
    }
    .product img {
        max-width: 100%;
        height: auto;
    }
    .product h2 {
        font-size: 1.2em;
    }
    form {
        display: flex;
        flex-direction: column;
        align-items: center;
    }
    form label, form input {
        margin: 5px;
    }
    ```

