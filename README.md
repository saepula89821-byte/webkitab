# webkitab<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="description" content="KitabShop Ultimate - Marketplace kitab Islam terlengkap dengan fitur profesional.">
    <title>KitabShop Ultimate</title>
    <link rel="manifest" id="manifest-link">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * { margin:0; padding:0; box-sizing:border-box; font-family:'Segoe UI', Roboto, sans-serif; }
        :root {
            --primary: #ee4d2d; --primary-dark: #d23f1f; --bg: #f5f5f5; --white: #fff;
            --text: #222; --text-light: #555; --border: #e0e0e0; --shadow: 0 2px 12px rgba(0,0,0,0.08);
            --shadow-hover: 0 8px 24px rgba(0,0,0,0.12); --gold: #f59e0b; --green: #10b981;
        }
        body.dark {
            --bg: #1a1a2e; --white: #16213e; --text: #eee; --text-light: #bbb; --border: #2a2a4a;
            --shadow: 0 2px 12px rgba(255,255,255,0.05); --shadow-hover: 0 8px 24px rgba(255,255,255,0.08);
        }
        body { background: var(--bg); color: var(--text); transition: background 0.3s, color 0.3s; }
        /* Toast */
        .toast-container { position:fixed; top:20px; right:20px; z-index:9999; display:flex; flex-direction:column; gap:8px; }
        .toast { background:white; padding:12px 20px; border-radius:8px; box-shadow:0 4px 20px rgba(0,0,0,0.15); display:flex; align-items:center; gap:8px; animation:slideIn 0.3s ease, fadeOut 0.5s 2.5s forwards; max-width:350px; font-weight:500; }
        .toast.success { border-left:4px solid var(--green); }
        .toast.info { border-left:4px solid #3b82f6; }
        @keyframes slideIn { from{transform:translateX(150%);opacity:0} to{transform:translateX(0);opacity:1} }
        @keyframes fadeOut { to{opacity:0;transform:translateX(50px)} }
        /* Header */
        header { background: var(--white); padding: 10px 5%; display:flex; align-items:center; gap:1.5rem; box-shadow:0 2px 8px rgba(0,0,0,0.06); position:sticky; top:0; z-index:100; transition: background 0.3s; }
        .logo { font-size:1.6rem; font-weight:700; color:var(--primary); text-decoration:none; display:flex; align-items:center; gap:8px; }
        .search-box { flex:1; max-width:500px; display:flex; border:2px solid var(--primary); border-radius:4px; overflow:hidden; }
        .search-box input { flex:1; border:none; padding:10px 16px; font-size:0.95rem; outline:none; background:var(--white); color:var(--text); }
        .search-box button { background:var(--primary); border:none; padding:10px 20px; color:white; cursor:pointer; }
        .header-actions { display:flex; gap:1.5rem; align-items:center; }
        .header-actions .btn-icon { font-size:1.6rem; color:var(--text-light); cursor:pointer; transition:0.2s; position:relative; }
        .header-actions .btn-icon:hover { color:var(--primary); }
        .badge { position:absolute; top:-10px; right:-14px; background:var(--primary); color:white; border-radius:50%; width:20px; height:20px; font-size:0.7rem; display:flex; align-items:center; justify-content:center; }
        #darkToggle { cursor:pointer; }
        /* Breadcrumb */
        .breadcrumb { padding: 10px 5%; font-size:0.85rem; color:var(--text-light); background:var(--white); margin:0 5%; border-radius:0 0 8px 8px; display:flex; gap:6px; flex-wrap:wrap; }
        .breadcrumb a { color:var(--primary); text-decoration:none; }
        /* Toolbar */
        .toolbar { display:flex; flex-wrap:wrap; justify-content:space-between; align-items:center; padding:15px 5%; gap:12px; background:var(--white); margin:12px 5%; border-radius:8px; box-shadow:var(--shadow); }
        .categories { display:flex; gap:8px; flex-wrap:wrap; }
        .category-btn { background: #f0f0f0; border:none; padding:8px 16px; border-radius:20px; cursor:pointer; font-size:0.9rem; transition:0.3s; color:#444; }
        .category-btn:hover, .category-btn.active { background:var(--primary); color:white; }
        .sort-select { padding:8px 12px; border-radius:8px; border:1px solid #ccc; font-size:0.9rem; background:var(--white); color:var(--text); }
        /* Skeleton */
        .skeleton { background: linear-gradient(90deg, #eee 25%, #ddd 50%, #eee 75%); background-size:200% 100%; animation: shimmer 1.5s infinite; border-radius:10px; }
        @keyframes shimmer { 0%{background-position:200% 0} 100%{background-position:-200% 0} }
        .product-card.skeleton-item { height:320px; }
        .product-card.skeleton-item .product-img { background:#e0e0e0; height:200px; }
        /* Product Grid */
        .product-grid { display:grid; grid-template-columns:repeat(auto-fill, minmax(200px,1fr)); gap:1.2rem; padding:0 5% 1rem; }
        .product-card { background:var(--white); border-radius:10px; overflow:hidden; box-shadow:var(--shadow); transition:0.2s; display:flex; flex-direction:column; position:relative; cursor:pointer; }
        .product-card:hover { transform:translateY(-6px); box-shadow:var(--shadow-hover); }
        .product-img { width:100%; height:200px; object-fit:cover; background:#f9f9f9; transition:0.3s; }
        .product-card:hover .product-img { transform:scale(1.03); }
        .wishlist-icon { position:absolute; top:12px; right:12px; background:rgba(255,255,255,0.8); border-radius:50%; width:32px; height:32px; display:flex; align-items:center; justify-content:center; color:#aaa; cursor:pointer; z-index:5; }
        .wishlist-icon.liked { color:#e74c3c; }
        .product-info { padding:12px 14px; flex:1; display:flex; flex-direction:column; }
        .product-title { font-weight:600; font-size:0.95rem; line-height:1.4; display:-webkit-box; -webkit-line-clamp:2; -webkit-box-orient:vertical; overflow:hidden; }
        .product-price { color:var(--primary); font-weight:bold; font-size:1.1rem; margin:8px 0; }
        .product-meta { display:flex; justify-content:space-between; font-size:0.75rem; color:#999; margin-bottom:10px; }
        .add-to-cart-btn, .compare-btn { background:var(--primary); color:white; border:none; padding:8px 12px; border-radius:6px; cursor:pointer; font-weight:500; display:flex; align-items:center; justify-content:center; gap:6px; transition:0.2s; font-size:0.85rem; }
        .add-to-cart-btn:hover, .compare-btn:hover { background:var(--primary-dark); }
        .compare-btn { background:#3498db; margin-top:4px; }
        .compare-btn.active { background:#2c3e50; }
        .stock-info { font-size:0.75rem; color:#e74c3c; margin-top:4px; }
        /* Load More */
        .load-more-container { text-align:center; padding:1rem; }
        .load-more-btn { background:white; border:1px solid var(--primary); color:var(--primary); padding:10px 30px; border-radius:20px; cursor:pointer; transition:0.3s; }
        .load-more-btn:hover { background:var(--primary); color:white; }
        /* Modal */
        .modal { position:fixed; top:0; left:0; width:100%; height:100%; background:rgba(0,0,0,0.5); z-index:1000; display:flex; align-items:center; justify-content:center; visibility:hidden; opacity:0; transition:0.3s; }
        .modal.open { visibility:visible; opacity:1; }
        .modal-content { background:var(--white); border-radius:12px; max-width:600px; width:90%; max-height:90vh; overflow-y:auto; padding:24px; position:relative; color:var(--text); }
        .modal-close { position:absolute; top:16px; right:16px; background:none; border:none; font-size:2rem; cursor:pointer; color:var(--text-light); }
        img { max-width:100%; }
        .gallery { display:flex; gap:8px; overflow-x:auto; margin-bottom:16px; }
        .gallery img { width:80px; height:80px; object-fit:cover; border-radius:4px; cursor:pointer; opacity:0.6; transition:0.2s; }
        .gallery img.active { opacity:1; border:2px solid var(--primary); }
        .variant-option { display:inline-block; padding:6px 12px; border:1px solid var(--border); border-radius:20px; cursor:pointer; margin:0 4px; transition:0.2s; }
        .variant-option.selected { background:var(--primary); color:white; border-color:var(--primary); }
        /* Panel slide */
        .slide-panel-overlay{position:fixed;top:0;left:0;width:100%;height:100%;background:rgba(0,0,0,0.4);z-index:500;display:flex;justify-content:flex-end;visibility:hidden;opacity:0;transition:0.3s;}
        .slide-panel-overlay.open{visibility:visible;opacity:1;}
        .slide-panel{background:var(--white);width:100%;max-width:420px;height:100%;display:flex;flex-direction:column;box-shadow:-4px 0 20px rgba(0,0,0,0.2);transform:translateX(100%);transition:0.3s;color:var(--text);}
        .slide-panel-overlay.open .slide-panel{transform:translateX(0);}
        .panel-header{padding:16px 20px;border-bottom:1px solid var(--border);display:flex;justify-content:space-between;align-items:center;font-weight:600;}
        .panel-close{background:none;border:none;font-size:1.8rem;cursor:pointer;color:var(--text-light);}
        .panel-items{flex:1;overflow-y:auto;padding:12px 16px;}
        .panel-footer{padding:16px 20px;border-top:1px solid var(--border);background:#fafafa;}
        /* Login modal */
        .login-form input{width:100%;padding:10px;margin-bottom:10px;border:1px solid var(--border);border-radius:6px;background:var(--white);color:var(--text);}
        /* Dark mode override */
        body.dark .modal-content, body.dark .slide-panel, body.dark header, body.dark .toolbar { background: #16213e; }
        body.dark .category-btn { background:#2a2a4a; color:#ccc; }
        body.dark .category-btn.active { background:var(--primary); color:white; }
        body.dark .skeleton { background: linear-gradient(90deg, #2a2a4a 25%, #1a1a2e 50%, #2a2a4a 75%); }
        /* Compare Modal Table */
        .compare-table { width:100%; border-collapse:collapse; }
        .compare-table th, .compare-table td { padding:8px; border:1px solid var(--border); text-align:center; }
        @media(max-width:600px){ .product-grid{grid-template-columns:repeat(2,1fr)} }
    </style>
</head>
<body>
    <div class="toast-container" id="toastContainer"></div>

    <!-- Header -->
    <header>
        <a href="#" class="logo"><i class="fas fa-book-open"></i> KitabShop Ultimate</a>
        <div class="search-box">
            <input type="text" id="searchInput" placeholder="Cari kitab...">
            <button id="searchBtn"><i class="fas fa-search"></i></button>
        </div>
        <div class="header-actions">
            <span class="btn-icon" id="darkToggle" title="Dark Mode"><i class="fas fa-moon"></i></span>
            <span class="btn-icon" id="wishlistToggle"><i class="fas fa-heart"></i><span class="badge" id="wishlistCount">0</span></span>
            <span class="btn-icon" id="cartToggle"><i class="fas fa-shopping-cart"></i><span class="badge" id="cartCount">0</span></span>
            <span class="btn-icon" id="userToggle" title="Login"><i class="fas fa-user"></i></span>
            <span class="btn-icon" id="historyToggle" title="Riwayat"><i class="fas fa-history"></i></span>
            <span id="userGreeting" style="font-size:0.85rem; margin-left:8px;"></span>
        </div>
    </header>

    <!-- Breadcrumb -->
    <div class="breadcrumb" id="breadcrumb"><a href="#">Beranda</a> / <span id="breadcrumbCat">Semua</span></div>

    <!-- Toolbar -->
    <div class="toolbar">
        <div class="categories" id="categoryFilters">
            <button class="category-btn active" data-category="all">Semua</button>
            <button class="category-btn" data-category="tafsir">Tafsir</button>
            <button class="category-btn" data-category="hadits">Hadits</button>
            <button class="category-btn" data-category="fiqih">Fiqih</button>
            <button class="category-btn" data-category="akhlak">Akhlak</button>
        </div>
        <select id="sortSelect" class="sort-select">
            <option value="default">Urutkan: Relevansi</option>
            <option value="price-asc">Harga: Rendah → Tinggi</option>
            <option value="price-desc">Harga: Tinggi → Rendah</option>
            <option value="sold">Terlaris</option>
        </select>
    </div>

    <!-- Product Grid dengan skeleton awal -->
    <section class="product-section">
        <div class="product-grid" id="productGrid"></div>
        <div class="load-more-container" id="loadMoreContainer"></div>
    </section>

    <footer style="text-align:center; padding:20px; background:var(--white); color:#888; margin-top:20px;">
        &copy; 2026 KitabShop Ultimate
    </footer>

    <!-- Modals -->
    <div class="modal" id="productModal"><div class="modal-content" id="modalContent"></div></div>
    <div class="modal" id="loginModal"><div class="modal-content" id="loginContent"></div></div>
    <div class="modal" id="checkoutModal"><div class="modal-content" id="checkoutContent"></div></div>
    <div class="modal" id="compareModal"><div class="modal-content" id="compareContent"></div></div>
    <div class="slide-panel-overlay" id="wishlistOverlay"><div class="slide-panel"><div class="panel-header">❤️ Wishlist<button class="panel-close" id="wishlistClose">&times;</button></div><div class="panel-items" id="wishlistItems"></div></div></div>
    <div class="slide-panel-overlay" id="cartOverlay"><div class="slide-panel"><div class="panel-header">🛒 Keranjang<button class="panel-close" id="cartClose">&times;</button></div><div class="panel-items" id="cartItems"></div><div class="panel-footer" id="cartFooter" style="display:none;"></div></div></div>
    <div class="slide-panel-overlay" id="historyOverlay"><div class="slide-panel"><div class="panel-header">📋 Riwayat Pesanan<button class="panel-close" id="historyClose">&times;</button></div><div class="panel-items" id="historyItems"></div></div></div>
    <div class="modal" id="adminModal"><div class="modal-content" id="adminContent"></div></div>

    <script>
        // ==================== DATA PRODUK DENGAN STOK & VARIAN ====================
        const defaultProducts = [
            { id:1, title:"Tafsir Ibnu Katsir (30 Juz)", price:350000, category:"tafsir", sold:120, rating:4.9, images:["https://placehold.co/300x200/eee/999?text=Tafsir+1","https://placehold.co/300x200/eee/999?text=Tafsir+2"], desc:"Tafsir lengkap 30 juz.", stock:10, variants:[{name:"Hard Cover",add:0},{name:"Soft Cover",add:-30000}] },
            { id:2, title:"Shahih Bukhari Lengkap", price:420000, category:"hadits", sold:95, rating:4.8, images:["https://placehold.co/300x200/eee/999?text=Bukhari+1","https://placehold.co/300x200/eee/999?text=Bukhari+2"], desc:"Kumpulan hadits shahih.", stock:5, variants:[{name:"Jilid Biasa",add:0},{name:"Hardcover",add:50000}] },
            { id:3, title:"Riyadhus Shalihin", price:180000, category:"hadits", sold:210, rating:4.7, images:["https://placehold.co/300x200/eee/999?text=Riyadhus"], desc:"Hadits pilihan Imam Nawawi.", stock:8, variants:[{name:"Kertas Biasa",add:0},{name:"Kertas Premium",add:20000}] },
            { id:4, title:"Fiqih Sunnah Sayyid Sabiq", price:275000, category:"fiqih", sold:88, rating:4.9, images:["https://placehold.co/300x200/eee/999?text=Fiqih+Sunnah"], desc:"Buku fiqih 4 jilid.", stock:3, variants:[] },
            { id:5, title:"Al-Adzkar Imam Nawawi", price:95000, category:"akhlak", sold:150, rating:4.6, images:["https://placehold.co/300x200/eee/999?text=Al-Adzkar"], desc:"Dzikir dan doa.", stock:20, variants:[] },
            { id:6, title:"Tafsir Al-Muyassar", price:220000, category:"tafsir", sold:67, rating:4.8, images:["https://placehold.co/300x200/eee/999?text=Muyassar"], desc:"Tafsir ringkas.", stock:6, variants:[] },
            { id:7, title:"Bulughul Maram", price:155000, category:"hadits", sold:134, rating:4.7, images:["https://placehold.co/300x200/eee/999?text=Bulughul"], desc:"Hadits hukum.", stock:12, variants:[] },
            { id:8, title:"Minhajul Muslim", price:200000, category:"fiqih", sold:76, rating:4.5, images:["https://placehold.co/300x200/eee/999?text=Minhajul"], desc:"Panduan muslim.", stock:2, variants:[] },
            { id:9, title:"Ayyuhal Walad", price:45000, category:"akhlak", sold:300, rating:4.9, images:["https://placehold.co/300x200/eee/999?text=Ayyuhal"], desc:"Wasiat Imam Ghazali.", stock:25, variants:[] },
            { id:10, title:"Syarah Arba'in Nawawi", price:120000, category:"hadits", sold:112, rating:4.8, images:["https://placehold.co/300x200/eee/999?text=Arbain"], desc:"Penjelasan 40 hadits.", stock:7, variants:[] },
        ];

        // Load produk dari localStorage jika ada (admin edit)
        let products = JSON.parse(localStorage.getItem('kitab_products')) || defaultProducts;
        // Pastikan properti variants dan stock ada untuk produk lama
        products = products.map(p => ({ ...{variants:[], stock:10}, ...p }));
        localStorage.setItem('kitab_products', JSON.stringify(products));

        // State
        let cart = JSON.parse(localStorage.getItem('kitab_cart')) || [];
        let wishlist = JSON.parse(localStorage.getItem('kitab_wishlist')) || [];
        let orders = JSON.parse(localStorage.getItem('kitab_orders')) || [];
        let currentUser = JSON.parse(sessionStorage.getItem('currentUser')) || null; // {username, role}
        let compareList = JSON.parse(localStorage.getItem('kitab_compare')) || [];
        let currentCategory = 'all', searchTerm = '', sortBy = 'default', currentPage = 1, itemsPerPage = 8;
        const coupons = {'SANTRI10':10, 'KITAB20':20};
        let couponCode = '';

        // ==================== HELPERS ====================
        function formatRupiah(n) { return 'Rp'+n.toLocaleString('id-ID'); }
        function save(k, v) { localStorage.setItem(k, JSON.stringify(v)); }
        function toast(msg, type='info') {
            const c = document.getElementById('toastContainer');
            const el = document.createElement('div');
            el.className = `toast ${type}`;
            el.innerHTML = `<i class="fas fa-${type==='success'?'check-circle':'info-circle'}"></i> ${msg}`;
            c.appendChild(el);
            setTimeout(()=>el.remove(),3000);
        }
        function updateBadges() {
            document.getElementById('cartCount').textContent = cart.reduce((s,i)=>s+i.qty,0);
            document.getElementById('wishlistCount').textContent = wishlist.length;
        }
        function loginRequired() { if(!currentUser) { openLogin(); toast('Silakan login terlebih dahulu','info'); return false; } return true; }

        // ==================== PAGINATION & RENDER ====================
        function getFiltered() {
            let arr = products.filter(p=>{
                if(currentCategory!=='all' && p.category!==currentCategory) return false;
                if(searchTerm && !p.title.toLowerCase().includes(searchTerm.toLowerCase())) return false;
                return true;
            });
            if(sortBy==='price-asc') arr.sort((a,b)=>a.price-b.price);
            else if(sortBy==='price-desc') arr.sort((a,b)=>b.price-a.price);
            else if(sortBy==='sold') arr.sort((a,b)=>b.sold-a.sold);
            return arr;
        }
        function renderProducts(resetPage=true) {
            if(resetPage) currentPage=1;
            const filtered = getFiltered();
            const totalPages = Math.ceil(filtered.length/itemsPerPage);
            const start = 0, end = currentPage*itemsPerPage;
            const pageItems = filtered.slice(0, end);
            const grid = document.getElementById('productGrid');
            if(pageItems.length===0) {
                grid.innerHTML = '<p style="text-align:center;width:100%;padding:2rem;">Produk tidak ditemukan.</p>';
                document.getElementById('loadMoreContainer').innerHTML = '';
                return;
            }
            grid.innerHTML = pageItems.map(p=>{
                const liked = wishlist.some(w=>w.id===p.id);
                const inCompare = compareList.some(c=>c.id===p.id);
                return `
                <div class="product-card" onclick="openDetail(${p.id})">
                    <div class="wishlist-icon ${liked?'liked':''}" onclick="event.stopPropagation(); toggleWishlist(${p.id})"><i class="fas fa-heart"></i></div>
                    <img src="${p.images[0]}" class="product-img" loading="lazy" onerror="this.src='https://placehold.co/300x200/eee/999?text=No+Image'">
                    <div class="product-info">
                        <div class="product-title">${p.title}</div>
                        <div class="product-price">${formatRupiah(p.price)}</div>
                        <div class="product-meta"><span>⭐${p.rating}</span><span>Terjual ${p.sold}</span></div>
                        ${p.stock<5?`<div class="stock-info">Stok tinggal ${p.stock}</div>`:''}
                        <button class="add-to-cart-btn" onclick="event.stopPropagation(); promptVariant(${p.id})"><i class="fas fa-cart-plus"></i> Keranjang</button>
                        <button class="compare-btn ${inCompare?'active':''}" onclick="event.stopPropagation(); toggleCompare(${p.id})"><i class="fas fa-balance-scale"></i> ${inCompare?'Bandingkan':'Bandingkan'}</button>
                    </div>
                </div>`;
            }).join('');
            document.getElementById('loadMoreContainer').innerHTML = (currentPage*itemsPerPage < filtered.length) 
                ? '<button class="load-more-btn" id="loadMoreBtn">Muat Lebih Banyak</button>'
                : '';
            document.getElementById('loadMoreBtn')?.addEventListener('click', ()=>{ currentPage++; renderProducts(false); });
            document.getElementById('breadcrumbCat').textContent = currentCategory==='all'?'Semua':currentCategory;
        }

        // ==================== SKELETON ====================
        function showSkeleton() {
            document.getElementById('productGrid').innerHTML = Array(8).fill(0).map(()=>`
                <div class="product-card skeleton-item">
                    <div class="product-img skeleton"></div>
                    <div class="product-info"><div class="skeleton" style="height:16px; width:80%; margin-bottom:8px;"></div>
                    <div class="skeleton" style="height:20px; width:50%;"></div></div>
                </div>`).join('');
        }
        // Panggil skeleton saat reload pertama (optional)
        // showSkeleton(); setTimeout(renderProducts, 1000); // untuk simulasi

        // ==================== WISHLIST ====================
        function toggleWishlist(id) {
            const idx = wishlist.findIndex(w=>w.id===id);
            if(idx>-1) { wishlist.splice(idx,1); toast('Dihapus dari wishlist'); }
            else { const p = products.find(x=>x.id===id); wishlist.push({...p}); toast('❤️ Ditambah ke wishlist'); }
            save('kitab_wishlist',wishlist); updateBadges(); renderWishlist(); renderProducts(false);
        }
        function renderWishlist() {
            const c = document.getElementById('wishlistItems');
            c.innerHTML = wishlist.length===0?'<p style="text-align:center;color:#aaa;">Kosong</p>': wishlist.map(w=>`
                <div style="display:flex; gap:12px; padding:12px 0; border-bottom:1px solid var(--border);">
                    <img src="${w.images[0]}" width="50" style="border-radius:4px;">
                    <div style="flex:1"><strong>${w.title}</strong><br>${formatRupiah(w.price)}</div>
                    <button class="btn-text" onclick="toggleWishlist(${w.id})" style="color:#e74c3c; background:none; border:none; cursor:pointer;">Hapus</button>
                </div>`).join('');
        }

        // ==================== KERANJANG & VARIAN ====================
        function promptVariant(productId) {
            const p = products.find(x=>x.id===productId);
            if(!p) return;
            if(p.variants.length>0) {
                // Pilih varian sederhana: langsung pakai varian pertama, atau bisa buat modal. Untuk demo, kita pakai varian pertama.
                // Biar sederhana, kita langsung add dengan varian default index 0.
                addToCart(productId, 0);
            } else addToCart(productId);
        }
        function addToCart(productId, variantIdx=0) {
            if(!loginRequired()) return;
            const p = products.find(x=>x.id===productId);
            if(p.stock<1) { toast('Stok habis!','info'); return; }
            const existing = cart.find(i=>i.id===productId && i.variantIdx===variantIdx);
            if(existing) {
                if(existing.qty >= p.stock) { toast('Stok tidak mencukupi','info'); return; }
                existing.qty++;
            } else {
                cart.push({ ...p, qty:1, variantIdx });
            }
            save('kitab_cart',cart); updateBadges(); renderCart(); toast('Ditambahkan ke keranjang','success');
        }
        function updateCartQty(id, variantIdx, newQty) {
            newQty = parseInt(newQty);
            const p = products.find(x=>x.id===id);
            if(newQty<1) { removeCartItem(id, variantIdx); return; }
            if(newQty > p.stock) { toast('Stok maksimal '+p.stock,'info'); newQty = p.stock; }
            const item = cart.find(i=>i.id===id && i.variantIdx===variantIdx);
            if(item) item.qty = newQty;
            save('kitab_cart',cart); renderCart();
        }
        function removeCartItem(id, variantIdx) {
            cart = cart.filter(i=>!(i.id===id && i.variantIdx===variantIdx));
            save('kitab_cart',cart); updateBadges(); renderCart();
        }
        function renderCart() {
            const container = document.getElementById('cartItems');
            const footer = document.getElementById('cartFooter');
            if(cart.length===0) {
                container.innerHTML = '<p style="text-align:center; color:#aaa;">Keranjang kosong</p>';
                footer.style.display='none';
                return;
            }
            footer.style.display='block';
            let subtotal = 0;
            container.innerHTML = cart.map(item=>{
                const p = products.find(x=>x.id===item.id);
                const price = p.variants[item.variantIdx]? p.price + p.variants[item.variantIdx].add : p.price;
                subtotal += price*item.qty;
                return `
                <div style="display:flex; gap:12px; padding:12px 0; border-bottom:1px solid var(--border); align-items:center;">
                    <img src="${item.images[0]}" width="50" style="border-radius:4px;">
                    <div style="flex:1">
                        <strong>${item.title}</strong> ${item.variants[item.variantIdx]?'('+item.variants[item.variantIdx].name+')':''}<br>
                        <span style="color:var(--primary); font-weight:bold;">${formatRupiah(price)}</span>
                        <div style="display:flex; gap:4px; margin-top:4px;">
                            <button class="qty-btn" onclick="updateCartQty(${item.id},${item.variantIdx},${item.qty-1})">-</button>
                            <input value="${item.qty}" size="2" style="text-align:center;" onchange="updateCartQty(${item.id},${item.variantIdx},this.value)">
                            <button class="qty-btn" onclick="updateCartQty(${item.id},${item.variantIdx},${item.qty+1})">+</button>
                        </div>
                    </div>
                    <button style="color:red; background:none; border:none; cursor:pointer;" onclick="removeCartItem(${item.id},${item.variantIdx})">Hapus</button>
                </div>`;
            }).join('');
            document.getElementById('cartTotal')?.remove();
            const totalEl = document.createElement('div');
            totalEl.id='cartTotal';
            totalEl.innerHTML = `<strong>Total: ${formatRupiah(subtotal)}</strong>`;
            footer.appendChild(totalEl);
            if(!document.getElementById('couponSection')) {
                const coup = document.createElement('div');
                coup.id='couponSection';
                coup.innerHTML = `<input id="couponInput" placeholder="Kode kupon" style="width:60%; padding:6px;"><button onclick="applyCoupon()" style="padding:6px 12px;">Pakai</button><span id="couponMsg"></span>`;
                footer.appendChild(coup);
            }
        }

        function applyCoupon() {
            const code = document.getElementById('couponInput').value.trim().toUpperCase();
            if(coupons[code]) {
                couponCode = code;
                document.getElementById('couponMsg').textContent = `Diskon ${coupons[code]}% diterapkan!`;
                toast('Kupon berhasil!','success');
            } else {
                couponCode = '';
                document.getElementById('couponMsg').textContent = 'Kode tidak valid';
            }
        }

        // ==================== CHECKOUT ====================
        function checkout() {
            if(cart.length===0) return;
            if(!loginRequired()) return;
            const subtotal = cart.reduce((s,item)=>{
                const p = products.find(x=>x.id===item.id);
                return s + (p.price + (p.variants[item.variantIdx]?.add||0))*item.qty;
            },0);
            const discount = couponCode ? Math.round(subtotal*coupons[couponCode]/100) : 0;
            const shippingCost = document.getElementById('shippingOption')?.value==='exp'?30000:15000;
            const total = subtotal - discount + shippingCost;
            document.getElementById('checkoutContent').innerHTML = `
                <button class="modal-close" onclick="document.getElementById('checkoutModal').classList.remove('open')">&times;</button>
                <h3>Checkout</h3>
                <p>Subtotal: ${formatRupiah(subtotal)}</p>
                <p>Diskon: ${formatRupiah(discount)}</p>
                <select id="shippingOption" style="width:100%; padding:8px; margin:8px 0;">
                    <option value="reg">Reguler (Rp15.000)</option>
                    <option value="exp">Express (Rp30.000)</option>
                </select>
                <p>Ongkir: <span id="shippingFee">${formatRupiah(shippingCost)}</span></p>
                <p><strong>Total: ${formatRupiah(total)}</strong></p>
                <div id="checkoutError" style="color:red;"></div>
                <input id="checkoutName" placeholder="Nama" required>
                <input id="checkoutEmail" placeholder="Email" required>
                <textarea id="checkoutAddress" placeholder="Alamat" required></textarea>
                <button class="add-to-cart-btn" id="confirmCheckout">Bayar</button>
            `;
            document.getElementById('checkoutModal').classList.add('open');
            document.getElementById('shippingOption').addEventListener('change', function(){
                const sc = this.value==='exp'?30000:15000;
                document.getElementById('shippingFee').textContent = formatRupiah(sc);
            });
            document.getElementById('confirmCheckout').addEventListener('click', ()=>{
    const name = document.getElementById('checkoutName').value.trim();
    const email = document.getElementById('checkoutEmail').value.trim();
    const address = document.getElementById('checkoutAddress').value.trim();
    const err = document.getElementById('checkoutError');
    if(!name || name.length<3) { err.textContent='Nama minimal 3 karakter'; return; }
    if(!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email)) { err.textContent='Email tidak valid'; return; }
    if(!address) { err.textContent='Alamat wajib'; return; }

    // Susun pesan WhatsApp
    let waText = `Halo, saya mau pesan:%0A%0A`;
    let subtotalWA = 0;
    cart.forEach((item, index) => {
        const p = products.find(x => x.id === item.id);
        const price = p.price + (p.variants[item.variantIdx]?.add || 0);
        subtotalWA += price * item.qty;
        waText += `${index+1}. ${item.title} (${item.variants[item.variantIdx]?.name || 'Standar'}) x${item.qty} = ${formatRupiah(price * item.qty)}%0A`;
    });
    const discountWA = couponCode ? Math.round(subtotalWA * coupons[couponCode] / 100) : 0;
    const shippingOption = document.getElementById('shippingOption')?.value;
    const shippingCost = shippingOption === 'exp' ? 30000 : 15000;
    const totalWA = subtotalWA - discountWA + shippingCost;
    waText += `%0ASubtotal: ${formatRupiah(subtotalWA)}`;
    if(discountWA) waText += `%0ADiskon: -${formatRupiah(discountWA)}`;
    waText += `%0AOngkir (${shippingOption==='exp'?'Express':'Reguler'}): ${formatRupiah(shippingCost)}`;
    waText += `%0ATotal: ${formatRupiah(totalWA)}%0A%0A`;
    waText += `Nama: ${name}%0AEmail: ${email}%0AAlamat: ${address}%0A%0A`;
    waText += `Mohon diproses ya. Terima kasih.`;

    // Nomor WhatsApp tujuan (GANTI dengan nomormu)
    const nomorWA = '6281234567890'; // Contoh: 6285xxxxxx tanpa + atau spasi
    const waLink = `https://wa.me/${nomorWA}?text=${waText}`;

    // Kurangi stok (tetap di browser admin)
    cart.forEach(item=>{
        const p = products.find(x=>x.id===item.id);
        if(p) p.stock = Math.max(0, p.stock - item.qty);
    });
    save('kitab_products', products);
    // Simpan order untuk riwayat
    orders.push({ date: new Date().toLocaleString(), items: [...cart], total: totalWA, name, email, address });
    save('kitab_orders', orders);
    cart = []; couponCode = '';
    save('kitab_cart', cart); updateBadges(); renderCart();
    document.getElementById('checkoutModal').classList.remove('open');
    toast('✅ Pesanan dikirim ke WhatsApp!','success');
    // Buka WhatsApp
    window.open(waLink, '_blank');
    renderProducts(false);
});
        }

        // ==================== RIWAYAT ====================
        function renderHistory() {
            const c = document.getElementById('historyItems');
            if(orders.length===0) c.innerHTML = '<p style="text-align:center;">Belum ada pesanan.</p>';
            else c.innerHTML = orders.map(o=>`
                <div style="border-bottom:1px solid var(--border); padding:8px 0;">
                    <strong>${o.date}</strong> - Total: ${formatRupiah(o.total)}<br>
                    <small>${o.items.map(i=>i.title).join(', ')}</small>
                </div>`).join('');
        }

        // ==================== DETAIL & REVIEW ====================
        function openDetail(id) {
            const p = products.find(x=>x.id===id);
            const reviews = JSON.parse(localStorage.getItem('reviews_'+id)) || [];
            document.getElementById('modalContent').innerHTML = `
                <button class="modal-close" onclick="closeDetail()">&times;</button>
                <div class="gallery">${p.images.map((img,i)=>`<img src="${img}" class="${i===0?'active':''}" onclick="this.parentNode.querySelectorAll('img').forEach(im=>im.classList.remove('active'));this.classList.add('active');document.getElementById('mainImg').src='${img}'">`).join('')}</div>
                <img id="mainImg" src="${p.images[0]}" style="width:100%; border-radius:8px; margin-bottom:12px;">
                <h2>${p.title}</h2>
                <p style="color:var(--primary); font-weight:bold;">${formatRupiah(p.price)}</p>
                <p>Stok: ${p.stock}</p>
                <div>Varian: ${p.variants.map((v,i)=>`<span class="variant-option" onclick="selectVariant(${p.id},${i})">${v.name} ${v.add!==0?'('+(v.add>0?'+':'')+formatRupiah(v.add)+')':''}</span>`).join('')}</div>
                <p>${p.desc}</p>
                <button class="add-to-cart-btn" onclick="addToCart(${p.id},0)">Tambah ke Keranjang</button>
                <h3>Review (${reviews.length})</h3>
                <div id="reviewList">${reviews.map(r=>`<p><strong>${r.user}</strong> ⭐${r.rating} - ${r.comment}</p>`).join('')}</div>
                ${currentUser?`
                <div>
                    <select id="reviewRating"><option>5</option><option>4</option><option>3</option></select>
                    <input id="reviewComment" placeholder="Komentar"><button onclick="submitReview(${id})">Kirim</button>
                </div>`:'<p>Login untuk review.</p>'}
            `;
            document.getElementById('productModal').classList.add('open');
        }
        function selectVariant(id, idx) { /* simpan pilihan di detail saja, tidak digunakan di sini */ }
        function submitReview(id) {
            const rating = document.getElementById('reviewRating').value;
            const comment = document.getElementById('reviewComment').value;
            const reviews = JSON.parse(localStorage.getItem('reviews_'+id)) || [];
            reviews.push({ user: currentUser.username, rating, comment });
            save('reviews_'+id, reviews);
            toast('Review terkirim!','success');
            openDetail(id);
        }
        function closeDetail() { document.getElementById('productModal').classList.remove('open'); }

        // ==================== COMPARE ====================
        function toggleCompare(id) {
            const idx = compareList.findIndex(c=>c.id===id);
            if(idx>-1) compareList.splice(idx,1);
            else {
                if(compareList.length>=3) { toast('Maksimal 3 produk','info'); return; }
                compareList.push(products.find(p=>p.id===id));
            }
            save('kitab_compare',compareList);
            renderProducts(false);
            renderCompareModal();
        }
        function renderCompareModal() {
            if(compareList.length===0) document.getElementById('compareContent').innerHTML = 'Tidak ada produk.';
            else {
                let html = '<table class="compare-table"><tr><th>Judul</th>'+compareList.map(p=>`<th>${p.title}</th>`).join('')+'</tr>';
                html += '<tr><td>Harga</td>'+compareList.map(p=>`<td>${formatRupiah(p.price)}</td>`).join('')+'</tr>';
                html += '<tr><td>Rating</td>'+compareList.map(p=>`<td>⭐${p.rating}</td>`).join('')+'</tr>';
                html += '<tr><td>Stok</td>'+compareList.map(p=>`<td>${p.stock}</td>`).join('')+'</tr></table>';
                document.getElementById('compareContent').innerHTML = html;
            }
            document.getElementById('compareModal').classList.add('open');
        }

        // ==================== LOGIN & REGISTER ====================
        function openLogin() {
            document.getElementById('loginContent').innerHTML = `
                <button class="modal-close" onclick="document.getElementById('loginModal').classList.remove('open')">&times;</button>
                <h3>Login</h3>
                <input id="loginUsername" placeholder="Username"><br>
                <input id="loginPassword" type="password" placeholder="Password"><br>
                <button onclick="login()">Login</button>
                <p>Belum punya akun? <a href="#" onclick="registerForm()">Daftar</a></p>
                <p style="font-size:0.8rem;">Admin demo: admin / admin123</p>
            `;
            document.getElementById('loginModal').classList.add('open');
        }
        function registerForm() {
            document.getElementById('loginContent').innerHTML = `
                <button class="modal-close" onclick="document.getElementById('loginModal').classList.remove('open')">&times;</button>
                <h3>Daftar</h3>
                <input id="regUsername" placeholder="Username"><br>
                <input id="regPassword" type="password" placeholder="Password"><br>
                <button onclick="register()">Daftar</button>
            `;
        }
        function register() {
            const u = document.getElementById('regUsername').value.trim();
            const p = document.getElementById('regPassword').value.trim();
            if(!u||!p) return;
            let users = JSON.parse(localStorage.getItem('kitab_users')) || [];
            if(users.find(x=>x.username===u)) { toast('Username sudah ada'); return; }
            users.push({username:u, password:p, role:'user'});
            save('kitab_users', users);
            toast('Pendaftaran berhasil, silakan login','success');
            openLogin();
        }
        function login() {
            const u = document.getElementById('loginUsername').value.trim();
            const p = document.getElementById('loginPassword').value.trim();
            const users = JSON.parse(localStorage.getItem('kitab_users')) || [];
            // Admin default
            if(u==='admin' && p==='admin123') {
                currentUser = {username:'admin', role:'admin'};
                sessionStorage.setItem('currentUser', JSON.stringify(currentUser));
                updateUserUI(); document.getElementById('loginModal').classList.remove('open');
                toast('Login sebagai Admin','success'); return;
            }
            const user = users.find(x=>x.username===u && x.password===p);
            if(user) {
                currentUser = {username:user.username, role:user.role||'user'};
                sessionStorage.setItem('currentUser', JSON.stringify(currentUser));
                updateUserUI(); document.getElementById('loginModal').classList.remove('open');
                toast('Login berhasil','success');
            } else toast('Username/password salah','info');
        }
        function logout() {
            currentUser = null;
            sessionStorage.removeItem('currentUser');
            updateUserUI();
            toast('Logout berhasil');
            document.getElementById('adminModal').classList.remove('open');
        }
        function updateUserUI() {
            const greet = document.getElementById('userGreeting');
            if(currentUser) {
                greet.textContent = `Hi, ${currentUser.username}`;
                if(currentUser.role==='admin') greet.innerHTML += ' <a href="#" onclick="openAdmin()">[Dashboard]</a>';
                greet.innerHTML += ' <a href="#" onclick="logout()">(Logout)</a>';
            } else greet.textContent = '';
        }

        // ==================== ADMIN DASHBOARD ====================
        function openAdmin() {
            if(!currentUser || currentUser.role!=='admin') return;
            const pList = products.map(p=>`
                <tr>
                    <td>${p.title}</td><td>${formatRupiah(p.price)}</td><td>${p.stock}</td>
                    <td><button onclick="editProduct(${p.id})">Edit</button> <button onclick="deleteProduct(${p.id})">Hapus</button></td>
                </tr>`).join('');
            document.getElementById('adminContent').innerHTML = `
                <button class="modal-close" onclick="document.getElementById('adminModal').classList.remove('open')">&times;</button>
                <h3>Dashboard Admin</h3>
                <h4>Produk</h4>
                <table border="1" width="100%"><tr><th>Judul</th><th>Harga</th><th>Stok</th><th>Aksi</th></tr>${pList}</table>
                <button onclick="addProductForm()">Tambah Produk</button>
                <h4>Pesanan Masuk</h4>
                ${orders.map(o=>`<div>${o.date} - ${o.name} - Total: ${formatRupiah(o.total)}</div>`).join('')||'Belum ada'}
            `;
            document.getElementById('adminModal').classList.add('open');
        }
        function addProductForm() {
            // Implementasi sederhana prompt
            const title = prompt('Judul');
            const price = parseInt(prompt('Harga'));
            const category = prompt('Kategori (tafsir/hadits/fiqih/akhlak)');
            const stock = parseInt(prompt('Stok'))||10;
            if(title && price) {
                const newId = Math.max(...products.map(p=>p.id))+1;
                products.push({id:newId, title, price, category, sold:0, rating:5.0, images:['https://placehold.co/300x200'], desc:'', stock, variants:[]});
                save('kitab_products', products);
                toast('Produk ditambah','success');
                openAdmin();
                renderProducts();
            }
        }
        window.editProduct = function(id) {
            const p = products.find(x=>x.id===id);
            const title = prompt('Judul', p.title);
            const price = parseInt(prompt('Harga', p.price));
            const stock = parseInt(prompt('Stok', p.stock));
            if(title && price) {
                p.title = title; p.price = price; p.stock = stock;
                save('kitab_products', products);
                toast('Produk diupdate','success');
                openAdmin(); renderProducts();
            }
        };
        window.deleteProduct = function(id) {
            products = products.filter(p=>p.id!==id);
            save('kitab_products', products);
            toast('Produk dihapus','success');
            openAdmin(); renderProducts();
        };

        // ==================== DARK MODE ====================
        if(localStorage.getItem('darkMode')==='true') document.body.classList.add('dark');
        document.getElementById('darkToggle').addEventListener('click', ()=>{
            document.body.classList.toggle('dark');
            localStorage.setItem('darkMode', document.body.classList.contains('dark'));
        });

        // ==================== EVENT LISTENERS ====================
        document.getElementById('cartToggle').onclick = ()=>document.getElementById('cartOverlay').classList.add('open');
        document.getElementById('cartClose').onclick = ()=>document.getElementById('cartOverlay').classList.remove('open');
        document.getElementById('wishlistToggle').onclick = ()=>document.getElementById('wishlistOverlay').classList.add('open');
        document.getElementById('wishlistClose').onclick = ()=>document.getElementById('wishlistOverlay').classList.remove('open');
        document.getElementById('historyToggle').onclick = ()=>{ document.getElementById('historyOverlay').classList.add('open'); renderHistory(); };
        document.getElementById('historyClose').onclick = ()=>document.getElementById('historyOverlay').classList.remove('open');
        document.getElementById('userToggle').onclick = ()=>{ if(!currentUser) openLogin(); else logout(); };
        document.getElementById('searchInput').addEventListener('input', (e)=>{ searchTerm=e.target.value; renderProducts(); });
        document.getElementById('sortSelect').addEventListener('change', (e)=>{ sortBy=e.target.value; renderProducts(); });
        document.querySelectorAll('.category-btn').forEach(b=>b.addEventListener('click', function(){
            document.querySelectorAll('.category-btn').forEach(x=>x.classList.remove('active'));
            this.classList.add('active');
            currentCategory = this.dataset.category;
            renderProducts();
        }));
        window.addEventListener('click', (e)=>{
            if(e.target.classList.contains('slide-panel-overlay')) e.target.classList.remove('open');
            if(e.target.classList.contains('modal') && e.target.id!=='checkoutModal') e.target.classList.remove('open');
        });

        // Tambahkan tombol checkout di panel keranjang
        const cartFooterDefault = document.getElementById('cartFooter');
        cartFooterDefault.innerHTML += '<button class="add-to-cart-btn" onclick="checkout()" style="width:100%; margin-top:8px;">Checkout</button>';

        // ==================== PWA ====================
        if('serviceWorker' in navigator) {
            const manifest = {
                name: "KitabShop Ultimate",
                short_name: "KitabShop",
                start_url: ".",
                display: "standalone",
                background_color: "#ffffff",
                theme_color: "#ee4d2d",
                icons: [{src:"https://placehold.co/192x192/ee4d2d/white?text=KitabShop", sizes:"192x192", type:"image/png"}]
            };
            const blob = new Blob([JSON.stringify(manifest)], {type:'application/json'});
            document.getElementById('manifest-link').href = URL.createObjectURL(blob);
            navigator.serviceWorker.register('data:text/javascript,'+encodeURIComponent(
                "self.addEventListener('fetch',e=>{"+
                "e.respondWith(caches.match(e.request).then(r=>r||fetch(e.request)))})"
            )).catch(()=>{});
        }

        // ==================== INIT ====================
        renderProducts();
        updateBadges();
        renderWishlist();
        renderCart();
        updateUserUI();
    </script>
</body>
</html>
