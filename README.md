<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Taste of Pie</title>
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <a href="order-history.html" class="order-history-link">📜 View Order History </a>
<h3"Credit- Zayan,Tansih,Kirtan"</h3>
  <div class="app-header">
    <img src="logo.png" alt="Logo" class="logo" />
    <h1 class="brand-name">PayPie</h1>
  </div>

  <div class="card">
    <h2 class="section-title">Customer Billing</h2>
    <form id="billingForm">
      <input type="text" id="name" placeholder="Customer Name" required />

      <div id="itemsContainer">
        <div class="item-row">
          <input type="text" name="itemName" placeholder="Item Name" required />
          <input type="number" name="itemPrice" placeholder="Price (₹)" required min="0" step="0.01" />
          <button type="button" class="removeItemBtn" onclick="removeItem(this)" title="Remove Item" style="display:none;">✖</button>
        </div>
      </div>

      <button type="button" id="addItemBtn">+ Add Another Item</button>
      <br />
      <button type="submit">Continue</button>
    </form>
  </div>

  <script>
    const addItemBtn = document.getElementById("addItemBtn");
    const itemsContainer = document.getElementById("itemsContainer");

    addItemBtn.addEventListener("click", () => {
      const newItem = document.createElement("div");
      newItem.classList.add("item-row");
      newItem.innerHTML = `
        <input type="text" name="itemName" placeholder="Item Name" required />
        <input type="number" name="itemPrice" placeholder="Price (₹)" required min="0" step="0.01" />
        <button type="button" class="removeItemBtn" onclick="removeItem(this)" title="Remove Item">✖</button>
      `;
      itemsContainer.appendChild(newItem);
      updateRemoveButtons();
    });

    function removeItem(btn) {
      btn.parentElement.remove();
      updateRemoveButtons();
    }

    function updateRemoveButtons() {
      const removeBtns = document.querySelectorAll(".removeItemBtn");
      removeBtns.forEach((btn, i) => {
        btn.style.display = removeBtns.length > 1 ? "inline-block" : "none";
      });
    }

    updateRemoveButtons();

    document.getElementById("billingForm").addEventListener("submit", e => {
      e.preventDefault();

      const name = document.getElementById("name").value.trim();
      if (!name) return alert("Please enter customer name.");

      const items = [];
      const itemNames = document.getElementsByName("itemName");
      const itemPrices = document.getElementsByName("itemPrice");

      for (let i = 0; i < itemNames.length; i++) {
        const n = itemNames[i].value.trim();
        const p = parseFloat(itemPrices[i].value);
        if (!n || isNaN(p) || p < 0) {
          return alert("Please enter valid item name and price for all items.");
        }
        items.push({ name: n, price: p });
      }

      localStorage.setItem("name", name);
      localStorage.setItem("items", JSON.stringify(items));

      window.location.href = "summary.html";
    });
  </script>
</body>
</html>
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>PayFast - Card Payment</title>
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <div class="app-header">
    <img src="logo.png" alt="Logo" class="logo" />
    <h1 class="brand-name">PayPie</h1>
  </div>

  <div class="card">
    <button class="back-btn" onclick="goBack()">← Back</button>
    <h2>Pay via Card</h2>
    <p class="amount-big">₹<span id="cardAmount"></span></p>

    <form id="cardForm">
      <input type="text" id="cardNumber" placeholder="Card Number" maxlength="16" required />
      <input type="text" id="expiry" placeholder="MM/YY" maxlength="5" required />
      <input type="password" id="cvv" placeholder="CVV" maxlength="3" required />
      <button type="submit">Pay Now</button>
    </form>
  </div>

  <script>
    document.getElementById("cardAmount").innerText = localStorage.getItem("totalAmount");

    document.getElementById("cardForm").addEventListener("submit", e => {
      e.preventDefault();
      const cardNumber = document.getElementById("cardNumber").value.trim();
      const expiry = document.getElementById("expiry").value.trim();
      const cvv = document.getElementById("cvv").value.trim();

      if (!/^\d{16}$/.test(cardNumber)) {
        alert("Enter a valid 16-digit card number.");
        return;
      }
      if (!/^\d{2}\/\d{2}$/.test(expiry)) {
        alert("Enter expiry date in MM/YY format.");
        return;
      }
      if (!/^\d{3}$/.test(cvv)) {
        alert("Enter a valid 3-digit CVV.");
        return;
      }
      alert("Payment successful via Card!");
      window.location.href = "index.html";
    });

    function goBack() {
      window.location.href = "payment.html";
    }
  </script><div id="loader">
    <img src="loading.gif" alt="Loading..." />
  </div>

