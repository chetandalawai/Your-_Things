
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Your Things | Online Grocery Store</title>
<style>
:root{--green:#0c831f;--light:#f5f8f2;--yellow:#f8df45}
*{box-sizing:border-box}
body{margin:0;font-family:Arial,sans-serif;background:#fff;color:#222}
header{background:white;position:sticky;top:0;z-index:10;box-shadow:0 2px 8px #ddd}
.top{display:flex;align-items:center;gap:12px;padding:13px;flex-wrap:wrap}
.logo{font-size:24px;font-weight:900;color:var(--green)}
.tag{font-size:11px;color:#777}
.search{flex:1;min-width:180px;padding:12px;border:1px solid #ddd;border-radius:9px;background:#f7f7f7}
button{cursor:pointer;border:0;border-radius:8px;padding:10px 14px;font-weight:bold}
.cartbtn{background:var(--green);color:white}
.hero{background:#e6f5dc;padding:24px 16px;margin:12px;border-radius:14px}
.hero h1{margin:0 0 8px;font-size:28px}
.hero p{margin:0 0 12px}
.categories{display:flex;gap:9px;overflow-x:auto;padding:12px}
.categories button{background:#f1f5ed;white-space:nowrap;border:1px solid #ddd}
.categories button.active{background:var(--green);color:white}
main{padding:10px 12px}
.sectiontitle{font-size:21px;margin:12px 0}
.grid{display:grid;grid-template-columns:repeat(2,minmax(0,1fr));gap:10px}
.card{border:1px solid #eee;border-radius:12px;padding:10px;min-width:0;position:relative}
.photo{height:125px;display:flex;align-items:center;justify-content:center;background:#f8f8f5;border-radius:8px;overflow:hidden}
.photo img{width:100%;height:100%;object-fit:contain}
.brand{font-size:11px;color:#777;margin-top:9px}
.name{font-size:14px;font-weight:bold;min-height:35px;margin-top:4px}
.size{font-size:12px;color:#777;margin:4px 0}
.price{font-weight:bold;font-size:14px}
.mrp{font-size:12px;color:#888;text-decoration:line-through;margin-left:5px}
.discount{font-size:11px;color:var(--green);font-weight:bold;margin-top:5px}
.add{background:#e5f5df;color:var(--green);border:1px solid var(--green);margin-top:9px;width:100%}
.qty{display:flex;align-items:center;justify-content:space-between;margin-top:9px;border:1px solid var(--green);border-radius:8px;color:var(--green)}
.qty button{background:var(--green);color:white;border-radius:6px}
.hidden{display:none!important}
.panel{margin:12px;padding:14px;border:1px solid #ddd;border-radius:12px}
.panel input,.panel select,.panel textarea{width:100%;padding:12px;margin:6px 0 10px;border:1px solid #ccc;border-radius:8px}
.line{display:flex;justify-content:space-between;gap:8px;padding:9px 0;border-bottom:1px solid #eee}
footer{text-align:center;background:#f5f5f5;padding:22px;margin-top:30px;color:#666;font-size:13px}
@media(min-width:600px){.grid{grid-template-columns:repeat(3,minmax(0,1fr))}.photo{height:160px}}
@media(min-width:950px){.grid{grid-template-columns:repeat(5,minmax(0,1fr))}.top{padding:16px 4%}main{padding:15px 4%}}
</style>
</head>
<body>
<header>
  <div class="top">
    <div>
      <div class="logo">🛒 YOUR THINGS</div>
      <div class="tag">EVERYDAY ESSENTIALS AT YOUR DOOR</div>
    </div>
    <input class="search" id="search" placeholder="Search groceries, shampoo, chocolates..." oninput="render()">
    <button class="cartbtn" onclick="showCart()">🛒 Cart <span id="cartCount">0</span></button>
  </div>
</header>

<div class="hero">
  <h1>Everything you need, delivered!</h1>
  <p>Groceries • Snacks • Personal Care • Home Essentials</p>
  <b>Discover your everyday favourites.</b>
</div>

<div class="categories" id="categories"></div>

<main>
  <div id="catalogue">
    <h2 class="sectiontitle" id="heading">Popular products</h2>
    <div class="grid" id="products"></div>
  </div>

  <section class="panel hidden" id="cartPanel">
    <h2>Your Shopping Cart</h2>
    <div id="cartItems"></div>
    <div class="line"><b>Subtotal</b><b id="subtotal">₹0</b></div>
    <div class="line"><span>Delivery charge</span><span id="delivery">₹0</span></div>
    <div class="line"><b>Total</b><b id="total">₹0</b></div>
    <button class="cartbtn" onclick="showCheckout()">Proceed to Checkout</button>
    <button onclick="hidePanels()">Continue Shopping</button>
  </section>

  <section class="panel hidden" id="checkoutPanel">
    <h2>Delivery Details</h2>
    <form id="orderForm">
      <label>Full name</label>
      <input name="customer" required placeholder="Your name">
      <label>Mobile number</label>
      <input name="phone" required type="tel" pattern="[6-9][0-9]{9}" placeholder="10-digit mobile number">
      <label>Full delivery address</label>
      <textarea name="address" required placeholder="House number, street, area, city, PIN code"></textarea>
      <label>Payment method</label>
      <select name="payment">
        <option value="Cash on Delivery">Cash on Delivery</option>
        <option value="UPI on delivery">UPI on delivery</option>
      </select>
      <p id="checkoutTotal"></p>
      <button type="submit" class="cartbtn">Place Order</button>
    </form>
    <button onclick="showCart()">Back to Cart</button>
    <div id="orderResult"></div>
  </section>
</main>

<footer>
  <b>YOUR THINGS</b><br><br>
  Your everyday grocery shopping destination.<br>
  Prices and availability are subject to confirmation.
</footer>

<script>
const image={
 "Chocolate":"https://images.unsplash.com/photo-1548907040-4d42bfc7f1a5?auto=format&fit=crop&w=400&q=80",
 "Chips & Snacks":"https://images.unsplash.com/photo-1566478989037-eec170784d0b?auto=format&fit=crop&w=400&q=80",
 "Cold Drinks & Juices":"https://images.unsplash.com/photo-1629203851122-3726ecdf080e?auto=format&fit=crop&w=400&q=80",
 "Biscuits & Bakery":"https://images.unsplash.com/photo-1558961363-fa8fdf82db35?auto=format&fit=crop&w=400&q=80",
 "Dairy & Eggs":"https://images.unsplash.com/photo-1563636619-e9143da7973b?auto=format&fit=crop&w=400&q=80",
 "Atta, Rice & Dal":"https://images.unsplash.com/photo-1586201375761-83865001e31c?auto=format&fit=crop&w=400&q=80",
 "Oil, Spices & Cooking":"https://images.unsplash.com/photo-1596040033229-a9821ebd058d?auto=format&fit=crop&w=400&q=80",
 "Shampoo & Hair Care":"https://images.unsplash.com/photo-1608248543803-ba4f8c70ae0b?auto=format&fit=crop&w=400&q=80",
 "Soap & Body Wash":"https://images.unsplash.com/photo-1600857544200-b2f666a9a2ec?auto=format&fit=crop&w=400&q=80",
 "Toothpaste & Oral Care":"https://images.unsplash.com/photo-1609840114035-3c981b782dfe?auto=format&fit=crop&w=400&q=80",
 "Skincare & Face Care":"https://images.unsplash.com/photo-1556228720-195a672e8a03?auto=format&fit=crop&w=400&q=80",
 "Deodorants & Perfumes":"https://images.unsplash.com/photo-1594035910387-fea47794261f?auto=format&fit=crop&w=400&q=80",
 "Personal Hygiene":"https://images.unsplash.com/photo-1608571423902-eed4a5ad8108?auto=format&fit=crop&w=400&q=80",
 "Cleaning & Laundry":"https://images.unsplash.com/photo-1585421514738-01798e348b17?auto=format&fit=crop&w=400&q=80",
 "Baby Care":"https://images.unsplash.com/photo-1519689680058-324335c77eba?auto=format&fit=crop&w=400&q=80",
 "Dry Fruits & Breakfast":"https://images.unsplash.com/photo-1536591375667-f6e2a6d4a8c1?auto=format&fit=crop&w=400&q=80"
};

const groups = {
 "Chocolate": {
  "Cadbury Dairy Milk":[5,10,20,40,80,100,150],
  "Cadbury 5 Star":[5,10,20,30,50],
  "Cadbury Fuse":[10,20,40,50],
  "Nestlé Munch":[5,10,20,40],
  "Nestlé KitKat":[10,20,40,50,100],
  "Snickers":[10,20,40,50,100],
  "Hershey's":[10,20,40,80,150],
  "Milkybar":[10,20,40,80],
  "Perk":[5,10,20,30],
  "Amul Chocolate":[10,20,40,80,150],
  "Ferrero Rocher":[50,100,150]
 },
 "Chips & Snacks": {
  "Lay's":[5,10,20,30,50],
  "Kurkure":[5,10,20,30],
  "Bingo! Mad Angles":[10,20,30],
  "Uncle Chipps":[5,10,20],
  "Pringles":[50,99,150],
  "Haldiram's Namkeen":[10,20,50,100,150],
  "Balaji Wafers":[5,10,20,50],
  "Too Yumm!":[10,20,30,50]
 },
 "Cold Drinks & Juices": {
  "Coca-Cola":[20,40,60,80],
  "Pepsi":[20,40,60,80],
  "Sprite":[20,40,60],
  "Thums Up":[20,40,60],
  "Fanta":[20,40,60],
  "Maaza":[10,20,40,60],
  "Slice":[10,20,40,60],
  "Real Fruit Power":[20,30,60,100,150],
  "Tropicana":[20,30,60,100],
  "Paper Boat":[20,30,50],
  "Red Bull":[125,150]
 },
 "Biscuits & Bakery": {
  "Parle-G":[5,10,20,40],
  "Oreo":[10,20,30,50],
  "Britannia Good Day":[10,20,30,50],
  "Sunfeast Dark Fantasy":[10,20,40,100],
  "Hide & Seek":[10,20,40],
  "Marie Gold":[10,20,40],
  "Bourbon":[10,20,30],
  "Jim Jam":[10,20,30],
  "Unibic":[20,30,50]
 },
 "Dairy & Eggs": {
  "Amul Milk":[15,30,35,40,50],
  "Nandini Milk":[25,30,35,40,50],
  "Amul Butter":[30,60,120,150],
  "Amul Cheese":[50,100,150],
  "Mother Dairy Curd":[25,40,60],
  "Amul Lassi":[15,25,30],
  "Britannia Bread":[30,40,50],
  "Eggs":[30,60,90,120]
 },
 "Atta, Rice & Dal": {
  "Aashirvaad Atta":[50,100,150],
  "Fortune Rice":[50,100,150],
  "India Gate Rice":[100,150],
  "Tata Sampann Dal":[50,100,150],
  "Fortune Besan":[40,80,150],
  "Pillsbury Atta":[50,100,150],
  "Organic Tattva Dal":[50,100,150]
 },
 "Oil, Spices & Cooking": {
  "Fortune Oil":[50,100,150],
  "Saffola Oil":[50,100,150],
  "Tata Salt":[10,20,30],
  "Everest Spices":[10,20,50,100],
  "MDH Masala":[10,20,50,100],
  "Catch Spices":[10,20,50],
  "Maggi Noodles":[5,14,28,56],
  "Tata Sampann Poha":[30,50,100]
 },
 "Shampoo & Hair Care": {
  "Dove Shampoo":[2,5,10,20,50,100,180],
  "Sunsilk Shampoo":[2,5,10,20,50,100,180],
  "Clinic Plus Shampoo":[1,2,5,10,20,50,100],
  "Head & Shoulders":[2,5,10,20,50,100,180],
  "Pantene Shampoo":[2,5,10,20,50,100,180],
  "Tresemmé Shampoo":[10,20,50,100,180],
  "L'Oréal Paris Shampoo":[10,20,50,100,180],
  "Himalaya Shampoo":[2,5,10,20,50,100],
  "Indulekha Hair Oil":[10,20,50,100,180],
  "Parachute Coconut Oil":[10,20,40,80,150],
  "Livon Hair Serum":[10,20,50,100,150]
 },
 "Soap & Body Wash": {
  "Lux Soap":[10,20,30,50],
  "Lifebuoy Soap":[10,20,30,50],
  "Dove Soap":[10,20,35,50,100],
  "Pears Soap":[10,20,40,60],
  "Dettol Soap":[10,20,35,50],
  "Santoor Soap":[10,20,30,50],
  "Cinthol Soap":[10,20,30,50],
  "Fiama Soap":[10,20,40,60],
  "Medimix Soap":[10,20,35,50],
  "Mysore Sandal Soap":[10,20,40,75],
  "Nivea Body Wash":[50,100,180],
  "Godrej No.1 Soap":[10,20,30,50]
 },
 "Toothpaste & Oral Care": {
  "Colgate Strong Teeth":[10,20,40,70,100,150],
  "Colgate MaxFresh":[10,20,50,100,150],
  "Pepsodent":[10,20,40,70,100],
  "Closeup":[10,20,40,70,100],
  "Sensodyne":[20,50,100,150],
  "Oral-B Toothpaste":[20,50,100,150],
  "Himalaya Toothpaste":[10,20,40,70,100],
  "Dabur Red":[10,20,40,70,100],
  "Colgate Toothbrush":[10,20,40,80,150],
  "Listerine Mouthwash":[50,100,150]
 },
 "Skincare & Face Care": {
  "Nivea Creme":[10,20,40,60,100,150],
  "Pond's Face Wash":[10,20,50,100,150],
  "Lakmé Sunscreen":[50,100,150,199],
  "Garnier Face Wash":[10,20,50,100,150],
  "Mamaearth Face Wash":[50,100,150,199],
  "Cetaphil Cleanser":[100,150,199],
  "Neutrogena Sunscreen":[100,150,199],
  "Simple Face Wash":[100,150,199],
  "Himalaya Face Wash":[10,20,50,100,150],
  "Lotus Herbals Sunscreen":[50,100,150,199],
  "Minimalist Serum":[100,150,199],
  "Vaseline Lotion":[10,20,50,100,150],
  "Pond's Cold Cream":[10,20,50,100,150],
  "Boroline Cream":[10,20,40,80]
 },
 "Deodorants & Perfumes": {
  "Fogg Body Spray":[50,100,150,199],
  "Denver Deodorant":[50,100,150,199],
  "Wild Stone Deodorant":[50,100,150,199],
  "Axe Body Spray":[50,100,150,199],
  "Nivea Deodorant":[50,100,150,199],
  "Engage Deodorant":[50,100,150,199],
  "Park Avenue Deodorant":[50,100,150,199],
  "Yardley Perfume":[100,150,199],
  "Bella Vita Perfume":[100,150,199],
  "Skinn Perfume":[150,199]
 },
 "Personal Hygiene": {
  "Gillette Shaving Cream":[20,40,80,150],
  "Gillette Razor":[20,50,100,150],
  "Veet Hair Removal Cream":[50,100,150,199],
  "Whisper Sanitary Pads":[30,50,100,150,199],
  "Stayfree Sanitary Pads":[30,50,100,150],
  "Dettol Handwash":[20,40,80,150],
  "Nivea Lip Balm":[20,40,80,150],
  "Pampers Baby Wipes":[50,100,150,199]
 },
 "Cleaning & Laundry": {
  "Surf Excel Detergent":[10,20,50,100,150,199],
  "Ariel Detergent":[10,20,50,100,150,199],
  "Rin Detergent":[10,20,50,100],
  "Vim Dishwash":[5,10,20,50,100],
  "Vim Bar":[5,10,20,30],
  "Harpic Cleaner":[20,50,100,150],
  "Lizol Cleaner":[20,50,100,150,199],
  "Good Knight":[20,50,100,150],
  "Scotch-Brite Scrub":[10,20,40,80]
 },
 "Baby Care": {
  "Pampers Diapers":[50,100,150,199],
  "Huggies Diapers":[50,100,150,199],
  "Johnson's Baby Soap":[20,40,60,100],
  "Johnson's Baby Powder":[20,50,100,150],
  "Himalaya Baby Lotion":[50,100,150,199],
  "Mamaearth Baby Wash":[100,150,199]
 },
 "Dry Fruits & Breakfast": {
  "Kellogg's Corn Flakes":[50,100,150,199],
  "Quaker Oats":[30,50,100,150],
  "Bagrry's Muesli":[100,150,199],
  "Happilo Almonds":[50,100,150,199],
  "Farmley Cashews":[50,100,150,199],
  "Saffola Oats":[30,50,100,150]
 }
};

const categories=["All Products",...Object.keys(groups)];
let products=[];
let id=0;

for(const [category,brands] of Object.entries(groups)){
  for(const [brand,prices] of Object.entries(brands)){
    prices.forEach((price,index)=>{
      id++;
      const pack=index===0?"Small pack":index===1?"Regular pack":index===2?"Value pack":"Family pack";
      products.push({
        id,category,brand,
        name:brand+" - "+pack,
        price,
        mrp:Math.ceil(price*1.12),
        size:pack,
        image:image[category]||"",
      });
    });
  }
}

let cart={};
try{cart=JSON.parse(localStorage.getItem("yourThingsCart")||"{}")||{};}catch(e){cart={};}
let activeCategory="All Products";

function saveCart(){
  try{localStorage.setItem("yourThingsCart",JSON.stringify(cart));}catch(e){}
  updateCount();
}
function money(n){return "₹"+Number(n).toFixed(0);}

function buildCategories(){
  document.getElementById("categories").innerHTML=categories.map(c=>
    `<button class="${c===activeCategory?'active':''}" onclick="chooseCategory(${JSON.stringify(c)})">${c}</button>`
  ).join("");
}
function chooseCategory(c){
  activeCategory=c;
  buildCategories();
  render();
  document.getElementById("catalogue").scrollIntoView({behavior:"smooth"});
}
function productCard(p){
  const q=Number(cart[p.id]||0);
  return `<article class="card">
    <div class="photo">
      <img src="${p.image}" alt="${p.brand}" loading="lazy"
      onerror="this.style.display='none';this.parentElement.innerHTML='<span style=&quot;font-size:45px&quot;>🛍️</span>'">
    </div>
    <div class="brand">${p.brand}</div>
    <div class="name">${p.name}</div>
    <div class="size">${p.size}</div>
    <div><span class="price">${money(p.price)}</span><span class="mrp">${money(p.mrp)}</span></div>
    <div class="discount">${Math.round((1-p.price/p.mrp)*100)}% off catalogue MRP</div>
    ${q===0
      ?`<button class="add" onclick="changeQty(${p.id},1)">ADD +</button>`
      :`<div class="qty"><button onclick="changeQty(${p.id},-1)">−</button><b>${q}</b><button onclick="changeQty(${p.id},1)">+</button></div>`}
  </article>`;
}
function render(){
  const query=document.getElementById("search").value.trim().toLowerCase();
  const list=products.filter(p=>
    (activeCategory==="All Products"||p.category===activeCategory)&&
    (p.brand.toLowerCase().includes(query)||p.name.toLowerCase().includes(query)||p.category.toLowerCase().includes(query))
  );
  document.getElementById("heading").textContent=
    activeCategory==="All Products"?"Popular products":activeCategory;
  document.getElementById("products").innerHTML=list.length
    ?list.map(productCard).join("")
    :"<p>No products found. Try another search.</p>";
}
function changeQty(id,delta){
  const next=Number(cart[id]||0)+delta;
  if(next<=0)delete cart[id];else cart[id]=next;
  saveCart();
  render();
  if(!document.getElementById("cartPanel").classList.contains("hidden"))showCart();
}
function updateCount(){
  document.getElementById("cartCount").textContent=
    Object.values(cart).reduce((a,b)=>a+Number(b),0);
}
function cartTotal(){
  return Object.entries(cart).reduce((sum,[id,q])=>{
    const p=products.find(x=>x.id===Number(id));
    return sum+(p?p.price*Number(q):0);
  },0);
}
function hidePanels(){
  document.getElementById("cartPanel").classList.add("hidden");
  document.getElementById("checkoutPanel").classList.add("hidden");
  document.getElementById("catalogue").classList.remove("hidden");
}
function showCart(){
  document.getElementById("catalogue").classList.add("hidden");
  document.getElementById("checkoutPanel").classList.add("hidden");
  document.getElementById("cartPanel").classList.remove("hidden");
  const entries=Object.entries(cart).filter(([id,q])=>Number(q)>0&&products.some(p=>p.id===Number(id)));
  document.getElementById("cartItems").innerHTML=entries.length?entries.map(([id,q])=>{
    const p=products.find(x=>x.id===Number(id));
    return `<div class="line"><div><b>${p.brand}</b><br><small>${p.name}</small><br>
    <button onclick="changeQty(${p.id},-1)">−</button>
    ${q}
    <button onclick="changeQty(${p.id},1)">+</button></div><b>${money(p.price*q)}</b></div>`;
  }).join(""):"<p>Your cart is empty. Add products to continue.</p>";
  const subtotal=cartTotal(),delivery=subtotal>0?0:0;
  document.getElementById("subtotal").textContent=money(subtotal);
  document.getElementById("delivery").textContent=subtotal===0?"₹0":"FREE (demo)";
  document.getElementById("total").textContent=money(subtotal+delivery);
  document.getElementById("cartPanel").scrollIntoView({behavior:"smooth"});
}
function showCheckout(){
  if(cartTotal()<=0){alert("Your cart is empty. Add products first.");return;}
  document.getElementById("cartPanel").classList.add("hidden");
  document.getElementById("checkoutPanel").classList.remove("hidden");
  document.getElementById("checkoutTotal").textContent="Order total: "+money(cartTotal());
  document.getElementById("checkoutPanel").scrollIntoView({behavior:"smooth"});
}
document.getElementById("orderForm").addEventListener("submit",function(e){
  e.preventDefault();
  if(cartTotal()<=0){alert("Your cart is empty.");return;}
  const f=new FormData(this);
  const orderId="YT"+Date.now().toString().slice(-8);
  const lines=Object.entries(cart).map(([id,q])=>{
    const p=products.find(x=>x.id===Number(id));
    return p?`${p.brand} (${p.name}) x ${q} = ${money(p.price*q)}`:null;
  }).filter(Boolean);
  const order={
    orderId,
    customer:f.get("customer"),
    phone:f.get("phone"),
    address:f.get("address"),
    payment:f.get("payment"),
    total:cartTotal(),
    items:lines,
    status:"Order details prepared - awaiting store confirmation",
    createdAt:new Date().toLocaleString()
  };
  let orders=[];
  try{orders=JSON.parse(localStorage.getItem("yourThingsOrders")||"[]");}catch(e){}
  orders.push(order);
  try{localStorage.setItem("yourThingsOrders",JSON.stringify(orders));}catch(e){}
  document.getElementById("orderResult").innerHTML=
    `<h3>Order details prepared</h3>
    <p>Order reference: <b>${orderId}</b></p>
    <p>Total: <b>${money(order.total)}</b></p>
    <p>Your order is saved on this device, but has NOT been sent to the shop.</p>
    <p>Contact the store to confirm availability, delivery and payment.</p>
    <button class="cartbtn" onclick="finishOrder()">Finish</button>`;
  document.getElementById("orderResult").scrollIntoView({behavior:"smooth"});
});
function finishOrder(){
  cart={};
  saveCart();
  document.getElementById("orderForm").reset();
  document.getElementById("orderResult").innerHTML="";
  hidePanels();
  render();
  window.scrollTo({top:0,behavior:"smooth"});
}
buildCategories();
updateCount();
render();
</script>
</body>
</html>
