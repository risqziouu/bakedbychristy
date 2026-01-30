<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>BakedByChristy</title>
    <style>
        body {
            margin: 0;
            font-family: Arial, sans-serif;
            background-color: #f5efe9;
            color: #3e2723;
        }

        header {
            background-color: #5d4037;
            color: white;
            padding: 30px;
            text-align: center;
        }

        .container {
            max-width: 1000px;
            margin: 40px auto;
            padding: 0 20px;
        }

        .menu {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
            gap: 20px;
        }

        .item {
            background-color: white;
            border-radius: 12px;
            padding: 20px;
            text-align: center;
            box-shadow: 0 4px 10px rgba(0,0,0,0.1);
        }

        .price {
            font-weight: bold;
            color: #6d4c41;
            margin-bottom: 15px;
        }

        .order-btn {
            background-color: #5d4037;
            color: white;
            border: none;
            padding: 10px 18px;
            border-radius: 25px;
            cursor: pointer;
            font-size: 1rem;
            transition: 0.3s;
        }

        .order-btn:hover {
            background-color: #4e342e;
        }

        footer {
            text-align: center;
            padding: 20px;
            background-color: #5d4037;
            color: white;
            margin-top: 40px;
        }
    </style>
</head>
<body>

<header>
    <h1>BakedByChristy</h1>
    <p>Freshly baked cookies & brownies 🤎</p>
</header>

<div class="container">
    <h2>Our Menu</h2>
    <div class="menu">

        <div class="item">
            <h3>Chocolate Chip Cookies</h3>
            <p class="price">$1.00</p>
            <a href="mailto:luvz4himm@gmail.com?subject=Order%20for%20Cookies%20or%20Special%20Order">
                <button class="order-btn">Order Now</button>
            </a>
        </div>

        <div class="item">
            <h3>Cookies n Creme Hershey's</h3>
            <p class="price">$3.00</p>
            <a href="mailto:luvz4himm@gmail.com?subject=Order%20for%20Cookies%20or%20Special%20Order">
                <button class="order-btn">Order Now</button>
            </a>
        </div>

        <div class="item">
            <h3>Double Stuff Oreo Cookies</h3>
            <p class="price">$3.00</p>
            <a href="mailto:luvz4himm@gmail.com?subject=Order%20for%20Cookies%20or%20Special%20Order">
                <button class="order-btn">Order Now</button>
            </a>
        </div>

        <div class="item">
            <h3>Brownies</h3>
            <p class="price">$3.00</p>
            <a href="mailto:luvz4himm@gmail.com?subject=Order%20for%20Cookies%20or%20Special%20Order">
                <button class="order-btn">Order Now</button>
            </a>
        </div>

    </div>
</div>

<footer>
    <p>© 2026 BakedByChristy</p>
</footer>

</body>
</html>