</body>
</html>
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>PayFast - Bank Payment</title>
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <div class="app-header">
    <img src="logo.png" alt="Logo" class="logo" />
    <h1 class="brand-name">PayPie</h1>
  </div>

  <div class="card">
    <button class="back-btn" onclick="goBack()">← Back</button>
    <h2>Bank Transfer Details</h2>
    <p class="amount-big">₹<span id="bankAmount"></span></p>

    <div class="bank-box">
      <p><strong>Account Name:</strong> PayFast Pvt Ltd</p>
      <p><strong>Account Number:</strong> 9876543210</p>
      <p><strong>IFSC Code:</strong> PYTM0123456</p>
    </div>

    <img src="bank-icons.png" alt="Supported Banks" class="bank-logos" />
    <p class="upi-note">Use your mobile banking app to complete the transfer.</p>

    <button onclick="done()">Payment Done</button>
  </div>

  <script>
    document.getElementById("bankAmount").innerText = localStorage.getItem("totalAmount");
    function done() {
      alert("Bank transfer completed!");
      window.location.href = "index.html";
    }

    function goBack() {
      window.location.href = "payment.html";
    }
  </script><div id="loader">
    <img src="loading.gif" alt="Loading..." />
  </div>

</body>
</html>
!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Order History</title>
  <link rel="stylesheet" href="style.css" />
  <style>
    .history-container {
      max-width: 600px;
      margin: 20px auto;
      background: #fff;
      padding: 16px;
      border-radius: 12px;
      box-shadow: 0 0 10px #ccc;
    }
    .order-entry {
      padding: 10px;
      border-bottom: 1px solid #eee;
    }
    .order-entry:last-child {
      border-bottom: none;
    }
    .order-entry h3 {
      margin: 0;
      color: #d10000;
    }
    .order-entry small {
      color: gray;
    }
    .back-home {
      margin-top: 20px;
      text-align: center;
    }
    .back-home a {
      text-decoration: none;
      padding: 10px 20px;
      background: #001f7a;
      color: white;
      border-radius: 6px;
    }
  </style>
</head>
<body>
  <div class="history-container">
    <h2>Your Order History</h2>
    <div id="orderList"></div>

    <div class="back-home">
      <a href="index.html">← Back to Home</a>
    </div>
  </div>
  <ul id="orderList"></ul>
  <script>
    const orderList = document.getElementById("orderList");
    const history = JSON.parse(localStorage.getItem("orderHistory") || "[]");

    if (history.length === 0) {
      orderList.innerHTML = "<p>No past orders found.</p>";
    } else {
      history.reverse().forEach(order => {
        const entry = document.createElement("div");
        entry.className = "order-entry";
        entry.innerHTML = `
          <h3>${order.item} - ₹${order.price}</h3>
          <p>Ordered by: ${order.name}</p>
          <small>${order.date}</small>
        `;
        orderList.appendChild(entry);
      });
    }
  </script>
