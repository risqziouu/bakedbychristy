<!DOCTYPE html>
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
