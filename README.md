<!DOCTYPE html>
</p>
</div>


<div class="card" style="margin-top:1rem">
<h3>💵 Cash Option</h3>
<p>Cash is accepted at pickup. Exact change preferred.</p>
</div>
</section>


<section>
<h2>📸 Follow & Order on Instagram</h2>
<p>DMs open for questions & sneak peeks 👀</p>
<a href="https://instagram.com" target="_blank" style="display:inline-block;margin-top:1rem;padding:0.8rem 1.2rem;border-radius:14px;background:#6b4f3f;color:white;text-decoration:none">@bakedbychristy</a>
</section>


<footer>
© 2026 Baked by Christy · bakedbychristy.com
</footer>


<script>
// bake countdown
function updateCountdown() {
const now = new Date();
const sunday = new Date();
sunday.setDate(now.getDate() + ((7 - now.getDay()) % 7));
sunday.setHours(12,0,0,0);
const diff = sunday - now;
const days = Math.floor(diff / (1000 * 60 * 60 * 24));
document.getElementById('countdown').textContent = `Next bake in ${days} day(s) 🍞`;
}
updateCountdown();
// subtle parallax effect
window.addEventListener('scroll', () => {
const scrolled = window.scrollY;
document.querySelector('.video-bg').style.transform = `translateY(${scrolled * 0.15}px)`;
});
/<script>
  const stock = {
    'Chocolate Chip': 20,
    'Double Chocolate': 15,
    'Sugar Cookie': 0
  };

  function updateStockUI() {
    document.getElementById('ccStock').textContent =
      stock['Chocolate Chip'] > 0 ? 'In Stock' : 'SOLD OUT';
    document.getElementById('dcStock').textContent =
      stock['Double Chocolate'] > 0 ? 'In Stock' : 'SOLD OUT';
    document.getElementById('scStock').textContent =
      stock['Sugar Cookie'] > 0 ? 'In Stock' : 'SOLD OUT';

    const flavorSelect = document.getElementById('flavor');
    [...flavorSelect.options].forEach(opt => {
      if (stock[opt.value] === 0) opt.disabled = true;
    });
  }

  updateStockUI();

  const form = document.getElementById('orderForm');
  const message = document.getElementById('message');

  form.addEventListener('submit', function (e) {
    e.preventDefault();

    const name = document.getElementById('name').value;
    const email = document.getElementById('email').value;
    const qty = document.getElementById('qty').value;
    const payment = document.getElementById('payment').value;
    const flavor = document.getElementById('flavor').value;
    const school = document.getElementById('school').value;
    const day = document.getElementById('day').value;

    const total = qty * 2;

    const body = `New Cookie Order:%0D%0A
Name: ${name}%0D%0A
Email: ${email}%0D%0A
Quantity: ${qty}%0D%0A
Total: $${total}%0D%0A
Pickup Location: ${school}%0D%0A
Pickup Day: ${day}%0D%0A
Payment Method: ${payment}%0D%0A
Cookie Flavor: ${flavor}%0D%0A`;

    window.location.href =
      `mailto:luvz4himm@gmail.com?subject=Baked by Christy Order&body=${body}`;

    message.textContent = 'Order ready to send! Check your email app.';
    message.style.color = 'green';
  });
</script> Stock control (change numbers as needed)