</body>
</html><!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>PayFast - Payment</title>
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <div class="app-header">
    <img src="logo.png" alt="Logo" class="logo" />
    <h1 class="brand-name">PayFast</h1>
  </div>

  <div class="card">
    <h2>Select Payment Method</h2>
    <button class="back-btn" onclick="goBack()">← Back</button>
    <p class="amount-big">₹<span id="amount"></span></p>

    <div class="methods">
      <button onclick="goTo('upi.html')">UPI</button>
      <button onclick="goTo('card.html')">Card Payment</button>
      <button onclick="goTo('bank.html')">Bank Transfer</button>
    </div>
  </div>

  <script>
    document.getElementById("amount").innerText = localStorage.getItem("totalAmount");
    function goTo(page) {
      window.location.href = page;
    }
    function goBack() {
      window.location.href = "summary.html";
    }
  </script>
</body>
</html>
document.getElementById("billingForm")?.addEventListener("submit", function (e) {
  e.preventDefault();
  const name = document.getElementById("name").value;
  const itemName = document.getElementById("itemName").value;
  const price = document.getElementById("itemPrice").value;

  localStorage.setItem("name", name);
  localStorage.setItem("itemName", itemName);
  localStorage.setItem("price", price);

  window.location.href = "summary.html";
});
function goBack() {
  window.location.href = "summary.html";
}
function addToCart(itemName, itemPrice) {
  let cart = JSON.parse(localStorage.getItem("cart")) || [];

  // Check if item already exists
  let existingItem = cart.find(item => item.name === itemName);
  if (existingItem) {
    existingItem.qty += 1;
  } else {
    cart.push({ name: itemName, price: itemPrice, qty: 1 });
  }

  localStorage.setItem("cart", JSON.stringify(cart));
}
@import url('https://fonts.googleapis.com/css2?family=Poppins:ital,wght@1,400;1,700&display=swap');

:root {
  --primary-blue: #1E3A8A;
  --accent-red: #E63946;
  --text-dark: #222;
  --text-light: #fff;
}

body {
  margin: 0;
  font-family: 'Poppins', italic;
  font-weight: 400;
  font-style: italic;
  background: url("BAG.jpg") no-repeat center center fixed;
  background-size: cover;
  color: var(--text-dark);
}

.app-header {
  background-color: var(--primary-blue);
  padding: 16px;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 12px;
  color: var(--text-light);
  font-family: 'Poppins', italic;
  font-weight: 400;
  font-style: italic;
}

.logo {
  width: 80px;
  height: auto;
}

.brand-name {
  font-family: 'Poppins', italic;
  font-weight: 700;
  font-style: italic;
  font-size: 2.5rem;
  letter-spacing: 1px;
  color: var(--text-light);
}

/* Section titles: bold italic */
.section-title {
  font-family: 'Poppins', italic;
  font-weight: 700;          /* Bold */
  font-style: italic;        /* Italic */
  font-size: 1.8rem;
  margin-bottom: 16px;
  color: #FFD700;            /* Gold/yellow color to stand out */
  text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.6); /* subtle shadow for better readability */
}

.card {
  
  background: linear-gradient(135deg, var(--primary-blue), var(--accent-red));
  color: var(--text-light);
  max-width: 440px;
  margin: 48px auto;
  padding: 32px 24px;
  border-radius: 16px;
  box-shadow: 0 6px 20px rgba(0, 0, 0, 0.1);
  font-family: 'Poppins', italic;
  font-weight: 400;
  font-style: italic;
}

/* Inputs */
input[type="text"],
input[type="number"],
input[type="password"] {
  width: 90%;
  max-width: 340px;
  padding: 14px;
  margin: 14px 0;
  border: 1px solid #ccc;
  border-radius: 6px;
  box-sizing: border-box;
  font-family: 'Poppins', italic;
  font-weight: 400;
  font-style: italic;
}

/* Buttons */
button {
  background-color: var(--accent-red);
  color: var(--text-light);
  padding: 14px 28px;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-weight: 400;
  font-style: italic;
  font-size: 1rem;
  margin-top: 24px;
  font-family: 'Poppins', italic;
  transition: background 0.2s;
}

button:hover {
  background-color: #c12e3c;
}

