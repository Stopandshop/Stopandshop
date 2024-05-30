<!DOCTYPE html>
<html lang="ar">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Stop & Shop</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f4;
            color: #333;
            margin: 0;
            padding: 20px;
            text-align: center;
        }
        header {
            background-color: #0073e6;
            color: white;
            padding: 10px 0;
        }
        h1 {
            margin: 0;
        }
        .container {
            max-width: 800px;
            margin: 20px auto;
            padding: 20px;
            background: white;
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
        }
        .product-list, .add-product, .login-form, .register-form {
            text-align: left;
            margin-top: 20px;
        }
        .product, .login-form input, .login-form button, .register-form input, .register-form button {
            padding: 10px;
            margin-bottom: 10px;
            width: calc(100% - 20px);
            box-sizing: border-box;
        }
        .product {
            border-bottom: 1px solid #ddd;
        }
        .product:last-child {
            border-bottom: none;
        }
        footer {
            margin-top: 20px;
            color: #777;
        }
        input[type="text"], input[type="password"], input[type="number"] {
            width: calc(100% - 20px);
            padding: 8px;
            margin: 10px 0;
            border: 1px solid #ccc;
            border-radius: 4px;
        }
        button {
            padding: 10px 20px;
            border: none;
            background-color: #0073e6;
            color: white;
            cursor: pointer;
            border-radius: 4px;
        }
        button:hover {
            background-color: #005bb5;
        }
        .social-links {
            margin-top: 20px;
        }
        .social-links a {
            display: inline-block;
            margin: 0 10px;
            text-decoration: none;
            color: white;
            padding: 10px 20px;
            border-radius: 4px;
        }
        .whatsapp { background-color: #25D366; }
        .facebook { background-color: #3b5998; }
        .tiktok { background-color: #000000; }
        .scanner-container {
            display: none;
            margin-top: 20px;
        }
        #scanner {
            width: 100%;
        }
    </style>
</head>
<body>

<header>
    <h1>Stop & Shop</h1>
    <p>حاروف-حي البيدر -الشارع العام</p>
    <p>⏰ ساعات العمل: 8:00 صباحًا - 10:00 مساءً</p>
</header>

<!-- صفحة تسجيل الدخول -->
<div class="container" id="login-container">
    <h2>تسجيل دخول</h2>
    <div class="login-form">
        <h3>تسجيل دخول الموظفين</h3>
        <input type="text" id="employee-username" placeholder="اسم المستخدم" required>
        <input type="password" id="employee-password" placeholder="كلمة المرور" required>
        <button onclick="employeeLogin()">تسجيل دخول الموظفين</button>
    </div>
    <div class="login-form">
        <h3>تسجيل دخول الزبائن</h3>
        <input type="text" id="customer-username" placeholder="اسم المستخدم" required>
        <input type="password" id="customer-password" placeholder="كلمة المرور" required>
        <button onclick="customerLogin()">تسجيل دخول الزبائن</button>
    </div>
    <div class="register-form">
        <h3><a href="#">تسجيل زبون جديد</a></h3>
    </div>
</div>

<!-- صفحة المنتجات للموظفين -->
<!-- صفحة المنتجات للموظفين -->
<div class="container" id="employee-container" style="display: none;">
    <h2>المنتجات</h2>
    <div class="product-list" id="employee-product-list">
        <!-- سيتم إضافة المنتجات هنا -->
    </div>

    <h2>أضف منتج جديد</h2>
    <div class="add-product">
        <input type="text" id="product-name" placeholder="اسم المنتج" required>
        <input type="number" id="product-price" placeholder="السعر" required>
        <input type="text" id="product-barcode" placeholder="الباركود" required>
        <button onclick="addProduct()">إضافة</button>
    </div>
    <button onclick="goBackToLogin()">العودة إلى صفحة تسجيل الدخول</button>
</div>


<!-- صفحة المنتجات للزبائن -->
<div class="container" id="customer-container" style="display: none;">
    <h2>المنتجات والأسعار</h2>
    <div class="product-list" id="customer-product-list">
        <div class="search-form">
            <h3>البحث عن المنتجات</h3>
            <input type="text" id="search-input" placeholder="اسم المنتج" oninput="searchProducts()">
            <button onclick="startScanner()">مسح الباركود</button>
        </div>
        <!-- سيتم عرض المنتجات هنا -->
    </div>
    <div class="scanner-container" id="scanner-container">
        <h3>مسح الباركود</h3>
        <video id="scanner" style="width: 100%;"></video>
        <button onclick="stopScanner()">إيقاف المسح</button>
    </div>
    <button onclick="goBackToLogin()">العودة إلى صفحة تسجيل الدخول</button>
</div>

<div class="social-links">
    <a href="https://chat.whatsapp.com/your-whatsapp-group-link" class="whatsapp">WhatsApp</a>
    <a href="https://www.facebook.com/your-facebook-group-link" class="facebook">Facebook</a>
    <a href="https://www.tiktok.com/@your-tiktok-profile" class="tiktok">TikTok</a>
</div>

<footer>
    <p>📍 Harouf, Al baydar, Main Street</p>
</footer>

<script>
    const employeeProductList = document.getElementById('employee-product-list');
    const customerProductList = document.getElementById('customer-product-list');
    const loginContainer = document.getElementById('login-container');
    const employeeContainer = document.getElementById('employee-container');
    const customerContainer = document.getElementById('customer-container');
    const scannerContainer = document.getElementById('scanner-container');

    // تحميل المنتجات والمستخدمين من التخزين المحلي عند تحميل الصفحة
    document.addEventListener('DOMContentLoaded', () => {
        loadProducts();
        loadCustomers();
    });

    // دالة تسجيل دخول الموظفين
    function employeeLogin() {
        const username = document.getElementById('employee-username').value;
        const password = document.getElementById('employee-password').value;

        // تحقق من بيانات الدخول
        if (username === 'admin' && password === '1234') {
            loginContainer.style.display = 'none';
            employeeContainer.style.display = 'block';
        } else {
            alert('اسم المستخدم أو كلمة المرور غير صحيحة');
        }
    }

    // دالة تسجيل دخول الزبائن
    function customerLogin() {
        const username = document.getElementById('customer-username').value;
        const password = document.getElementById('customer-password').value;

        // تحميل بيانات المستخدمين من التخزين المحلي
        let customers = localStorage.getItem('customers');
        customers = customers ? JSON.parse(customers) : [];

        // تحقق من بيانات الدخول
        const customer = customers.find(c => c.username === username && c.password === password);
        if (customer) {
            loginContainer.style.display = 'none';
            customerContainer.style.display = 'block';
        } else {
            alert('اسم المستخدم أو كلمة المرور غير صحيحة');
        }
    }

    // دالة تسجيل زبون جديد
    function registerCustomer() {
        alert('تسجيل زبون جديد');
    }

    // دالة العودة إلى صفحة تسجيل الدخول
    function goBackToLogin() {
        employeeContainer.style.display = 'none';
        customerContainer.style.display = 'none';
        loginContainer.style.display = 'block';
    }

    // دالة لحذف المنتج
    function deleteProduct(productName) {
        let products = localStorage.getItem('products');
        products = products ? JSON.parse(products) : [];
        products = products.filter(product => product.name !== productName);
        localStorage.setItem('products', JSON.stringify(products));

        // إعادة تحميل الصفحة لعرض التغييرات
        location.reload();
    }

    // دالة إضافة منتج
    function addProduct() {
        const name = document.getElementById('product-name').value;
        const price = document.getElementById('product-price').value;

        // تحقق من ملء الحقول
        if (name && price) {
            const product = { name, price };
            saveProduct(product);
            displayProduct(product);

            // تفريغ الحقول
            document.getElementById('product-name').value = '';
            document.getElementById('product-price').value = '';
        } else {
            alert('يرجى ملء جميع الحقول');
        }
    }

    // دالة حفظ المنتج في التخزين المحلي
    function saveProduct(product) {
        let products = localStorage.getItem('products');
        products = products ? JSON.parse(products) : [];
        products.push(product);
        localStorage.setItem('products', JSON.stringify(products));
    }

    // دالة تحميل المنتجات من التخزين المحلي
    function loadProducts() {
        let products = localStorage.getItem('products');
        products = products           ? JSON.parse(products) : [];
        products.forEach(product => displayProduct(product));
    }

    // دالة عرض المنتج على الصفحة
    function displayProduct(product) {
        const productDiv = document.createElement('div');
        productDiv.className = 'product';
        productDiv.innerHTML = `<strong>${product.name}</strong>: $${product.price}`;

        const productDivForCustomer = productDiv.cloneNode(true);
        customerProductList.appendChild(productDivForCustomer);
    }

    // دالة البحث عن المنتجات
    function searchProducts() {
        const searchInput = document.getElementById('search-input').value.toLowerCase();
        const productList = document.getElementById('customer-product-list');
        const products = productList.getElementsByClassName('product');

        // إظهار أو إخفاء المنتجات حسب نتائج البحث
        Array.from(products).forEach(product => {
            const productName = product.querySelector('strong').textContent.toLowerCase();
            if (productName.includes(searchInput)) {
                product.style.display = 'block';
            } else {
                product.style.display = 'none';
            }
        });
    }

    // دالة بدء مسح الباركود
    function startScanner() {
        scannerContainer.style.display = 'block';

        const scanner = new Instascan.Scanner({ video: document.getElementById('scanner') });
        scanner.addListener('scan', function (content) {
            alert('تم قراءة الباركود: ' + content);
            stopScanner();
        });

        Instascan.Camera.getCameras().then(function (cameras) {
            if (cameras.length > 0) {
                scanner.start(cameras[0]);
            } else {
                alert('لا يوجد كاميرا متاحة');
            }
        }).catch(function (e) {
            console.error(e);
        });
    }

    // دالة إيقاف مسح الباركود
    function stopScanner() {
        scannerContainer.style.display = 'none';
    }

</script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/instascan/1.0.0/instascan.min.js"></script>
</body>
</html>
