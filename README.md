<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Marketplace de Productos</title>
    <style>
        body {
            background-color: #333;
            color: #fff;
            font-family: Arial, sans-serif;
            display: flex;
            flex-direction: row;
            padding: 20px;
            margin: 0;
        }
        .catalog-container {
            width: 200px;
            margin-right: 20px;
        }
        .catalog-container h2 {
            text-align: center;
        }
        .catalog-item {
            background-color: #444;
            padding: 10px;
            margin-bottom: 10px;
            border-radius: 4px;
            cursor: pointer;
            text-align: center;
        }
        .catalog-item:hover {
            background-color: #555;
        }
        .catalog-item.active {
            background-color: #777;
        }
        .button-container {
            display: flex;
            justify-content: center;
            margin-bottom: 20px;
            width: 100%;
        }
        .add-btn, .search-btn {
            background-color: #ff4444;
            color: #fff;
            border: none;
            padding: 10px 20px;
            cursor: pointer;
            border-radius: 4px;
            transition: all 0.3s ease;
            margin: 0 10px;
        }
        .search-btn {
            background-color: #007bff;
        }
        .add-btn:hover {
            background-color: #aa3333;
        }
        .search-btn:hover {
            background-color: #0056b3;
            box-shadow: 0 4px 8px rgba(0, 123, 255, 0.4);
        }
        .form-container, .modal-container, .edit-modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            justify-content: center;
            align-items: center;
            z-index: 1000;
        }
        .form-container, .edit-modal {
            background-color: #555;
            padding: 20px;
            border-radius: 8px;
            max-width: 500px;
            width: 90%;
            max-height: 90%;
            overflow: auto;
        }
        .modal-content {
            background-color: #555;
            padding: 20px;
            border-radius: 8px;
            text-align: center;
            max-width: 500px;
            width: 90%;
        }
        .modal-content img {
            width: 200px;
            height: 200px;
            border-radius: 4px;
            object-fit: cover;
        }
        .product-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
            gap: 20px;
            width: 100%;
        }
        .product-card {
            background-color: #444;
            border-radius: 8px;
            overflow: hidden;
            text-align: center;
            position: relative;
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 10px;
        }
        .product-card img {
            width: 200px;
            height: 200px;
            object-fit: cover;
            border-radius: 4px;
            cursor: pointer;
            transition: transform 0.2s;
        }
        .product-card img:hover {
            transform: scale(1.05);
        }
        form label {
            display: block;
            margin-top: 10px;
        }
        form input, form textarea {
            width: 100%;
            padding: 8px;
            margin-top: 5px;
            border-radius: 4px;
            border: none;
        }
        form button {
            background-color: #888;
            color: #fff;
            border: none;
            padding: 10px;
            cursor: pointer;
            border-radius: 4px;
            margin-top: 10px;
            transition: background-color 0.3s ease;
        }
        form button:hover {
            background-color: #aaa;
        }
        .price-container {
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .price-container div {
            flex: 1;
            margin-right: 5px;
        }
        .price-container div:last-child {
            margin-right: 0;
        }
        @media (max-width: 600px) {
            .price-container {
                flex-direction: column;
            }
            .price-container div {
                margin-right: 0;
                margin-bottom: 10px;
            }
            .price-container div:last-child {
                margin-bottom: 0;
            }
        }
        .description-box {
            background-color: #666;
            padding: 10px;
            border-radius: 4px;
            margin-top: 10px;
        }
        .modal-content .button-container, .edit-modal .button-container {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-top: 20px;
        }
        .modal-content .button-container .edit-btn,
        .modal-content .button-container .delete-btn,
        .modal-content .button-container .close-btn,
        .edit-modal .button-container .edit-btn,
        .edit-modal .button-container .cancel-btn {
            background-color: #888;
            color: #fff;
            border: none;
            padding: 10px 20px;
            cursor: pointer;
            border-radius: 4px;
            transition: background-color 0.3s ease;
        }
        .modal-content .button-container .edit-btn:hover,
        .modal-content .button-container .delete-btn:hover,
        .modal-content .button-container .close-btn:hover,
        .edit-modal .button-container .edit-btn:hover,
        .edit-modal .button-container .cancel-btn:hover {
            background-color: #aaa;
        }
    </style>
</head>
<body>
    <div class="catalog-container">
        <h2>Catálogo</h2>
        <div class="catalog-item" onclick="filterProducts('All')">Todos</div>
        <div class="catalog-item" onclick="filterProducts('Electronics')">Electrónica</div>
        <div class="catalog-item" onclick="filterProducts('Clothing')">Ropa</div>
        <div class="catalog-item" onclick="filterProducts('Home')">Hogar</div>
        <div class="catalog-item" onclick="filterProducts('Books')">Libros</div>
    </div>
    <div style="flex: 1;">
        <div class="button-container">
            <button class="add-btn" onclick="toggleAddForm()">Agregar Producto</button>
            <button class="search-btn" onclick="toggleSearchForm()">Buscar Producto</button>
        </div>
        <div class="form-container" id="form-container">
            <form id="add-form">
                <h2>Agregar Producto</h2>
                <label for="add-name">Nombre:</label>
                <input type="text" id="add-name" required>
                <label for="add-category">Categoría:</label>
                <select id="add-category" required>
                    <option value="Electronics">Electrónica</option>
                    <option value="Clothing">Ropa</option>
                    <option value="Home">Hogar</option>
                    <option value="Books">Libros</option>
                </select>
                <label for="add-image">Imagen:</label>
                <input type="file" id="add-image" accept="image/*" required>
                <div class="price-container">
                    <div>
                        <label for="add-purchase-price">Precio de Compra:</label>
                        <input type="number" id="add-purchase-price" step="0.01" required>
                    </div>
                    <div>
                        <label for="add-sale-price">Precio de Venta:</label>
                        <input type="number" id="add-sale-price" step="0.01" required>
                    </div>
                </div>
                <label for="add-description">Descripción:</label>
                <textarea id="add-description" rows="3" required></textarea>
                <label for="add-quantity">Cantidad:</label>
                <input type="number" id="add-quantity" required>
                <button type="submit">Agregar</button>
            </form>
        </div>
        <div class="form-container" id="search-container">
            <h2>Buscar Producto</h2>
            <label for="search-name">Nombre del Producto:</label>
            <input type="text" id="search-name">
            <button onclick="searchProduct()">Buscar</button>
            <button onclick="toggleSearchForm()">Cancelar</button>
        </div>
        <div class="product-grid" id="product-grid">
            <!-- Las tarjetas de productos se renderizarán aquí -->
        </div>
    </div>
    <div class="modal-container" id="product-modal">
        <div class="modal-content">
            <img id="modal-img" src="" alt="Producto">
           <h2 id="modal-name"></h2>
            <div class="price-container">
                <div id="modal-price"></div>
                <div id="modal-sale-price"></div>
            </div>
            <div id="modal-description" class="description-box"></div>
            <div id="modal-quantity"></div>
            <div class="button-container">
                <button class="edit-btn" onclick="openEditModal()">Editar</button>
                <button class="delete-btn" onclick="deleteProduct(currentEditingProductId)">Eliminar</button>
                <button class="close-btn" onclick="closeModal()">Cerrar</button>
            </div>
        </div>
    </div>
    <div class="edit-modal" id="edit-modal">
        <form id="edit-form">
            <h2>Editar Producto</h2>
            <label for="edit-name">Nombre:</label>
            <input type="text" id="edit-name" required>
            <label for="edit-category">Categoría:</label>
            <select id="edit-category" required>
                <option value="Electronics">Electrónica</option>
                <option value="Clothing">Ropa</option>
                <option value="Home">Hogar</option>
                <option value="Books">Libros</option>
            </select>
            <label for="edit-image">Imagen:</label>
            <input type="file" id="edit-image" accept="image/*">
            <div class="price-container">
                <div>
                    <label for="edit-purchase-price">Precio de Compra:</label>
                    <input type="number" id="edit-purchase-price" step="0.01" required>
                </div>
                <div>
                    <label for="edit-sale-price">Precio de Venta:</label>
                    <input type="number" id="edit-sale-price" step="0.01" required>
                </div>
            </div>
            <label for="edit-description">Descripción:</label>
            <textarea id="edit-description" rows="3" required></textarea>
            <label for="edit-quantity">Cantidad:</label>
            <input type="number" id="edit-quantity" required>
            <div class="button-container">
                <button type="submit" class="edit-btn">Guardar Cambios</button>
                <button type="button" class="cancel-btn" onclick="closeEditModal()">Cancelar</button>
            </div>
        </form>
    </div>
    <script>
        let currentEditingProductId = null;
        let currentCategory = 'All';
        function toggleAddForm() {
            const form = document.getElementById('form-container');
            form.style.display = form.style.display === 'none' ? 'flex' : 'none';
        }
        function toggleSearchForm() {
            const form = document.getElementById('search-container');
            form.style.display = form.style.display === 'none' ? 'flex' : 'none';
        }
        function openEditModal() {
            const modal = document.getElementById('edit-modal');
            const product = JSON.parse(localStorage.getItem('products')).find(p => p.id === currentEditingProductId);
            document.getElementById('edit-name').value = product.name;
            document.getElementById('edit-category').value = product.category;
            document.getElementById('edit-purchase-price').value = product.purchasePrice;
            document.getElementById('edit-sale-price').value = product.salePrice;
            document.getElementById('edit-description').value = product.description;
            document.getElementById('edit-quantity').value = product.quantity; 
            modal.style.display = 'flex';
        }
        function closeEditModal() {
            const modal = document.getElementById('edit-modal');
            modal.style.display = 'none';
        }
        function openModal(product) {
            const modal = document.getElementById('product-modal');
            document.getElementById('modal-img').src = product.image;
            document.getElementById('modal-name').textContent = product.name;
            document.getElementById('modal-price').textContent = `Precio de Compra: ${product.purchasePrice}`;
            document.getElementById('modal-sale-price').textContent = `Precio de Venta: ${product.salePrice}`;
            document.getElementById('modal-description').textContent = product.description;
            document.getElementById('modal-quantity').textContent = `Cantidad: ${product.quantity}`;
            modal.style.display = 'flex';
            currentEditingProductId = product.id;
        }
        function closeModal() {
            const modal = document.getElementById('product-modal');
            modal.style.display = 'none';
        }
        function deleteProduct(productId) {
            if (!confirm('¿Estás seguro de que quieres eliminar este producto?')) return;
            const products = JSON.parse(localStorage.getItem('products')) || [];
            const updatedProducts = products.filter(product => product.id !== productId);
            localStorage.setItem('products', JSON.stringify(updatedProducts));
            loadProducts(); 
            closeModal();
        }
        function searchProduct() {
            const name = document.getElementById('search-name').value.toLowerCase();
            const products = JSON.parse(localStorage.getItem('products')) || [];
            const filteredProducts = products.filter(product => product.name.toLowerCase().includes(name));
            renderProducts(filteredProducts);
        }
        function filterProducts(category) {
            currentCategory = category;
            const catalogItems = document.querySelectorAll('.catalog-item');
            catalogItems.forEach(item => {
                item.classList.remove('active');
                if (item.textContent === category || (category === 'All' && item.textContent === 'Todos')) {
                    item.classList.add('active');
                }
            });
            loadProducts();
        }
        document.getElementById('add-form').addEventListener('submit', function(e) {
            e.preventDefault();
            const name = document.getElementById('add-name').value;
            const category = document.getElementById('add-category').value;
            const image = document.getElementById('add-image').files[0];
            const purchasePrice = document.getElementById('add-purchase-price').value;
            const salePrice = document.getElementById('add-sale-price').value;
            const description = document.getElementById('add-description').value;
            const quantity = document.getElementById('add-quantity').value;
            const reader = new FileReader();
            reader.onload = function(event) {
                const newProduct = {
                    id: Date.now(),
                    name,
                    category,
                    image: event.target.result,
                    purchasePrice,
                    salePrice,
                    description,
                    quantity
                };
                const products = JSON.parse(localStorage.getItem('products')) || [];
                products.push(newProduct);
                localStorage.setItem('products', JSON.stringify(products));
                loadProducts();
                toggleAddForm();
            };
            reader.readAsDataURL(image);
        });
        document.getElementById('edit-form').addEventListener('submit', function(e) {
            e.preventDefault();
            const name = document.getElementById('edit-name').value;
            const category = document.getElementById('edit-category').value;
            const image = document.getElementById('edit-image').files[0];
            const purchasePrice = document.getElementById('edit-purchase-price').value;
            const salePrice = document.getElementById('edit-sale-price').value;
            const description = document.getElementById('edit-description').value;
            const quantity = document.getElementById('edit-quantity').value;
            const products = JSON.parse(localStorage.getItem('products')) || [];
            const updatedProducts = products.map(product => {
                if (product.id === currentEditingProductId) {
                    if (image) {
                        const reader = new FileReader();
                        reader.onload = function(event) {
                            product.image = event.target.result;
                            localStorage.setItem('products', JSON.stringify(products));
                            loadProducts();
                        };
                        reader.readAsDataURL(image);
                    }
                    product.name = name;
                    product.category = category;
                    product.purchasePrice = purchasePrice;
                    product.salePrice = salePrice;
                    product.description = description;
                    product.quantity = quantity;
                }
                return product;
            });
            localStorage.setItem('products', JSON.stringify(updatedProducts));
            loadProducts();
            closeEditModal();
        });
        function loadProducts() {
            const products = JSON.parse(localStorage.getItem('products')) || [];
            const filteredProducts = currentCategory === 'All' ? products : products.filter(product => product.category === currentCategory);
            renderProducts(filteredProducts);
        }
        function renderProducts(products) {
            const productGrid = document.getElementById('product-grid');
            productGrid.innerHTML = '';
            products.forEach(product => {
                const productCard = document.createElement('div');
                productCard.classList.add('product-card');
                const img = document.createElement('img');
                img.src = product.image;
                img.onclick = () => openModal(product);
                productCard.appendChild(img);
                const productName = document.createElement('h3');
                productName.textContent = product.name;
                productCard.appendChild(productName);
                productGrid.appendChild(productCard);
            });
        }
        loadProducts();
    </script>
</body>
</html>
