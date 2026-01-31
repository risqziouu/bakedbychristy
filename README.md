<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Baked by Christy</title>
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;500;700&display=swap" rel="stylesheet">
  <style>
    :root {
      --brown: #6b4f3f;
      --khaki: #d6c7a1;
    }

    * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Poppins', sans-serif; }

    body {
      color: var(--brown);
      background: var(--khaki);
      overflow-x: hidden;
    }

    /* Background video */
    .video-bg {
      position: fixed;
      top: 0; left: 0;
      width: 100%; height: 100%;
      object-fit: cover;
      z-index: -2;
      filter: brightness(0.6);
    }

    .overlay {
      position: fixed;
      inset: 0;
      background: rgba(214,199,161,0.6);
      z-index: -1;
    }

    header {
      padding: 4rem 2rem;
      text-align: center;
      animation: fadeDown 1.2s ease;
    }

    header h1 {
      font-size: 4rem;
      font-weight: 700;
      letter-spacing: 2px;
    }

    header p {
      font-size: 1.3rem;
      margin-top: 1rem;
    }

    section {
      max-width: 900px;
      margin: 3rem auto;
      padding: 2rem;
      background: rgba(255,255,255,0.85);
      border-radius: 24px;
      box-shadow: 0 20px 40px rgba(0,0,0,0.15);
      animation: fadeUp 1.2s ease;
    }

    h2 {
      font-size: 2.2rem;
      margin-bottom: 1rem;
    }

    .pop {
      animation: pop 2s infinite alternate;
    }

    .grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
      gap: 1.5rem;
    }

    .card {
      background: var(--khaki);
      padding: 1.5rem;
      border-radius: 20px;
      text-align: center;
      transition: transform 0.3s ease;
    }

    .card:hover { transform: translateY(-8px); }

    form {
      display: grid;
      gap: 1rem;
    }

    input, select, button {
      padding: 0.8rem;
      border-radius: 12px;
      border: 2px solid var(--brown);
      font-size: 1rem;
    }

    button {
      background: var(--brown);
      color: white;
      cursor: pointer;
      transition: transform 0.2s ease, background 0.2s ease;
    }

    button:hover {
      transform: scale(1.05);
      background: #52392d;
    }

    footer {
      text-align: center;
      padding: 2rem;
      font-size: 0.9rem;
    }

    @keyframes fadeDown {
      from { opacity: 0; transform: translateY(-40px); }
      to { opacity: 1; transform: translateY(0); }
    }

    @keyframes fadeUp {
      from { opacity: 0; transform: translateY(40px); }
      to { opacity: 1; transform: translateY(0); }
    }

    @keyframes pop {
      from { letter-spacing: 1px; }
      to { letter-spacing: 4px; }
    }
  </style>
</head>
<body>

  <!-- Replace video src with your own baking video -->
  <video class="video-bg" autoplay muted loop>
    <source src="https://cdn.coverr.co/videos/coverr-baking-cookies-8540/1080p.mp4" type="video/mp4">
  </video>
  <div class="overlay"></div>

  <header>
    <h1 class="pop">Baked by Christy</h1>
    <p>Fresh, homemade cookies — baked with love 🤎</p>
  </header>

  <section>
    <h2>How Ordering Works</h2>
    <div class="grid">
      <div class="card">🍪 Orders are <strong>placed on Sundays</strong> (Bake Day)</div>
      <div class="card">📦 We sell only on <strong>Mondays, Wednesdays & Fridays</strong></div>
      <div class="card">🏫 Pick up at <strong>Benjamin Tasker</strong> or <strong>Bowie High</strong></div>
    </div>
  </section>

  <section>
    <h2>Checkout</h2>
    <p><strong>Price:</strong> $2 per cookie</p>
    <form id="orderForm">
      <input type="text" id="name" placeholder="Your Name" required />
      <input type="email" id="email" placeholder="Your Email" required />
      <input type="number" id="qty" placeholder="Number of Cookies" min="1" required />

      <select id="school" required>
        <option value="">Pickup Location</option>
        <option>Benjamin Tasker</option>
        <option>Bowie High</option>
      </select>

      <select id="day" required>
        <option value="">Pickup Day</option>
        <option>Monday</option>
        <option>Wednesday</option>
        <option>Friday</option>
      </select>

      <select id="payment" required>
        <option value="">Payment Method</option>
        <option>Apple Pay</option>
        <option>Cash</option>
      </select>

      <button type="submit">Place Order</button>
      <p id="message"></p>
    </form>

    <div class="card" style="margin-top:1.5rem">
      <h3>🍎 Apple Pay Instructions</h3>
      <p>
        Send payment via <strong>Apple Pay</strong> to:<br>
        <strong>202-873-0422</strong><br><br>
        Include your <strong>name + quantity</strong> in the note.
      </p>
    </div>

    <div class="card" style="margin-top:1rem">
      <h3>💵 Cash Option</h3>
      <p>Cash is accepted at pickup. Exact change preferred.</p>
    </div>
  </section>

  <footer>
    © 2026 Baked by Christy · bakedbychristy.com
  </footer>

  <script>
    const form = document.getElementById('orderForm');
    const message = document.getElementById('message');

    form.addEventListener('submit', function(e) {
      e.preventDefault();

      const today = new Date().getDay(); // 0 = Sunday
      if (today !== 0) {
        message.textContent = 'Orders can only be placed on Sundays (Bake Day).';
        message.style.color = 'red';
        return;
      }

      const name = document.getElementById('name').value;
      const email = document.getElementById('email').value;
      const qty = document.getElementById('qty').value;
      const payment = document.getElementById('payment').value;
      const total = qty * 2;
      const school = document.getElementById('school').value;
      const day = document.getElementById('day').value;

      const body = `New Cookie Order:%0D%0A
Name: ${name}%0D%0A
Email: ${email}%0D%0A
Quantity: ${qty}%0D%0A
Total: $${total}%0D%0A
Pickup Location: ${school}%0D%0A
Pickup Day: ${day}%0D%0A
Payment Method: ${payment}%0D%0A

If Apple Pay was selected, customer was instructed to send payment to 202-873-0422.`;

      window.location.href = `mailto:luvz4himm@gmail.com?subject=Baked by Christy Order&body=${body}`;

      message.textContent = 'Order ready to send! Check your email app.';
      message.style.color = 'green';
    });
  </script>

</body>
</html>
