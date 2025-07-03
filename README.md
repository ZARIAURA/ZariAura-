<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>ZariAura – Premium Clothing</title>
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600&display=swap" rel="stylesheet">
  <style>
    *,*::before,*::after{margin:0;padding:0;box-sizing:border-box;}
    body{font-family:'Poppins',sans-serif;background:#fff;color:#333;}
    img{max-width:100%;display:block;margin:auto;}
    header{background:#000;color:#fff;text-align:center;padding:25px 15px;}
    header h1{font-size:2rem;font-weight:600;}
    header p{font-size:0.9rem;opacity:0.8;}
    nav{background:#111;display:flex;justify-content:center;gap:25px;padding:12px 10px;flex-wrap:wrap;}
    nav a{color:#fff;text-decoration:none;font-weight:600;cursor:pointer;}
    nav a:hover{opacity:.7;}
    .banner{width:100%;height:300px;background:url('https://via.placeholder.com/1200x300/000000/FFFFFF?text=ZariAura+Collections') center/cover no-repeat;}
    .section-title{text-align:center;font-size:1.7rem;margin:35px 0 20px;font-weight:600;}
    .products{display:grid;grid-template-columns:repeat(auto-fit,minmax(210px,1fr));gap:24px;padding:0 20px;}
    .product{border:1px solid #ddd;border-radius:10px;padding:15px;text-align:center;transition:.2s;}
    .product:hover{box-shadow:0 4px 12px rgba(0,0,0,.1);}
    .product h3{font-size:1.05rem;margin:10px 0 6px;}
    .product p.price{font-weight:600;margin-top:2px;}
    .product p.desc{font-size:0.85rem;color:#666;min-height:34px;}
    .product-images{display:flex;gap:6px;overflow-x:auto;margin-bottom:10px;scroll-snap-type:x mandatory;justify-content:center;}
    .product-images img{height:160px;border-radius:8px;}
    .add-to-cart,.buy-now{background:#000;color:#fff;border:none;border-radius:5px;padding:9px 18px;cursor:pointer;margin:5px;}
    .add-to-cart:hover,.buy-now:hover{opacity:.85;}
    footer{background:#000;color:#fff;text-align:center;padding:22px 15px;margin-top:50px;font-size:0.85rem;}
    .admin-wrapper{max-width:550px;margin:60px auto;border:2px dashed #999;border-radius:12px;padding:25px 30px;}
    .admin-wrapper h2{text-align:center;margin-bottom:20px;}
    .admin-wrapper input,.admin-wrapper textarea{width:100%;padding:10px 12px;margin-bottom:12px;border:1px solid #ccc;border-radius:6px;}
    .admin-wrapper button{width:100%;padding:11px;background:#111;color:#fff;border:none;border-radius:6px;cursor:pointer;}
    .msg-error{color:#e53935;font-size:0.85rem;text-align:center;margin-top:-6px;}
    .modal-overlay{position:fixed;inset:0;background:rgba(0,0,0,.6);display:none;align-items:center;justify-content:center;z-index:999;}
    .modal{background:#fff;padding:25px;border-radius:12px;max-width:400px;width:90%;}
    .modal input,.modal textarea,.modal select{width:100%;padding:8px 10px;margin-bottom:10px;border:1px solid #ccc;border-radius:6px;}
    .modal button{padding:10px 16px;background:#111;color:#fff;border:none;border-radius:6px;cursor:pointer;}
    .modal .close-btn{background:#e53935;float:right;}
    #statusModal .modal{text-align:center;}
    #statusMessage{margin-bottom:18px;font-size:1.05rem;}
  </style>
</head>
<body>
  <header>
    <h1>ZariAura</h1>
    <p>Premium Streetwear & Ethnic Fashion</p>
  </header>

  <nav>
    <a href="#">Home</a>
    <a href="#">New Arrivals</a>
    <a href="#">Men</a>
    <a href="#">Women</a>
    <a onclick="openCart()">Cart 🛒<span id="cart-count">0</span></a>
  </nav>

  <div class="banner"></div>
  <div class="section-title">🔥 Featured Products</div>
  <div class="products" id="product-list"></div>

  <!-- Admin Panel -->
  <div class="admin-wrapper">
    <h2>🛠️ Admin Panel</h2>
    <div id="admin-login">
      <input type="password" id="admin-password" placeholder="Enter admin password"/>
      <button onclick="checkAdmin()">Login</button>
      <p id="admin-msg" class="msg-error"></p>
    </div>

    <div id="product-form" style="display:none;">
      <input type="text" id="p-title" placeholder="Product title"/>
      <input type="number" id="p-price" placeholder="Price (e.g. 799)"/>
      <input type="file" id="p-image" accept="image/*"/>
      <textarea id="p-desc" rows="3" placeholder="Short description"></textarea>
      <div style="margin-bottom:10px;">
        <label><input type="checkbox" class="size-option" value="S"/> S</label>
        <label><input type="checkbox" class="size-option" value="M"/> M</label>
        <label><input type="checkbox" class="size-option" value="L"/> L</label>
        <label><input type="checkbox" class="size-option" value="XL"/> XL</label>
        <label><input type="checkbox" class="size-option" value="XXL"/> XXL</label>
      </div>
      <button onclick="addProduct()">Publish Product</button>
    </div>
  </div>

  <!-- Cart Modal -->
  <div class="modal-overlay" id="cartModal">
    <div class="modal">
      <button class="close-btn" onclick="closeCart()">X</button>
      <h3>Your Cart</h3>
      <div id="cart-items"></div>
      <p id="cart-total" style="font-weight:bold;margin:10px 0;"></p>
      <button onclick="checkoutAll()">Order All</button>
    </div>
  </div>

  <!-- Buy Now / Checkout Modal -->
  <div class="modal-overlay" id="buyNowModal">
    <div class="modal">
      <button class="close-btn" onclick="closeBuyNow()">X</button>
      <h3 id="modalProductTitle">Buy Now</h3>
      <label><strong>Select Size *</strong></label>
      <select id="selected-size" required></select>
      <input type="text" id="cust-name" placeholder="Your Name"/>
      <input type="tel" id="cust-phone" placeholder="Phone Number"/>
      <textarea id="user-address" placeholder="Address"></textarea>
      <input type="text" id="house-number" placeholder="House No. / Flat"/>
      <input type="text" id="pin-code" placeholder="PIN Code"/>
      <button onclick="placeOrder()">Place Order</button>
    </div>
  </div>

  <!-- Status Modal -->
  <div class="modal-overlay" id="statusModal">
    <div class="modal">
      <h3 id="statusMessage"></h3>
      <button onclick="closeStatus()">OK</button>
    </div>
  </div>

  <footer>© 2025 ZariAura | Follow us @z_a_r_i_a_u_r_a</footer>

  <!-- =========================  SCRIPT  ========================= -->
  <script>
    const cart = [];
    const ADMIN_PW = "zariaura123";

    /* ------ Load saved products from localStorage ------ */
    const savedProducts = JSON.parse(localStorage.getItem("products") || "[]");
    savedProducts.forEach(html => document.getElementById("product-list").innerHTML += html);

    /* ------ Utility: Status modal ------ */
    function showStatus(msg) {
      document.getElementById("statusMessage").innerText = msg;
      document.getElementById("statusModal").style.display = "flex";
    }
    function closeStatus() { document.getElementById("statusModal").style.display = "none"; }

    /* ------ Admin login ------ */
    function checkAdmin() {
      if (document.getElementById("admin-password").value.trim() === ADMIN_PW) {
        document.getElementById("admin-login").style.display = "none";
        document.getElementById("product-form").style.display = "block";
      } else {
        document.getElementById("admin-msg").innerText = "❌ Incorrect Password!";
      }
    }

    /* ------ Publish new product ------ */
    function addProduct() {
      const title  = document.getElementById("p-title").value.trim();
      const price  = parseFloat(document.getElementById("p-price").value);
      const imgFile = document.getElementById("p-image").files[0];
      const desc   = document.getElementById("p-desc").value.trim();
      const sizes  = [...document.querySelectorAll(".size-option:checked")].map(el => el.value).join(", ");

      if (!title || isNaN(price) || !imgFile) { showStatus("❌ Enter title, price & image"); return; }

      const reader = new FileReader();
      reader.onload = () => {
        const card = document.createElement("div");
        card.className = "product";
        card.innerHTML = `
          <div class="product-images"><img src="${reader.result}" alt="${title}"></div>
          <h3>${title}</h3>
          <p class="desc">${desc}</p>
          <p class="price">₹${price}</p>
          <p><strong>Sizes:</strong> ${sizes}</p>
          <button class="add-to-cart" onclick="addToCart('${title}',${price})">Add to Cart</button>
          <button class="buy-now" onclick="openBuyNow('${title}',${price},'${sizes}')">Buy Now</button>`;
        document.getElementById("product-list").appendChild(card);

        /* Persist */
        localStorage.setItem("products", JSON.stringify(document.getElementById("product-list").innerHTML));
        showStatus("✅ Product added!");
      };
      reader.readAsDataURL(imgFile);

      /* Clear form */
      ["p-title","p-price","p-desc","p-image"].forEach(id => document.getElementById(id).value = "");
      document.querySelectorAll(".size-option").forEach(cb => cb.checked = false);
    }

    /* ------ Cart operations ------ */
    function addToCart(title, price) {
      cart.push({title, price});
      document.getElementById("cart-count").innerText = cart.length;
      showStatus("🛒 Added to cart");
    }

    function openCart() {
      const list = document.getElementById("cart-items");
      list.innerHTML = "";
      let total = 0;
      cart.forEach(item => {
        total += item.price;
        list.innerHTML += `<p>${item.title} — ₹${item.price}</p>`;
      });
      document.getElementById("cart-total").innerText = "Total: ₹" + total;
      document.getElementById("cartModal").style.display = "flex";
    }
    function closeCart() { document.getElementById("cartModal").style.display = "none"; }

    /* ------ Checkout All (multi-item order) ------ */
    function checkoutAll() {
      if (cart.length === 0) { showStatus("Cart is empty"); return; }
      const total = cart.reduce((sum, i) => sum + i.price, 0);
      const titles = cart.map(i => i.title).join(", ");               // All product names
      openBuyNow(titles, total, "S,M,L,XL,XXL");                      // Allow size choice
    }

    /* ------ Buy-Now modal ------ */
    function openBuyNow(title, price, sizes) {
      document.getElementById("modalProductTitle").innerText = `Buy Now: ₹${price}`;
      const sel = document.getElementById("selected-size");
      sel.innerHTML = "";
      sizes.split(",").forEach(sz => {
        const opt = document.createElement("option");
        opt.textContent = sz.trim();
        opt.value = sz.trim();
        sel.appendChild(opt);
      });
      const modal = document.getElementById("buyNowModal");
      modal.dataset.price = price;
      modal.dataset.productTitle = title;
      modal.style.display = "flex";
    }
    function closeBuyNow() { document.getElementById("buyNowModal").style.display = "none"; }

    /* ------ Place order (WhatsApp) ------ */
    function placeOrder() {
      const name  = document.getElementById("cust-name").value.trim();
      const phone = document.getElementById("cust-phone").value.trim();
      const addr  = document.getElementById("user-address").value.trim();
      const house = document.getElementById("house-number").value.trim();
      const pin   = document.getElementById("pin-code").value.trim();
      const size  = document.getElementById("selected-size").value;
      const modal = document.getElementById("buyNowModal");
      const price = modal.dataset.price;
      const title = modal.dataset.productTitle;

      if (!name || !phone || !addr || !house || !pin || !size) {
        showStatus("❌ Fill all address fields");
        return;
      }

      const msg  = `*New Order from ZariAura*%0A👕 Product: ${title}%0A💰 Price: ₹${price}%0A📏 Size: ${size}%0A👤 Name: ${name}%0A📞 Phone: ${phone}%0A🏠 Address: ${addr}, ${house}, ${pin}%0A💵 Payment: Cash on Delivery`;
      const phoneNumber = "7428443169";   // your receiving number
      window.open(`https://wa.me/${phoneNumber}?text=${msg}`, "_blank");

      /* reset */
      closeBuyNow();
      closeCart();
      cart.length = 0;
      document.getElementById("cart-count").innerText = "0";
      showStatus("✅ Order Ready — sent to WhatsApp!");
    }
  </script>
</body>
</html>
