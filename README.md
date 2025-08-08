<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Blanca's Kitchen Orders</title>
<style>
  body { font-family: Arial, sans-serif; background: #fff7f0; padding: 20px; }
  .container { max-width: 400px; margin: auto; background: white; padding: 20px; border-radius: 10px; box-shadow: 0 2px 10px rgba(0,0,0,0.1); }
  input, textarea, button { width: 100%; padding: 10px; margin-top: 8px; border-radius: 5px; border: 1px solid #ccc; }
  button { background: #e67300; color: white; border: none; font-size: 16px; }
  button:hover { background: #cc6600; }
</style>
</head>
<body>
<div class="container">
  <h1>Blanca's Kitchen</h1>
  <h3>Pan con Pollo - $12</h3>
  <form id="orderForm">
    <label>Quantity:</label>
    <input type="number" name="quantity" min="1" value="1" required />

    <label>Name:</label>
    <input type="text" name="name" required />

    <label>Phone:</label>
    <input type="tel" name="phone" required />

    <label>Address:</label>
    <textarea name="address" required></textarea>

    <button type="submit">Place Order</button>
  </form>
  <p id="message"></p>
</div>

<script>
const scriptURL = "https://script.google.com/macros/s/AKfycbyOvSepMZPETCheXygsSRSl_22e25ABmwTcvFSNWKSMDKk_i26ndSGNzt8Fxd8PQh1N2w/exec"; // Replace with your deployed URL
const pricePerItem = 12; // Price in USD

document.getElementById("orderForm").addEventListener("submit", async function(e) {
  e.preventDefault();
  
  const quantity = parseInt(e.target.quantity.value, 10);
  const totalPrice = quantity * pricePerItem;

  const formData = {
    quantity: quantity,
    name: e.target.name.value,
    phone: e.target.phone.value,
    address: e.target.address.value,
    pricePerItem: pricePerItem,
    totalPrice: totalPrice,
    timestamp: new Date().toISOString()
  };

  try {
    const res = await fetch(scriptURL, {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify(formData)
    });
    const result = await res.json();
    if (result.status === "success") {
      document.getElementById("message").textContent = `✅ Order placed! Total: $${totalPrice}`;
      e.target.reset();
    } else {
      document.getElementById("message").textContent = "❌ Error placing order.";
    }
  } catch (err) {
    document.getElementById("message").textContent = "❌ Network error.";
  }
});
</script>
</body>
</html>
