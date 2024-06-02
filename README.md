# Codigo-Proyecto-Final-
document.addEventListener("DOMContentLoaded", function() {
// Datos de productos
const products = [
{ id: 1, name: "iPhone 13", price: 999, category: "smartphones" },
{ id: 2, name: "iPad Air", price: 599, category: "tablets" },
{ id: 3, name: "MacBook Pro", price: 1299, category: "laptops" },
];

// Función para renderizar productos
function renderProducts(category = "all") {
    const productList = document.getElementById("productList");
    productList.innerHTML = "";
    products.filter(product => category === "all" || product.category === category)
            .forEach(product => {
                productList.insertAdjacentHTML("beforeend", `
                    <div class="product-item">
                        <h3>${product.name}</h3>
                        <p>Price: $${product.price}</p>
                        <button class="add-to-cart" data-id="${product.id}">Add to Cart</button>
                    </div>
                `);
            });
}

// Función para agregar producto al carrito
function addToCart(productId) {
    const cart = JSON.parse(localStorage.getItem("cart")) || [];
    cart.push(productId);
    localStorage.setItem("cart", JSON.stringify(cart));
    updateCartCount();
}

// Función para actualizar el contador del carrito
function updateCartCount() {
    const cartCount = document.getElementById("cartCount");
    cartCount.textContent = JSON.parse(localStorage.getItem("cart") || "[]").length;
}

// Función para manejar el clic en el botón "Add to Cart"
document.addEventListener("click", function(event) {
    if (event.target.classList.contains("add-to-cart")) {
        const productId = parseInt(event.target.dataset.id);
        addToCart(productId);
    }
});

// Función para mostrar/ocultar el menú hamburguesa en resoluciones móviles
document.querySelector(".menu-toggle").addEventListener("click", function() {
    document.querySelector(".nav-links").classList.toggle("show");
});

// Función para manejar el cambio de categoría en el filtro de productos
document.getElementById("category").addEventListener("change", function(event) {
    const category = event.target.value;
    renderProducts(category);
});

// Renderizar productos al cargar la página
renderProducts();
updateCartCount();

// Función para validar el formulario de contacto
function validateContactForm(event) {
    event.preventDefault(); // Evitar que se envíe el formulario automáticamente

    const nameInput = document.getElementById("name");
    const emailInput = document.getElementById("email");
    const messageInput = document.getElementById("message");
    const contactMessage = document.getElementById("contactMessage");

    if (!nameInput.value.trim() || !emailInput.value.trim() || !messageInput.value.trim()) {
        contactMessage.textContent = "Please fill in all fields.";
        contactMessage.style.color = "red";
        return false;
    }

    // Aquí podrías enviar el formulario a través de AJAX o hacer cualquier otra acción necesaria

    contactMessage.textContent = "Message sent successfully!";
    contactMessage.style.color = "green";
    return true;
}

// Escuchar el evento 'submit' del formulario de contacto
document.getElementById("contactForm").addEventListener("submit", validateContactForm);
});
