# kantin-digital
aplikasih kantin digital
 <h3>${f.name}</h3><p class="price">${rupiah(f.price)}</p>
 <button onclick="add(${f.id})">＋ Tambah</button></div></article>`).join("");
}
function add(id){cart[id]=(cart[id]||0)+1;save();renderCart();}
function change(id,delta){cart[id]=(cart[id]||0)+delta;if(cart[id]<=0)delete cart[id];save();renderCart();}
function clearCart(){cart={};save();renderCart();}
function getTotal(){return foods.reduce((s,f)=>s+f.price*(cart[f.id]||0),0);}
function renderCart(){
 const entries=foods.filter(f=>cart[f.id]);
 document.getElementById("cart").innerHTML=entries.length?entries.map(f=>`
 <div class="cartrow"><div><strong>${f.name}</strong><br>${rupiah(f.price)} × ${cart[f.id]}</div>
 <div><button class="secondary" onclick="change(${f.id},-1)">−</button><button onclick="change(${f.id},1)">＋</button></div></div>`).join(""):'<p class="note">Keranjang masih kosong. Tambahkan makanan dari menu.</p>';
 document.getElementById("total").textContent=rupiah(getTotal());
 document.getElementById("checkout").disabled=entries.length===0;
 document.getElementById("balance").textContent=rupiah(balance);
}
function topUp(){
 const amount=Number(prompt("Masukkan jumlah isi saldo (contoh: 10000):","10000"));
 if(!Number.isFinite(amount)||amount<=0||!Number.isInteger(amount)){alert("Masukkan jumlah saldo berupa angka positif.");return;}
 balance+=amount;save();renderCart();alert("Saldo simulasi berhasil ditambahkan.");
}
function checkout(){
 const total=getTotal();if(!total)return;
 if(total>balance){alert("Saldo tidak cukup. Silakan isi saldo simulasi terlebih dahulu.");return;}
 if(!confirm("Konfirmasi pesanan dengan total "+rupiah(total)+"?"))return;
 const items=foods.filter(f=>cart[f.id]).map(f=>`${f.name} × ${cart[f.id]}`).join(", ");
 balance-=total;
 orders.unshift({date:new Date().toLocaleString("id-ID"),items,total,status:"Pesanan diterima"});
 cart={};save();renderCart();renderOrders();alert("Pesanan berhasil dibuat! Tunjukkan ringkasan pesanan kepada penjual kantin.");
}
function renderOrders(){
 document.getElementById("orders").innerHTML=orders.length?orders.map(o=>`
 <div class="order"><span class="pill">${o.status}</span><p><strong>${o.items}</strong></p>
 <div>${o.date}</div><strong>${rupiah(o.total)}</strong></div>`).join(""):'<p class="note">Belum ada pesanan.</p>';
}
renderMenu();renderCart();renderOrders();
</script>
</body>
</html>
