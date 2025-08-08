<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Order Pan con Pollo</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <style>
    body { font-family: Arial, sans-serif; background: #fafafa; text-align: center; padding: 20px; }
    form { background: #fff; padding: 20px; border-radius: 10px; display: inline-block; box-shadow: 0 0 10px rgba(0,0,0,0.1); }
    input, button { width: 100%; padding: 10px; margin: 8px 0; font-size: 16px; }
    button { background: #28a745; color: #fff; border: none; cursor: pointer; }
    button:hover { background: #218838; }
  </style>
</head>
<body>
  <h1>Pan con Pollo - $12</h1>
  <form id="orderForm">
    <input type="text" id="name" placeholder="Your Name" required>
    <input type="tel" id="phone" placeholder="Phone Number" required>
    <input type="text" id="address" placeholder="Delivery Address" required>
    <input type="number" id="quantity" placeholder="Quantity" min="1" value="1" required>
    <button type="submit">Place Order</button>
  </form>

  <script>
    const WEB_APP_URL = "(https://script.google.com/macros/s/AKfycbyuXgann8QQ7osW2ZZ4MQEdtQWACzNl-WgHx3ODr5GX3u8fl44woAVusK2KndercU_WKA/exec)";

    document.getElementById("orderForm").addEventListener("submit", function(e) {
      e.preventDefault();

      const quantity = parseInt(document.getElementById("quantity").value);
      const orderData = {
        timestamp: new Date().toLocaleString(),
        name: document.getElementById("name").value,
        phone: document.getElementById("phone").value,
        address: document.getElementById("address").value,
        quantity: quantity
      };

      fetch(WEB_APP_URL, {
        method: "POST",
        mode: "cors",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(orderData)
      })
      .then(res => res.json())
      .then(data => {
        if (data.status === "success") {
          alert("Order placed! We’ll contact you soon.");
          document.getElementById("orderForm").reset();
        } else {
          alert("Error: " + data.message);
        }
      })
      .catch(err => {
        alert("Network error. Please try again.");
        console.error(err);
      });
    });
  </script>
</body>
</html>
