<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>The Anmol Shop</title>
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@700;900&family=Inter:wght@400;500;600&display=swap" rel="stylesheet"/>
<script src="https://cdn.jsdelivr.net/npm/@emailjs/browser@4/dist/email.min.js"></script>
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  :root {
    --green: #2d6a4f;
    --green-light: #40916c;
    --green-pale: #d8f3dc;
    --gold: #f4a261;
    --dark: #1b1b1b;
    --gray: #6b7280;
    --light: #f9fafb;
    --white: #ffffff;
    --radius: 14px;
    --shadow: 0 4px 24px rgba(0,0,0,0.10);
  }

  html { scroll-behavior: smooth; }
  body { font-family: 'Inter', sans-serif; background: var(--white); color: var(--dark); overflow-x: hidden; }

  /* ── NAVBAR ── */
  nav {
    position: fixed; top: 0; left: 0; right: 0; z-index: 100;
    display: flex; align-items: center; justify-content: space-between;
    padding: 14px 24px;
    background: rgba(255,255,255,0.92);
    backdrop-filter: blur(12px);
    border-bottom: 1px solid rgba(45,106,79,0.1);
    box-shadow: 0 2px 16px rgba(0,0,0,0.06);
  }
  .nav-logo { font-family: 'Playfair Display', serif; font-size: 1.4rem; color: var(--green); font-weight: 900; letter-spacing: -0.5px; }
  .nav-logo span { color: var(--gold); }
  .nav-cart-btn {
    position: relative; background: var(--green); color: white;
    border: none; border-radius: 50px; padding: 10px 20px;
    font-size: 0.9rem; font-weight: 600; cursor: pointer;
    display: flex; align-items: center; gap: 8px; transition: background 0.2s;
  }
  .nav-cart-btn:hover { background: var(--green-light); }
  #cart-count {
    background: var(--gold); color: var(--dark); border-radius: 50%;
    width: 20px; height: 20px; font-size: 0.72rem; font-weight: 700;
    display: flex; align-items: center; justify-content: center;
    display: none;
  }

  /* ── HERO ── */
  .hero {
    margin-top: 60px;
    position: relative; width: 100%; height: 92vh; min-height: 520px;
    overflow: hidden; display: flex; align-items: center; justify-content: center;
  }
  .hero-img {
    position: absolute; inset: 0; width: 100%; height: 100%;
    object-fit: cover; object-position: center;
  }
  .hero-overlay {
    position: absolute; inset: 0;
    background: linear-gradient(135deg, rgba(27,27,27,0.62) 0%, rgba(45,106,79,0.45) 100%);
  }
  .hero-content {
    position: relative; z-index: 2; text-align: center; padding: 0 20px;
    max-width: 700px;
  }
  .hero-eyebrow {
    display: inline-block; background: var(--gold); color: var(--dark);
    font-size: 0.78rem; font-weight: 700; letter-spacing: 2px; text-transform: uppercase;
    padding: 5px 14px; border-radius: 50px; margin-bottom: 18px;
  }
  .hero-title {
    font-family: 'Playfair Display', serif; font-size: clamp(2.4rem, 7vw, 4.2rem);
    color: white; font-weight: 900; line-height: 1.08; margin-bottom: 14px;
  }
  .hero-title span { color: var(--gold); }
  .hero-tagline {
    font-size: clamp(1rem, 2.5vw, 1.2rem); color: rgba(255,255,255,0.88);
    margin-bottom: 34px; font-weight: 400; letter-spacing: 0.3px;
  }
  .hero-btn {
    background: var(--gold); color: var(--dark); border: none;
    padding: 16px 38px; border-radius: 50px; font-size: 1rem; font-weight: 700;
    cursor: pointer; transition: transform 0.2s, box-shadow 0.2s;
    box-shadow: 0 6px 24px rgba(244,162,97,0.4); letter-spacing: 0.3px;
  }
  .hero-btn:hover { transform: translateY(-2px); box-shadow: 0 10px 32px rgba(244,162,97,0.5); }

  /* ── PRODUCTS SECTION ── */
  #products { padding: 60px 0 70px; background: var(--light); }
  .section-header { text-align: center; margin-bottom: 36px; padding: 0 20px; }
  .section-eyebrow {
    display: inline-block; color: var(--green); font-size: 0.78rem;
    font-weight: 700; letter-spacing: 2px; text-transform: uppercase; margin-bottom: 10px;
  }
  .section-title {
    font-family: 'Playfair Display', serif; font-size: clamp(1.7rem, 4vw, 2.4rem);
    font-weight: 900; color: var(--dark);
  }

  /* Carousel */
  .carousel-wrapper { position: relative; overflow: hidden; padding: 10px 0 20px; }
  .carousel-track {
    display: flex; gap: 20px; padding: 0 24px;
    overflow-x: auto; scroll-snap-type: x mandatory;
    -webkit-overflow-scrolling: touch;
    scrollbar-width: none;
    animation: autoScroll 30s linear infinite;
  }
  .carousel-track:hover { animation-play-state: paused; }
  .carousel-track::-webkit-scrollbar { display: none; }

  .product-card {
    flex: 0 0 240px; scroll-snap-align: start;
    background: var(--white); border-radius: var(--radius);
    box-shadow: var(--shadow); overflow: hidden;
    transition: transform 0.25s, box-shadow 0.25s;
    display: flex; flex-direction: column;
  }
  .product-card:hover { transform: translateY(-5px); box-shadow: 0 12px 32px rgba(0,0,0,0.14); }
  .product-img-wrap { width: 100%; height: 180px; overflow: hidden; background: #f0f0f0; }
  .product-img-wrap img { width: 100%; height: 100%; object-fit: cover; transition: transform 0.4s; }
  .product-card:hover .product-img-wrap img { transform: scale(1.06); }
  .product-info { padding: 14px 14px 16px; flex: 1; display: flex; flex-direction: column; gap: 6px; }
  .product-name { font-size: 0.95rem; font-weight: 600; color: var(--dark); line-height: 1.3; }
  .product-desc { font-size: 0.78rem; color: var(--gray); line-height: 1.4; }
  .product-price { font-size: 1.1rem; font-weight: 700; color: var(--green); margin-top: 4px; }
  .product-price span { font-size: 0.78rem; color: var(--gray); font-weight: 400; text-decoration: line-through; margin-left: 5px; }
  .add-cart-btn {
    margin-top: auto; background: var(--green); color: white;
    border: none; border-radius: 8px; padding: 10px;
    font-size: 0.88rem; font-weight: 600; cursor: pointer;
    transition: background 0.2s; width: 100%;
  }
  .add-cart-btn:hover { background: var(--green-light); }
  .add-cart-btn.added { background: var(--gold); color: var(--dark); }

  /* ── CART DRAWER ── */
  .cart-overlay {
    position: fixed; inset: 0; background: rgba(0,0,0,0.45);
    z-index: 200; opacity: 0; pointer-events: none; transition: opacity 0.3s;
  }
  .cart-overlay.open { opacity: 1; pointer-events: all; }
  .cart-drawer {
    position: fixed; top: 0; right: -100%; bottom: 0;
    width: min(400px, 100vw); background: var(--white);
    z-index: 201; transition: right 0.35s cubic-bezier(.4,0,.2,1);
    display: flex; flex-direction: column; box-shadow: -8px 0 40px rgba(0,0,0,0.15);
  }
  .cart-drawer.open { right: 0; }
  .cart-header {
    padding: 20px 20px 16px; border-bottom: 1px solid #e5e7eb;
    display: flex; align-items: center; justify-content: space-between;
  }
  .cart-header h2 { font-family: 'Playfair Display', serif; font-size: 1.3rem; }
  .cart-close {
    background: #f3f4f6; border: none; border-radius: 50%;
    width: 36px; height: 36px; font-size: 1.1rem; cursor: pointer;
    display: flex; align-items: center; justify-content: center; transition: background 0.2s;
  }
  .cart-close:hover { background: #e5e7eb; }
  .cart-items { flex: 1; overflow-y: auto; padding: 16px 20px; }
  .cart-empty { text-align: center; padding: 40px 20px; color: var(--gray); font-size: 0.95rem; }
  .cart-item {
    display: flex; gap: 12px; align-items: center;
    padding: 12px 0; border-bottom: 1px solid #f3f4f6;
  }
  .cart-item img { width: 54px; height: 54px; object-fit: cover; border-radius: 8px; }
  .cart-item-info { flex: 1; }
  .cart-item-name { font-size: 0.88rem; font-weight: 600; }
  .cart-item-price { font-size: 0.82rem; color: var(--green); font-weight: 600; margin-top: 2px; }
  .cart-item-qty { display: flex; align-items: center; gap: 8px; margin-top: 6px; }
  .qty-btn {
    background: #f3f4f6; border: none; border-radius: 6px;
    width: 26px; height: 26px; font-size: 1rem; cursor: pointer;
    display: flex; align-items: center; justify-content: center; transition: background 0.2s;
  }
  .qty-btn:hover { background: var(--green-pale); }
  .qty-num { font-size: 0.88rem; font-weight: 600; min-width: 20px; text-align: center; }
  .cart-remove { background: none; border: none; color: #ef4444; font-size: 1rem; cursor: pointer; padding: 4px; }
  .cart-footer { padding: 16px 20px 24px; border-top: 1px solid #e5e7eb; }
  .cart-total { display: flex; justify-content: space-between; font-size: 1rem; font-weight: 700; margin-bottom: 14px; }
  .order-btn {
    width: 100%; background: var(--green); color: white;
    border: none; border-radius: 10px; padding: 14px;
    font-size: 0.95rem; font-weight: 700; cursor: pointer; transition: background 0.2s;
  }
  .order-btn:hover { background: var(--green-light); }

  /* ── ORDER FORM MODAL ── */
  .modal-overlay {
    position: fixed; inset: 0; background: rgba(0,0,0,0.5);
    z-index: 300; display: none; align-items: center; justify-content: center; padding: 20px;
  }
  .modal-overlay.open { display: flex; }
  .modal {
    background: var(--white); border-radius: 18px; width: 100%; max-width: 440px;
    box-shadow: 0 20px 60px rgba(0,0,0,0.2); overflow: hidden;
    animation: slideUp 0.3s ease;
  }
  @keyframes slideUp { from { transform: translateY(30px); opacity: 0; } to { transform: translateY(0); opacity: 1; } }
  .modal-header {
    background: var(--green); color: white; padding: 20px 22px;
    display: flex; align-items: center; justify-content: space-between;
  }
  .modal-header h3 { font-family: 'Playfair Display', serif; font-size: 1.2rem; }
  .modal-close-btn {
    background: rgba(255,255,255,0.2); border: none; color: white;
    border-radius: 50%; width: 32px; height: 32px; font-size: 1rem;
    cursor: pointer; display: flex; align-items: center; justify-content: center;
  }
  .modal-body { padding: 22px; }
  .form-group { margin-bottom: 16px; }
  .form-group label { display: block; font-size: 0.82rem; font-weight: 600; color: var(--gray); margin-bottom: 6px; text-transform: uppercase; letter-spacing: 0.5px; }
  .form-group input, .form-group textarea {
    width: 100%; padding: 12px 14px; border: 1.5px solid #e5e7eb;
    border-radius: 10px; font-size: 0.92rem; font-family: 'Inter', sans-serif;
    transition: border-color 0.2s; outline: none; color: var(--dark);
  }
  .form-group input:focus, .form-group textarea:focus { border-color: var(--green); }
  .form-group textarea { resize: vertical; min-height: 80px; }
  .order-summary {
    background: var(--green-pale); border-radius: 10px; padding: 12px 14px; margin-bottom: 16px;
  }
  .order-summary-title { font-size: 0.82rem; font-weight: 700; color: var(--green); margin-bottom: 8px; text-transform: uppercase; letter-spacing: 0.5px; }
  .order-summary-item { font-size: 0.85rem; display: flex; justify-content: space-between; padding: 3px 0; color: var(--dark); }
  .order-summary-total { font-size: 0.95rem; font-weight: 700; color: var(--green); border-top: 1px solid rgba(45,106,79,0.2); margin-top: 8px; padding-top: 8px; display: flex; justify-content: space-between; }
  .send-order-btn {
    width: 100%; background: var(--gold); color: var(--dark);
    border: none; border-radius: 10px; padding: 14px;
    font-size: 1rem; font-weight: 700; cursor: pointer;
    transition: transform 0.2s, box-shadow 0.2s;
    box-shadow: 0 4px 16px rgba(244,162,97,0.35);
  }
  .send-order-btn:hover { transform: translateY(-1px); box-shadow: 0 8px 24px rgba(244,162,97,0.45); }
  .send-order-btn:disabled { opacity: 0.6; cursor: not-allowed; transform: none; }

  /* ── SUCCESS MESSAGE ── */
  .success-msg {
    text-align: center; padding: 30px 20px;
    display: none;
  }
  .success-icon { font-size: 3rem; margin-bottom: 12px; }
  .success-msg h3 { font-family: 'Playfair Display', serif; font-size: 1.4rem; color: var(--green); margin-bottom: 8px; }
  .success-msg p { color: var(--gray); font-size: 0.9rem; line-height: 1.5; }

  /* ── FLOATING BUTTONS ── */
  .float-btns { position: fixed; bottom: 24px; right: 20px; display: flex; flex-direction: column; gap: 12px; z-index: 150; }
  .float-wa {
    background: #25D366; color: white; border: none; border-radius: 50%;
    width: 54px; height: 54px; font-size: 1.5rem; cursor: pointer;
    box-shadow: 0 4px 20px rgba(37,211,102,0.4);
    display: flex; align-items: center; justify-content: center;
    text-decoration: none; transition: transform 0.2s;
  }
  .float-wa:hover { transform: scale(1.1); }

  /* ── TOAST ── */
  .toast {
    position: fixed; bottom: 90px; left: 50%; transform: translateX(-50%) translateY(20px);
    background: var(--dark); color: white; padding: 10px 22px;
    border-radius: 50px; font-size: 0.88rem; font-weight: 500;
    opacity: 0; pointer-events: none; transition: all 0.3s; z-index: 400;
    white-space: nowrap;
  }
  .toast.show { opacity: 1; transform: translateX(-50%) translateY(0); }

  /* ── FOOTER ── */
  footer {
    background: var(--dark); color: rgba(255,255,255,0.6);
    text-align: center; padding: 28px 20px; font-size: 0.82rem; line-height: 1.8;
  }
  footer a { color: var(--gold); text-decoration: none; }

  @media (max-width: 480px) {
    .product-card { flex: 0 0 200px; }
    .product-img-wrap { height: 150px; }
  }
</style>
</head>
<body>

<!-- NAVBAR -->
<nav>
  <div class="nav-logo">The <span>Anmol</span> Shop</div>
  <button class="nav-cart-btn" onclick="openCart()">
    🛒 Cart
    <span id="cart-count">0</span>
  </button>
</nav>

<!-- HERO -->
<section class="hero">
  <img class="hero-img" src="https://images.unsplash.com/photo-1556742049-0cfed4f6a45d?w=1600&q=80" alt="The Anmol Shop" onerror="this.src='https://images.unsplash.com/photo-1604719312566-8912e9227c6a?w=1600&q=80'"/>
  <div class="hero-overlay"></div>
  <div class="hero-content">
    <div class="hero-eyebrow">Welcome to Our Store</div>
    <h1 class="hero-title">The <span>Anmol</span> Shop</h1>
    <p class="hero-tagline">Fresh Finds, Best Prices</p>
    <button class="hero-btn" onclick="document.getElementById('products').scrollIntoView({behavior:'smooth'})">Shop Now ↓</button>
  </div>
</section>

<!-- PRODUCTS -->
<section id="products">
  <div class="section-header">
    <div class="section-eyebrow">Our Collection</div>
    <h2 class="section-title">Fresh Arrivals</h2>
  </div>
  <div class="carousel-wrapper">
    <div class="carousel-track" id="carousel">
      <!-- Cards injected by JS -->
    </div>
  </div>
</section>

<!-- CART DRAWER -->
<div class="cart-overlay" id="cart-overlay" onclick="closeCart()"></div>
<div class="cart-drawer" id="cart-drawer">
  <div class="cart-header">
    <h2>🛒 Your Cart</h2>
    <button class="cart-close" onclick="closeCart()">✕</button>
  </div>
  <div class="cart-items" id="cart-items-list">
    <div class="cart-empty">Your cart is empty.<br/>Add some items to get started!</div>
  </div>
  <div class="cart-footer" id="cart-footer" style="display:none">
    <div class="cart-total">
      <span>Total</span>
      <span id="cart-total-price">₹0</span>
    </div>
    <button class="order-btn" onclick="openOrderModal()">📦 Place Order</button>
  </div>
</div>

<!-- ORDER FORM MODAL -->
<div class="modal-overlay" id="order-modal">
  <div class="modal">
    <div class="modal-header">
      <h3>📦 Complete Your Order</h3>
      <button class="modal-close-btn" onclick="closeOrderModal()">✕</button>
    </div>
    <div class="modal-body" id="modal-body">
      <div class="form-group">
        <label>Your Name *</label>
        <input type="text" id="customer-name" placeholder="Enter your full name"/>
      </div>
      <div class="form-group">
        <label>Phone Number *</label>
        <input type="tel" id="customer-phone" placeholder="e.g. 9876543210"/>
      </div>
      <div class="form-group">
        <label>Delivery Address</label>
        <textarea id="customer-address" placeholder="House/Flat no., Street, Area, City..."></textarea>
      </div>
      <div class="order-summary" id="order-summary-box">
        <div class="order-summary-title">Order Summary</div>
        <div id="order-summary-items"></div>
        <div class="order-summary-total">
          <span>Total</span>
          <span id="order-summary-total">₹0</span>
        </div>
      </div>
      <button class="send-order-btn" id="send-btn" onclick="sendOrder()">✉️ Send Order to Shop</button>
    </div>
    <div class="success-msg" id="success-msg">
      <div class="success-icon">✅</div>
      <h3>Order Sent!</h3>
      <p>Your order has been sent to <strong>The Anmol Shop</strong>.<br/>We'll contact you on your phone number soon!</p>
    </div>
  </div>
</div>

<!-- FLOATING BUTTONS -->
<div class="float-btns">
  <a class="float-wa" href="https://wa.me/7251040835" target="_blank" title="Chat on WhatsApp">
    <svg width="26" height="26" viewBox="0 0 24 24" fill="white"><path d="M17.472 14.382c-.297-.149-1.758-.867-2.03-.967-.273-.099-.471-.148-.67.15-.197.297-.767.966-.94 1.164-.173.199-.347.223-.644.075-.297-.15-1.255-.463-2.39-1.475-.883-.788-1.48-1.761-1.653-2.059-.173-.297-.018-.458.13-.606.134-.133.298-.347.446-.52.149-.174.198-.298.298-.497.099-.198.05-.371-.025-.52-.075-.149-.669-1.612-.916-2.207-.242-.579-.487-.5-.669-.51-.173-.008-.371-.01-.57-.01-.198 0-.52.074-.792.372-.272.297-1.04 1.016-1.04 2.479 0 1.462 1.065 2.875 1.213 3.074.149.198 2.096 3.2 5.077 4.487.709.306 1.262.489 1.694.625.712.227 1.36.195 1.871.118.571-.085 1.758-.719 2.006-1.413.248-.694.248-1.289.173-1.413-.074-.124-.272-.198-.57-.347m-5.421 7.403h-.004a9.87 9.87 0 01-5.031-1.378l-.361-.214-3.741.982.998-3.648-.235-.374a9.86 9.86 0 01-1.51-5.26c.001-5.45 4.436-9.884 9.888-9.884 2.64 0 5.122 1.03 6.988 2.898a9.825 9.825 0 012.893 6.994c-.003 5.45-4.437 9.884-9.885 9.884m8.413-18.297A11.815 11.815 0 0012.05 0C5.495 0 .16 5.335.157 11.892c0 2.096.547 4.142 1.588 5.945L.057 24l6.305-1.654a11.882 11.882 0 005.683 1.448h.005c6.554 0 11.89-5.335 11.893-11.893a11.821 11.821 0 00-3.48-8.413z"/></svg>
  </a>
</div>

<!-- TOAST -->
<div class="toast" id="toast"></div>

<!-- FOOTER -->
<footer>
  <strong style="color:white;font-family:'Playfair Display',serif;font-size:1.1rem;">The Anmol Shop</strong><br/>
  Fresh Finds, Best Prices<br/>
  📧 <a href="mailto:anmolphondani09@gmail.com">anmolphondani09@gmail.com</a> &nbsp;|&nbsp;
  📞 <a href="tel:7251040835">7251040835</a><br/>
  <small style="opacity:0.5;font-size:0.75rem;">© 2025 The Anmol Shop. All rights reserved.</small>
</footer>

<script>
// ── EMAILJS INIT ──────────────────────────────────────────────
emailjs.init("YOUR_EMAILJS_PUBLIC_KEY"); // Replace with your EmailJS public key

// ── PRODUCTS DATA ─────────────────────────────────────────────
const products = [
  { id:1, name:"Basmati Rice (5kg)", desc:"Premium long-grain, aromatic", price:320, mrp:380, img:"https://i.ibb.co/jsG5jhg/17813318193714533121593482628131.jpg" },
  { id:2, name:"Cooking Oil (1L)", desc:"Pure refined sunflower oil", price:145, mrp:165, img:"https://i.ibb.co/jsG5jhg/17813318193714533121593482628131.jpg" },
  { id:3, name:"Toor Dal (1kg)", desc:"Fresh split pigeon peas", price:140, mrp:160, img:"https://i.ibb.co/jsG5jhg/17813318193714533121593482628131.jpg" },
  { id:4, name:"Whole Wheat Flour (5kg)", desc:"Stone-ground atta", price:225, mrp:260, img:"https://i.ibb.co/jsG5jhg/17813318193714533121593482628131.jpg" },
  { id:5, name:"Sugar (1kg)", desc:"Fine grain white sugar", price:48, mrp:55, img:"https://i.ibb.co/jsG5jhg/17813318193714533121593482628131.jpg" },
  { id:6, name:"Detergent Powder (1kg)", desc:"Powerful stain remover", price:89, mrp:105, img:"https://i.ibb.co/jsG5jhg/17813318193714533121593482628131.jpg" },
  { id:7, name:"Masala Pack (Combo)", desc:"5 essential spice mix packs", price:175, mrp:210, img:"https://i.ibb.co/jsG5jhg/17813318193714533121593482628131.jpg" },
  { id:8, name:"Tea (250g)", desc:"Strong Assam blend", price:95, mrp:115, img:"https://i.ibb.co/jsG5jhg/17813318193714533121593482628131.jpg" },
];

// ── CART STATE ────────────────────────────────────────────────
let cart = {};

// ── RENDER CAROUSEL ───────────────────────────────────────────
function renderCarousel() {
  const track = document.getElementById('carousel');
  // Duplicate for infinite scroll feel
  const allProducts = [...products, ...products];
  track.innerHTML = allProducts.map(p => `
    <div class="product-card">
      <div class="product-img-wrap">
        <img src="${p.img}" alt="${p.name}" loading="lazy"
          onerror="this.src='https://via.placeholder.com/240x180/d8f3dc/2d6a4f?text=Product'"/>
      </div>
      <div class="product-info">
        <div class="product-name">${p.name}</div>
        <div class="product-desc">${p.desc}</div>
        <div class="product-price">₹${p.price} <span>₹${p.mrp}</span></div>
        <button class="add-cart-btn" onclick="addToCart(${p.id}, this)">+ Add to Cart</button>
      </div>
    </div>
  `).join('');
}

// ── CART FUNCTIONS ────────────────────────────────────────────
function addToCart(id, btn) {
  const product = products.find(p => p.id === id);
  if (!product) return;
  if (cart[id]) { cart[id].qty++; }
  else { cart[id] = { ...product, qty: 1 }; }
  btn.textContent = "✓ Added!";
  btn.classList.add('added');
  setTimeout(() => { btn.textContent = "+ Add to Cart"; btn.classList.remove('added'); }, 1500);
  updateCartUI();
  showToast(`${product.name} added to cart!`);
}

function updateCartUI() {
  const items = Object.values(cart);
  const totalQty = items.reduce((s, i) => s + i.qty, 0);
  const totalPrice = items.reduce((s, i) => s + i.price * i.qty, 0);

  const countEl = document.getElementById('cart-count');
  countEl.style.display = totalQty > 0 ? 'flex' : 'none';
  countEl.textContent = totalQty;

  const listEl = document.getElementById('cart-items-list');
  const footerEl = document.getElementById('cart-footer');
  document.getElementById('cart-total-price').textContent = `₹${totalPrice}`;

  if (items.length === 0) {
    listEl.innerHTML = '<div class="cart-empty">Your cart is empty.<br/>Add some items to get started!</div>';
    footerEl.style.display = 'none';
    return;
  }
  footerEl.style.display = 'block';
  listEl.innerHTML = items.map(item => `
    <div class="cart-item">
      <img src="${item.img}" alt="${item.name}" onerror="this.src='https://via.placeholder.com/54x54/d8f3dc/2d6a4f?text=P'"/>
      <div class="cart-item-info">
        <div class="cart-item-name">${item.name}</div>
        <div class="cart-item-price">₹${item.price} each</div>
        <div class="cart-item-qty">
          <button class="qty-btn" onclick="changeQty(${item.id}, -1)">−</button>
          <span class="qty-num">${item.qty}</span>
          <button class="qty-btn" onclick="changeQty(${item.id}, 1)">+</button>
        </div>
      </div>
      <button class="cart-remove" onclick="removeItem(${item.id})" title="Remove">🗑️</button>
    </div>
  `).join('');
}

function changeQty(id, delta) {
  if (!cart[id]) return;
  cart[id].qty += delta;
  if (cart[id].qty <= 0) delete cart[id];
  updateCartUI();
}

function removeItem(id) {
  delete cart[id];
  updateCartUI();
}

function openCart() {
  document.getElementById('cart-drawer').classList.add('open');
  document.getElementById('cart-overlay').classList.add('open');
  document.body.style.overflow = 'hidden';
}

function closeCart() {
  document.getElementById('cart-drawer').classList.remove('open');
  document.getElementById('cart-overlay').classList.remove('open');
  document.body.style.overflow = '';
}

// ── ORDER MODAL ───────────────────────────────────────────────
function openOrderModal() {
  closeCart();
  const items = Object.values(cart);
  const total = items.reduce((s, i) => s + i.price * i.qty, 0);

  document.getElementById('order-summary-items').innerHTML = items.map(i =>
    `<div class="order-summary-item"><span>${i.name} x${i.qty}</span><span>₹${i.price * i.qty}</span></div>`
  ).join('');
  document.getElementById('order-summary-total').textContent = `₹${total}`;

  document.getElementById('modal-body').style.display = 'block';
  document.getElementById('success-msg').style.display = 'none';
  document.getElementById('order-modal').classList.add('open');
}

function closeOrderModal() {
  document.getElementById('order-modal').classList.remove('open');
}

// ── SEND ORDER via EMAILJS ─────────────────────────────────────
function sendOrder() {
  const name = document.getElementById('customer-name').value.trim();
  const phone = document.getElementById('customer-phone').value.trim();
  const address = document.getElementById('customer-address').value.trim();

  if (!name) { showToast('Please enter your name'); return; }
  if (!phone || !/^\d{10}$/.test(phone)) { showToast('Enter a valid 10-digit phone number'); return; }

  const items = Object.values(cart);
  const total = items.reduce((s, i) => s + i.price * i.qty, 0);
  const itemsList = items.map(i => `• ${i.name} x${i.qty} = ₹${i.price * i.qty}`).join('\n');

  const btn = document.getElementById('send-btn');
  btn.disabled = true;
  btn.textContent = 'Sending...';

  const templateParams = {
    to_email: 'anmolphondani09@gmail.com',
    customer_name: name,
    customer_phone: phone,
    customer_address: address || 'Not provided',
    order_items: itemsList,
    order_total: `₹${total}`,
    shop_name: 'The Anmol Shop'
  };

  // EmailJS send — replace 'YOUR_SERVICE_ID' and 'YOUR_TEMPLATE_ID' with your EmailJS IDs
  emailjs.send('YOUR_SERVICE_ID', 'YOUR_TEMPLATE_ID', templateParams)
    .then(() => {
      showSuccess();
      cart = {};
      updateCartUI();
    })
    .catch(() => {
      // Fallback: open mailto if EmailJS not configured
      const subject = encodeURIComponent(`New Order from ${name} - The Anmol Shop`);
      const body = encodeURIComponent(`New Order Details:\n\nCustomer: ${name}\nPhone: ${phone}\nAddress: ${address || 'Not provided'}\n\nItems:\n${itemsList}\n\nTotal: ₹${total}`);
      window.open(`mailto:anmolphondani09@gmail.com?subject=${subject}&body=${body}`);
      showSuccess();
      cart = {};
      updateCartUI();
    })
    .finally(() => {
      btn.disabled = false;
      btn.textContent = '✉️ Send Order to Shop';
    });
}

function showSuccess() {
  document.getElementById('modal-body').style.display = 'none';
  document.getElementById('success-msg').style.display = 'block';
  setTimeout(closeOrderModal, 3500);
}

// ── TOAST ─────────────────────────────────────────────────────
function showToast(msg) {
  const t = document.getElementById('toast');
  t.textContent = msg;
  t.classList.add('show');
  setTimeout(() => t.classList.remove('show'), 2200);
}

// ── INIT ──────────────────────────────────────────────────────
renderCarousel();
</script>
</body>
</html>
