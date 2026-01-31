<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Baked by Christy</title>

  <!-- Stripe -->
  <script src="https://js.stripe.com/v3/"></script>

  <!-- Firebase -->
  <script src="https://www.gstatic.com/firebasejs/9.23.0/firebase-app-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.23.0/firebase-firestore-compat.js"></script>

  <style>
    :root {
      --brown: #6b4f3f;
      --khaki: #d8cfc4;
      --cream: #fffaf5;
    }
    body { margin:0; font-family:Poppins, sans-serif; background:var(--cream); color:var(--brown);}    

    .hero { position:relative; height:100vh; overflow:hidden; }
    .bg-video {
      position:absolute;
      top:50%; left:50%;
      width:100%; height:100%;
      object-fit:cover;
      transform:translate(-50%, -50%);
      z-index:-2;
    }
    .overlay {
      position:absolute;
      inset:0;
      background:linear-gradient(rgba(107,79,63,0.55), rgba(216,207,196,0.55));
      z-index:-1;
    }
    .hero-content {
      height:100%;
      display:flex;
      flex-direction:column;
      align-items:center;
      justify-content:center;
      text-align:center;
      color:white;
      padding:1rem;
    }
    .hero-content h1 {
      font-size:clamp(2.5rem,5vw,4rem);
      letter-spacing:2px;
    }
    .hero-content p {
      font-size:1.2rem;
      margin-top:0.75rem;
    }

    .corner { position:absolute; top:15px; right:15px; font-size:0.9rem; color:white; }
    section { max-width:1100px; margin:auto; padding:4rem 1.5rem; }
    .menu { display:grid; grid-template-columns:repeat(auto-fit,minmax(220px,1fr)); gap:1.5rem; }
    .card { background:#fff; border-radius:20px; padding:1.5rem; box-shadow:0 10px 20px rgba(0,0,0,0.08);}    
    button { background:var(--brown); color:#fff; border:none; padding:0.6rem 1rem; border-radius:999px; cursor:pointer; }
    .out { opacity:0.5; pointer-events:none; }
    .admin { background:var(--khaki); border-radius:20px; padding:2rem; margin-top:4rem; }

.transition-section {
  position:relative;
  height:70vh;
  overflow:hidden;
}
.transition-video {
  position:absolute;
  top:50%; left:50%;
  width:100%; height:100%;
  object-fit:cover;
  transform:translate(-50%, -50%);
}
.transition-overlay {
  position:absolute;
  inset:0;
  background:linear-gradient(rgba(255,250,245,0.1), rgba(255,250,245,0.9));
}
  </style>
</head>
<body onload="unlockAdmin()">

<header class="hero">
  <video autoplay muted loop playsinline class="bg-video">
    <source src="https://cdn.coverr.co/videos/coverr-baker-preparing-dough-6995/1080p.mp4" type="video/mp4">
  </video>
  <div class="overlay"></div>
  <div class="hero-content">
    <h1>Baked by Christy</h1>
    <p>Founded in 2025, Baked with love</p>
  </div>
  <div class="corner">Christiana Thompson & Kwame Thompson</div>
</header>

<section class="transition-section">
  <video autoplay muted loop playsinline class="transition-video">
    <source src="https://cdn.coverr.co/videos/coverr-putting-cookies-in-the-oven-9717/1080p.mp4" type="video/mp4">
  </video>
  <div class="transition-overlay"></div>
</section>

<section>
  <h2>Our Cookies</h2>

  <div id="statusBanner" style="background:#6b4f3f;color:white;padding:1rem 1.25rem;border-radius:16px;margin-bottom:2rem;text-align:center;font-weight:500"></div></h2>

  <div style="background:#fff;border-radius:20px;padding:1.5rem;margin-bottom:3rem;box-shadow:0 10px 20px rgba(0,0,0,0.08)">
    <h3>Order Details</h3>
    <p><strong>Selling Days:</strong> Monday • Wednesday • Friday</p>
    <p><strong>Delivery:</strong> Available every day</p>

    <label>
      <input type="radio" name="orderType" value="pickup" /> Pickup
    </label><br><br>

    <label>
      <input type="radio" name="orderType" value="school" /> School Delivery
    </label><br><br>

    <label for="daySelect"><strong>Select Order Day</strong></label><br>
    <select id="daySelect" required>
      <option value="">Choose a day</option>
      <option value="Monday">Monday</option>
      <option value="Wednesday">Wednesday</option>
      <option value="Friday">Friday</option>
    </select><br><br>

    <select id="schoolSelect" style="display:none">
      <option value="">Select School (if delivery)</option>
      <option value="Benjamin Tasker">Benjamin Tasker</option>
      <option value="Bowie High School">Bowie High School</option>
    </select>
  </div>
  <div class="menu" id="menu"></div>
</section>

<section class="admin" id="adminPanel" style="display:none">
  <h3>Admin – Inventory & Orders Control</h3>
  <p>Toggle items in or out of stock (only you should see this)</p>
  <div id="adminControls"></div>
</section>

<section style="background:#fff;border-radius:20px;padding:2rem;margin-top:4rem;box-shadow:0 10px 20px rgba(0,0,0,0.08)">
  <h3>Pickup Time</h3>
  <select id="pickupTime">
    <option value="">Select pickup time</option>
    <option>8:00 AM – 9:00 AM</option>
    <option>9:00 AM – 10:00 AM</option>
    <option>10:00 AM – 11:00 AM</option>
    <option>11:00 AM – 12:00 PM</option>
  </select>
</section>

<footer style="background:#6b4f3f;color:white;text-align:center;padding:2.5rem">
  <div>Follow Our IG</div>
  <div>IG – bakedbychristy</div>
</footer>

<script>
  // 🔐 Firebase config (REPLACE WITH YOUR OWN)
  const firebaseConfig = {
    apiKey: "YOUR_API_KEY",
    authDomain: "YOUR_PROJECT.firebaseapp.com",
    projectId: "YOUR_PROJECT_ID",
  };

  firebase.initializeApp(firebaseConfig);
  const db = firebase.firestore();

  // 💳 Stripe (Apple Pay enabled in Stripe Dashboard)
  const stripe = Stripe("YOUR_STRIPE_PUBLISHABLE_KEY");

  const products = [
    { id: "oreo", name: "Double Stuff Oreo", price: 300 },
    { id: "chip", name: "Regular Chocolate Chip", price: 100 },
    { id: "creme", name: "Hershey's Cookies n Creme", price: 300 }
  ];

  async function loadMenu() {
    const menu = document.getElementById('menu');
    const admin = document.getElementById('adminControls');
    menu.innerHTML = admin.innerHTML = '';

    for (let p of products) {
      const doc = await db.collection('inventory').doc(p.id).get();
      const inStock = doc.exists ? doc.data().inStock : true;

      const card = document.createElement('div');
      card.className = 'card' + (inStock ? '' : ' out');
      card.innerHTML = `<h3>${p.name}</h3><p>$${(p.price/100).toFixed(2)}</p>`;

      if (inStock) {
        const btn = document.createElement('button');
        btn.innerText = 'Pay with Apple Pay';
        btn.onclick = () => checkout(p);
        card.appendChild(btn);
      } else {
        card.innerHTML += '<p>Out of Stock</p>';
      }
      menu.appendChild(card);

      // Admin toggle
      const toggle = document.createElement('button');
      toggle.innerText = `${p.name}: ${inStock ? 'IN STOCK' : 'OUT OF STOCK'}`;
      toggle.onclick = () => db.collection('inventory').doc(p.id).set({ inStock: !inStock });
      admin.appendChild(toggle);
    }
  }

  function unlockAdmin() {
    const pass = prompt('Enter admin password');
    if (pass === 'bakedbychristy') {
      document.getElementById('adminPanel').style.display = 'block';
    }
  }

  function updateUIRules() {
    const today = new Date().getDay(); // 0 Sun
    const sellingDays = [1,3,5]; // Mon Wed Fri
    const banner = document.getElementById('statusBanner');

    if (!sellingDays.includes(today)) {
      banner.innerText = 'Orders are closed today 🤎 Next bake day is ' + nextBakeDay();
      banner.style.opacity = '0.9';
    } else {
      banner.innerText = 'Taking orders today 🍪 Same‑day delivery available';
    }

    document.querySelectorAll('input[name="orderType"]').forEach(radio => {
      radio.addEventListener('change', () => {
        const schoolSelect = document.getElementById('schoolSelect');
        if (radio.value === 'school' && radio.checked) {
          schoolSelect.style.display = 'block';
        } else if (radio.value === 'pickup' && radio.checked) {
          schoolSelect.style.display = 'none';
          schoolSelect.value = '';
        }
      });
    });
  }

  function nextBakeDay() {
    const days = ['Sunday','Monday','Tuesday','Wednesday','Thursday','Friday','Saturday'];
    const today = new Date().getDay();
    const bakeDays = [1,3,5];
    for (let d of bakeDays) {
      if (d > today) return days[d];
    }
    return 'Monday';
  }

  async function checkout(product) {
    const orderType = document.querySelector('input[name="orderType"]:checked')?.value;
    const school = document.getElementById('schoolSelect')?.value || 'N/A';

    if (!orderType) {
      alert('Please select delivery school or pickup option');
      return;
    }

    const session = await fetch('/create-checkout-session', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        product,
        orderType,
        school
      })
    }).then(res => res.json());

    // Send order summary email
    fetch('/send-order-summary', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ product, orderType, school, orderDay })
    });

    stripe.redirectToCheckout({ sessionId: session.id });
  },
      body: JSON.stringify(product)
    }).then(res => res.json());

    stripe.redirectToCheckout({ sessionId: session.id });
  }

  loadMenu();
  updateUIRules();
</script>

</body>
</html>