/* Back button */
.back-btn {
  background-color: transparent;
  color: var(--primary-blue);
  border: none;
  font-size: 1rem;
  cursor: pointer;
  margin-bottom: 18px;
  text-align: left;
  font-family: 'Poppins', italic;
  font-weight: 400;
  font-style: italic;
}

.back-btn:hover {
  text-decoration: underline;
}

/* Payment amount */
.amount-big {
  font-family: 'Poppins', italic;
  font-weight: 700;
  font-style: italic;
  font-size: 2.4rem;
  margin-bottom: 20px;
  color: var(--text-light);
}

/* Item entry row */
.item-row {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin: 14px 0;
  gap: 10px;
}

.item-row input[type="text"] {
  flex: 2;
}

.item-row input[type="number"] {
  flex: 1;
}

.removeItemBtn {
  background-color: var(--accent-red);
  color: var(--text-light);
  border: none;
  border-radius: 4px;
  padding: 6px 12px;
  cursor: pointer;
  font-family: 'Poppins', italic;
  font-weight: 400;
  font-style: italic;
}

.removeItemBtn:hover {
  background-color: #c12e3c;
}

/* Payment methods section */
.methods button {
  display: block;
  width: 84%;
  max-width: 320px;
  margin: 16px auto;
}

/* Bank box for UPI */
.bank-box {
  background-color: #FFF5F6;
  border: 1px solid var(--accent-red);
  border-radius: 10px;
  padding: 18px;
  margin: 18px auto;
  max-width: 340px;
  text-align: left;
  line-height: 1.6;
  font-family: 'Poppins', italic;
  font-weight: 400;
  font-style: italic;
  color: var(--text-dark);
}

.bank-logos {
  width: 220px;
  margin: 12px 0;
}

.upi-note {
  font-size: 0.9rem;
  color: #555;
  margin-top: 8px;
  font-family: 'Poppins', italic;
  font-weight: 400;
  font-style: italic;
}

footer {
  margin-top: 40px;
  font-size: 0.8rem;
  color: #888;
  font-family: 'Poppins', italic;
  font-weight: 400;
  font-style: italic;
}
.order-history-link {
  font-style: italic;
  font-weight: bold;
  color: #ff0000;
  text-decoration: none;
}

.order-history-link:hover {
  text-decoration: underline;
  color: #000000;
}<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>PayFast - Summary</title>
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <div class="app-header">
    <img src="logo.png" alt="Logo" class="logo" />
    <h1 class="brand-name">PayFast</h1>
  </div>

  <div class="card">
    <button class="back-btn" onclick="goBack()">← Back</button>
    <h2>Order Summary</h2>
    <p><strong>Customer:</strong> <span id="custName"></span></p>

    <table id="itemsTable" style="width:100%; margin-top:10px; border-collapse: collapse;">
      <thead>
        <tr>
          <th style="text-align:left; border-bottom:1px solid #ccc;">Item Name</th>
          <th style="text-align:right; border-bottom:1px solid #ccc;">Price (₹)</th>
        </tr>
      </thead>
      <tbody></tbody>
    </table>

    <p style="text-align:right; margin-top: 20px;">
      <strong>Tax (18%): ₹<span id="taxAmount"></span></strong>
    </p>
    <p style="text-align:right;">
      <strong>Total: ₹<span id="totalAmount"></span></strong>
    </p>

    <button onclick="goToPayment()">Proceed to Payment</button>
  </div>

  <script>
    const name = localStorage.getItem("name");
    const items = JSON.parse(localStorage.getItem("items") || "[]");

    document.getElementById("custName").innerText = name;

    const tbody = document.querySelector("#itemsTable tbody");
    let subtotal = 0;
    items.forEach(({ name, price }) => {
      const tr = document.createElement("tr");
      tr.innerHTML = `<td>${name}</td><td style="text-align:right;">₹${price.toFixed(2)}</td>`;
      tbody.appendChild(tr);
      subtotal += price;
    });

    const tax = subtotal * 0.18;
    const total = subtotal + tax;

    document.getElementById("taxAmount").innerText = tax.toFixed(2);
    document.getElementById("totalAmount").innerText = total.toFixed(2);

    function goToPayment() {
      localStorage.setItem("totalAmount", total.toFixed(2));
      window.location.href = "payment.html";
    }

    function goBack() {
      window.location.href = "index.html";
    }
  </script>
