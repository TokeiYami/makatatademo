let cart = [];

function addToCart(name, price) {
  const existing = cart.find(item => item.name === name);
  if (existing) {
    existing.qty++;
  } else {
    cart.push({ name, price, qty: 1 });
  }
  renderCart();
}

function removeFromCart(name) {
  cart = cart.filter(item => item.name !== name);
  renderCart();
}

function changeQty(name, delta) {
  const item = cart.find(i => i.name === name);
  if (!item) return;
  item.qty += delta;
  if (item.qty <= 0) removeFromCart(name);
  renderCart();
}

function renderCart() {
  const container = document.getElementById('cart-items');
  container.innerHTML = '';
  let total = 0;
  cart.forEach(item => {
    total += item.price * item.qty;
    const div = document.createElement('div');
    div.className = 'cart-item';
    div.innerHTML = `
      <div class="cart-item-name">${item.name} x ${item.qty}</div>
      <div>₱${item.price * item.qty}.00</div>
      <div>
        <button onclick="changeQty('${item.name}', -1)">−</button>
        <button onclick="changeQty('${item.name}', 1)">+</button>
      </div>
    `;
    container.appendChild(div);
  });
  document.getElementById('total').innerText = `Total: ₱${total}.00`;
}

function checkout() {
  if (cart.length === 0) {
    alert('Your cart is empty!');
    return;
  }
  alert('Thank you for your order!');
  cart = [];
  renderCart();
}
