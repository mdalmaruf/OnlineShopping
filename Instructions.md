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

## Add Products to Text File

### Create products.txt

1. In the project directory, create a file named `products.txt`.
2. Add product details in the following format:

    ```plaintext
    1,Product A,10.99,Type 1,https://via.placeholder.com/150
    2,Product B,15.49,Type 2,https://via.placeholder.com/150
    3,Product C,8.99,Type 3,https://via.placeholder.com/150
    ```

## Run the Application

### Run the Flask App

1. In Visual Studio, set the startup file to `OnlineShopping.py` if not already set.
2. Click **Run** or press **F5** to start the server.
3. Open your web browser and navigate to [http://127.0.0.1:5000/](http://127.0.0.1:5000/).

## Push to GitHub

### Initialize Git

1. In your project directory, open a terminal and run:

    ```bash
    git init
    git add .
    git commit -m "Initial commit"
    ```

### Create a GitHub Repository

1. Go to GitHub and create a new repository named `OnlineShopping`.

### Push to GitHub

1. Follow the instructions provided by GitHub to push your local repository to GitHub:

    ```bash
    git remote add origin https://github.com/yourusername/OnlineShopping.git
    git branch -M main
    git push -u origin main
    ```

## Deploy to Google Cloud Platform

1. **Install Google Cloud SDK**: Follow the instructions to install the Google Cloud SDK.
2. **Create a New Project**: Go to the Google Cloud Console and create a new project.
3. **Enable Billing**: Ensure billing is enabled for your project.
4. **Deploy the Application**:

    1. In your project directory, create an `app.yaml` file with the following content:

        ```yaml
        runtime: python39

        entrypoint: gunicorn -b :$PORT OnlineShopping:app

        env_variables:
          PYTHON_ENV: 'development'
        ```

    2. Deploy your application:

        ```bash
        gcloud app deploy
        ```

5. Access your deployed application at `https://<your-project-id>.appspot.com`.

## Deploy to Microsoft Azure

1. **Install Azure CLI**: Follow the instructions to install the Azure CLI.
2. **Create a Resource Group**:

    ```bash
    az group create --name online-shopping-group --location <your-location>
    ```

3. **Create an App Service Plan**:

    ```bash
    az appservice plan create --name online-shopping-plan --resource-group online-shopping-group --sku FREE
    ```

4. **Create a Web App**:

    ```bash
    az webapp create --resource-group online-shopping-group --plan online-shopping-plan --name online-shopping-app --runtime "PYTHON:3.9"
    ```

5. **Deploy the Application**:

    1. Initialize a local Git repository:

        ```bash
        git init
        git add .
        git commit -m "Initial commit"
        ```

    2. Configure the Azure remote and push your code:

        ```bash
        az webapp deployment source config-local-git --name online-shopping-app --resource-group online-shopping-group
        git remote add azure <deployment-url>
        git push azure main
        ```

6. Access your deployed application at `https://online-shopping-app.azurewebsites.net`.
