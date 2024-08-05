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
            justify-content: center;
            align-items: center;
            flex-direction: column;
            padding: 20px;
        }
        .button-container {
            margin-bottom: 20px;
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
        }
        .add-btn, .close-btn, .search-btn {
            background-color: #888;
            color: #fff;
            border: none;
            padding: 10px 20px;
            cursor: pointer;
            border-radius: 4px;
            margin: 5px;
        }
        .add-btn:hover, .close-btn:hover, .search-btn:hover {
            background-color: #aaa;
        }
        .add-btn {
            background-color: #ff4444;
        }
        .search-btn {
            background-color: #007bff;
        }
        .form-container {
            display: none;
            background-color: #555;
            padding: 20px;
            border-radius: 8px;
            margin-top: 20px;
            width: 100%;
            max-width: 600px;
        }
        .modal-container {
            display: none;
            background-color: rgba(0, 0, 0, 0.7);
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            justify-content: center;
            align-items: center;
            z-index: 1000;
        }
        .modal-content {
            background-color: #555;
            padding: 20px;
            border-radius: 8px;
            text-align: center;
            max-width: 600px;
            width: 90%;
        }
        .modal-content img {
            max-width: 100%;
            height: auto;
            border-radius: 4px;
            max-height: 400px;
            object-fit: contain;
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
            width: 100%;
            height: auto;
            object-fit: cover;
            border-radius: 4px;
            cursor: pointer;
            transition: transform 0.2s;
        }
        .product-card img:hover {
            transform: scale(1.05);
        }
        .product-card h2 {
            margin: 10px 0;
            font-size: 16px;
            display: block;
            overflow: hidden;
            text-overflow: ellipsis;
            white-space: nowrap;
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
        }
        form button:hover {
            background-color: #aaa;
        }
        .price-container {
            display: flex;
            justify-content: space-between;
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
        .modal-content .button-container {
            display: flex;
            justify-content: space-around;
        }
        .modal-content .button-container .edit-btn, 
        .modal-content .button-container .delete-btn,
        .modal-content .button-container .close-btn {
            background-color: #888;
            color: #fff;
            border: none;
            padding: 10px 20px;
            cursor: pointer;
            border-radius: 4px;
            margin: 5px;
        }
        .modal-content .button-container .edit-btn:hover,
        .modal-content .button-container .delete-btn:hover,
        .modal-content .button-container .close-btn:hover {
            background-color: #aaa;
        }
    </style>
</head>
<body>
    <div class="button-container">
        <button class="add-btn" onclick="toggleAddForm()">Agregar Producto</button>
        <button class="search-btn" onclick="toggleSearchForm()">Buscar Producto</button>
    </div>
    <div class="form-container" id="form-container">
        <form id="add-form">
            <h2>Agregar Producto</h2>
            <label for="add-name">Nombre:</label>
            <input type="text" id="add-name" required>
            <label for="add-image">Imagen:</label>
            <input type="file" id="add-image" accept="image/*" required>
            <div class="price-container">
                <div>
                    <label for="add-purchase-price">Precio de Compra:</label>
                    <input type="number" id="add-purchase-price" required>
                </div>
                <div>
                    <label for="add-sale-price">Precio de Venta:</label>
                    <input type="number" id="add-sale-price" required>
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
        <!-- Las tarjetas de productos se agregarán aquí dinámicamente -->
    </div>
    <div class="modal-container" id="edit-modal">
        <div class="modal-content">
            <h2>Editar Producto</h2>
            <label for="edit-name">Nombre:</label>
            <input type="text" id="edit-name">
            <label for="edit-image">Imagen:</label>
            <input type="file" id="edit-image" accept="image/*">
            <div class="price-container">
                <div>
                    <label for="edit-purchase-price">Precio de Compra:</label>
                    <input type="number" id="edit-purchase-price">
                </div>
                <div>
                    <label for="edit-sale-price">Precio de Venta:</label>
                    <input type="number" id="edit-sale-price">
                </div>
            </div>
            <label for="edit-description">Descripción:</label>
            <textarea id="edit-description" rows="3"></textarea>
            <label for="edit-quantity">Cantidad:</label>
            <input type="number" id="edit-quantity">
            <button onclick="saveChanges()">Guardar Cambios</button>
            <button class="close-btn" onclick="closeEditModal()">Cancelar</button>
        </div>
    </div>
    <div class="modal-container" id="product-modal">
        <div class="modal-content">
            <img id="modal-img" src="" alt="Producto">
            <h2 id="modal-name"></h2>
            <p id="modal-price"></p>
            <p id="modal-sale-price"></p>
            <div class="description-box" id="modal-description"></div>
            <p id="modal-quantity"></p>
            <div class="button-container">
                <button class="edit-btn" onclick="openEditModalFromModal()">Editar</button>
                <button class="delete-btn" onclick="deleteProduct(currentEditingProductId)">Eliminar</button>
                <button class="close-btn" onclick="closeModal()">Cerrar</button>
            </div>
        </div>
    </div>
    <script>
        let products = JSON.parse(localStorage.getItem('products')) || [];
        function saveProductsToLocalStorage() {
            localStorage.setItem('products', JSON.stringify(products));
        }
        function renderGrid() {
            const grid = document.getElementById('product-grid');
            grid.innerHTML = '';
            products.forEach(product => {
                const card = document.createElement('div');
                card.className = 'product-card';
                card.innerHTML = `
                    <img src="${product.image}" alt="${product.name}" onclick="showModal(${product.id})">
                    <h2>${product.name}</h2>
                `;
                grid.appendChild(card);
            });
        }
        let currentEditingProductId = null;
        function showModal(id) {
            const product = products.find(p => p.id === id);
            document.getElementById('modal-img').src = product.image;
            document.getElementById('modal-name').textContent = product.name;
            document.getElementById('modal-price').textContent = `Precio de Compra: ${product.purchasePrice}`;
            document.getElementById('modal-sale-price').textContent = `Precio de Venta: ${product.salePrice}`;
            document.getElementById('modal-description').textContent = product.description;
            document.getElementById('modal-quantity').textContent = product.quantity;
            document.getElementById('product-modal').style.display = 'flex';
            currentEditingProductId = id;
        }
        function closeModal() {
            document.getElementById('product-modal').style.display = 'none';
        }
        function toggleAddForm() {
            const formContainer = document.getElementById('form-container');
            formContainer.style.display = formContainer.style.display === 'none' ? 'block' : 'none';
        }
        function toggleSearchForm() {
            const searchContainer = document.getElementById('search-container');
            searchContainer.style.display = searchContainer.style.display === 'none' ? 'block' : 'none';
        }
        document.getElementById('add-form').addEventListener('submit', function(event) {
            event.preventDefault();
            const name = document.getElementById('add-name').value;
            const imageFile = document.getElementById('add-image').files[0];
            const reader = new FileReader();   
            reader.onload = function(e) {
                const image = e.target.result;
                const purchasePrice = `$${parseFloat(document.getElementById('add-purchase-price').value).toFixed(2)}`;
                const salePrice = `$${parseFloat(document.getElementById('add-sale-price').value).toFixed(2)}`;
                const description = document.getElementById('add-description').value;
                const quantity = `${parseInt(document.getElementById('add-quantity').value)} disponibles`;
                const id = products.length ? products[products.length - 1].id + 1 : 1;
                products.push({ id, image, name, purchasePrice, salePrice, description, quantity });
                saveProductsToLocalStorage();
                renderGrid();
                document.getElementById('add-form').reset();
                toggleAddForm();
            };
            if (imageFile) {
                reader.readAsDataURL(imageFile);
            }
        });
        function searchProduct() {
            const searchName = document.getElementById('search-name').value.toLowerCase();
            const filteredProducts = products.filter(p => p.name.toLowerCase().includes(searchName));
            renderFilteredGrid(filteredProducts);
        }
        function renderFilteredGrid(filteredProducts) {
            const grid = document.getElementById('product-grid');
            grid.innerHTML = '';
            filteredProducts.forEach(product => {
                const card = document.createElement('div');
                card.className = 'product-card';
                card.innerHTML = `
                    <img src="${product.image}" alt="${product.name}" onclick="showModal(${product.id})">
                    <h2>${product.name}</h2>
                `;
                grid.appendChild(card);
            });
            toggleSearchForm();
        }
        function openEditModal(id) {
            const product = products.find(p => p.id === id);
            currentEditingProductId = id;
            document.getElementById('edit-name').value = product.name;
            document.getElementById('edit-purchase-price').value = parseFloat(product.purchasePrice.replace('$', '')).toFixed(2);
            document.getElementById('edit-sale-price').value = parseFloat(product.salePrice.replace('$', '')).toFixed(2);
            document.getElementById('edit-description').value = product.description;
            document.getElementById('edit-quantity').value = parseInt(product.quantity);
            document.getElementById('edit-modal').style.display = 'flex';
        }
        function openEditModalFromModal() {
            closeModal();
            openEditModal(currentEditingProductId);
        }
        function closeEditModal() {
            document.getElementById('edit-modal').style.display = 'none';
        }
        function saveChanges() {
            const name = document.getElementById('edit-name').value;
            const purchasePrice = `$${parseFloat(document.getElementById('edit-purchase-price').value).toFixed(2)}`;
            const salePrice = `$${parseFloat(document.getElementById('edit-sale-price').value).toFixed(2)}`;
            const description = document.getElementById('edit-description').value;
            const quantity = `${parseInt(document.getElementById('edit-quantity').value)} disponibles`;
            const imageFile = document.getElementById('edit-image').files[0];
            const reader = new FileReader();   
            reader.onload = function(e) {
                const image = e.target.result;
                const product = products.find(p => p.id === currentEditingProductId);
                product.name = name;
                product.purchasePrice = purchasePrice;
                product.salePrice = salePrice;
                product.description = description;
                product.quantity = quantity;
                if (imageFile) {
                    product.image = image;
                }
                saveProductsToLocalStorage();
                renderGrid();
                closeEditModal();
            };
            if (imageFile) {
                reader.readAsDataURL(imageFile);
            } else {
                const product = products.find(p => p.id === currentEditingProductId);
                product.name = name;
                product.purchasePrice = purchasePrice;
                product.salePrice = salePrice;
                product.description = description;
                product.quantity = quantity;
                saveProductsToLocalStorage();
                renderGrid();
                closeEditModal();
            }
        }
        function deleteProduct(id) {
            products = products.filter(p => p.id !== id);
            saveProductsToLocalStorage();
            renderGrid();
            closeModal();
        }
        // Renderizar la tabla al cargar la página
        renderGrid();
    </script>
</body>
</html>