</body>
</html>
<!DOCTYPE html>
<html lang="en" >
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>PayFast - UPI Payment Scanner</title>
  <link rel="stylesheet" href="style.css" />
  <style>
    .scanner-container {
      margin-top: 20px;
      position: relative;
      width: 280px;
      height: 280px;
      margin-left: auto;
      margin-right: auto;
      border: 3px solid #d10000;
      border-radius: 12px;
      box-sizing: border-box;
      background: #fff0f0;
      overflow: hidden;
      display: flex;
      justify-content: center;
      align-items: center;
    }

    /* Corners */
    .corner {
      width: 30px;
      height: 30px;
      border: 4px solid #d10000;
      position: absolute;
      pointer-events: none;
    }
    .corner.top-left {
      top: 0;
      left: 0;
      border-right: none;
      border-bottom: none;
      border-top-left-radius: 8px;
    }
    .corner.top-right {
      top: 0;
      right: 0;
      border-left: none;
      border-bottom: none;
      border-top-right-radius: 8px;
    }
    .corner.bottom-left {
      bottom: 0;
      left: 0;
      border-right: none;
      border-top: none;
      border-bottom-left-radius: 8px;
    }
    .corner.bottom-right {
      bottom: 0;
      right: 0;
      border-left: none;
      border-top: none;
      border-bottom-right-radius: 8px;
    }

    .scanned-image {
      max-width: 90%;
      max-height: 90%;
      border-radius: 10px;
      box-shadow: 0 0 10px #d10000aa;
    }

    /* Pay Button */
    .continue-btn {
      margin: 24px auto 0;
      display: block;
      background: linear-gradient(to right, #d10000, #001f7a);
      color: white;
      border: none;
      padding: 12px 30px;
      border-radius: 8px;
      font-weight: bold;
      font-size: 1rem;
      cursor: pointer;
      transition: background-color 0.3s ease;
      max-width: 280px;
      text-align: center;
    }
    .continue-btn:hover {
      background: linear-gradient(to right, #a00000, #000a55);
    }
  </style>
</head>
<body>
  <div class="app-header">
    <img src="logo.png" alt="Logo" class="logo" />
    <h1 class="brand-name">PayPie</h1>
  </div>

  <div class="card">
    <button class="back-btn" onclick="goBack()">← Back</button>
    <h2>Scan QR to Pay</h2>
    <p class="amount-big">₹<span id="upiAmount"></span></p>

    <div class="scanner-container" id="scannerContainer">
      <div class="corner top-left"></div>
      <div class="corner top-right"></div>
      <div class="corner bottom-left"></div>
      <div class="corner bottom-right"></div>

      <!-- Replace the src below with your actual image -->
      <img src="your-image.png" alt="Scanned QR" class="scanned-image" />
    </div>

    <!-- Pay button -->
    <button class="continue-btn" onclick="payNow()">Pay</button>
  </div>

  <script>
    document.getElementById("upiAmount").innerText = localStorage.getItem("totalAmount");

    function goBack() {
      window.location.href = "payment.html";
    }

    function payNow() {
      const name = localStorage.getItem("name") || "Customer";
      const item = localStorage.getItem("itemName") || "Food Item";
      const price = localStorage.getItem("price") || localStorage.getItem("totalAmount") || "0";

      // Save to order history
      const order = {
        name: name,
        item: item,
        price: price,
        date: new Date().toLocaleString()
      };

      const history = JSON.parse(localStorage.getItem("orderHistory") || "[]");
      history.push(order);
      localStorage.setItem("orderHistory", JSON.stringify(history));

      alert("Payment successful! Returning to home.");
      window.location.href = "index.html";
    }
  </script>
</body>
</html>
