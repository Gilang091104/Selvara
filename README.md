<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Selvara - Jual Beli Barang Bekas & Baru</title>
  <style>
    body {
      font-family: 'Segoe UI', sans-serif;
      margin: 0;
      background: #f4f4f4;
    }

    header {
      background: #2a9d8f;
      padding: 1.5rem;
      text-align: center;
      color: white;
    }

    .search-bar {
      padding: 1rem;
      text-align: center;
      background: #e9ecef;
    }

    .search-bar input {
      width: 90%;
      max-width: 400px;
      padding: 0.6rem;
      font-size: 1rem;
      border: 1px solid #ccc;
      border-radius: 8px;
    }

    .container {
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      padding: 2rem;
      gap: 1rem;
    }

    .card {
      background: white;
      border-radius: 8px;
      box-shadow: 0 2px 8px rgba(0,0,0,0.1);
      width: 250px;
      overflow: hidden;
    }

    .card img {
      width: 100%;
      height: 160px;
      object-fit: cover;
      cursor: pointer;
    }

    .card-content {
      padding: 1rem;
    }

    .card-content h3 {
      margin: 0 0 0.5rem;
    }

    .card-content p {
      font-size: 0.9rem;
      margin-bottom: 1rem;
    }

    .buy-btn {
      background: #2a9d8f;
      color: white;
      border: none;
      padding: 0.6rem 1rem;
      border-radius: 5px;
      cursor: pointer;
    }

    .buy-btn:disabled {
      background: #ccc;
      cursor: not-allowed;
    }

    .sold-label {
      background: #e63946;
      color: white;
      font-weight: bold;
      padding: 0.4rem 0.6rem;
      border-radius: 4px;
      font-size: 0.9rem;
    }

    footer {
      background: #264653;
      color: white;
      text-align: center;
      padding: 1rem;
    }

    /* Modal galeri */
    .modal {
      display: none;
      position: fixed;
      z-index: 999;
      left: 0;
      top: 0;
      width: 100vw;
      height: 100vh;
      background-color: rgba(0, 0, 0, 0.8);
      justify-content: center;
      align-items: center;
      flex-direction: column;
    }

    .modal img {
      width: 90%;
      max-width: 400px;
      margin: 1rem;
      border-radius: 10px;
      box-shadow: 0 0 10px #000;
    }

    .close-btn {
      position: absolute;
      top: 20px;
      right: 30px;
      font-size: 2rem;
      color: white;
      cursor: pointer;
      font-weight: bold;
    }
  </style>
</head>
<body>

  <header>
    <h1>Selvara</h1>
    <p>Jual Beli Barang Bekas & Baru</p>
  </header>

  <div class="search-bar">
    <input type="text" id="searchInput" placeholder="Cari produk...">
  </div>

  <div class="container" id="product-list">
    <!-- Produk akan muncul di sini -->
  </div>

  <!-- Modal Galeri -->
  <div id="modal" class="modal">
    <span class="close-btn" onclick="closeModal()">✖</span>
    <div id="modal-images"></div>
  </div>

  <footer>
    <p>Hubungi Admin: <a href="https://wa.me/6281234567890" style="color:#fff; text-decoration: underline;">+62 812-3456-7890</a></p>
    <p>&copy; 2025 Selvara</p>
  </footer>

  <script>
    const adminWa = '6281234567890';

    const products = [
      {
        id: 1,
        name: "Handphone Bekas",
        price: "Rp 1.000.000",
        sold: false,
        images: [
          "https://via.placeholder.com/400x250?text=HP+1",
          "https://via.placeholder.com/400x250?text=HP+2",
          "https://via.placeholder.com/400x250?text=HP+3"
        ]
      },
      {
        id: 2,
        name: "Kipas Angin Baru",
        price: "Rp 250.000",
        sold: true,
        images: [
          "https://via.placeholder.com/400x250?text=Kipas+1",
          "https://via.placeholder.com/400x250?text=Kipas+2",
          "https://via.placeholder.com/400x250?text=Kipas+3"
        ]
      },
      {
        id: 3,
        name: "Laptop Second",
        price: "Rp 2.500.000",
        sold: false,
        images: [
          "https://via.placeholder.com/400x250?text=Laptop+1",
          "https://via.placeholder.com/400x250?text=Laptop+2",
          "https://via.placeholder.com/400x250?text=Laptop+3"
        ]
      }
    ];

    function renderProducts(filter = "") {
      const list = document.getElementById("product-list");
      list.innerHTML = "";

      const filtered = products.filter(p =>
        p.name.toLowerCase().includes(filter.toLowerCase())
      );

      filtered.forEach(product => {
        const card = document.createElement("div");
        card.className = "card";

        const soldText = product.sold ? `<span class="sold-label">SOLD</span>` : "";

        card.innerHTML = `
          <img src="${product.images[0]}" alt="${product.name}" onclick="showGallery(${product.id})" />
          <div class="card-content">
            <h3>${product.name}</h3>
            <p>${product.price}</p>
            ${soldText}
            <button class="buy-btn" ${product.sold ? 'disabled' : ''} onclick="buyProduct('${product.name}', '${product.price}')">
              ${product.sold ? 'Tidak Tersedia' : 'Beli Sekarang'}
            </button>
          </div>
        `;

        list.appendChild(card);
      });

      if (filtered.length === 0) {
        list.innerHTML = `<p>Tidak ada produk ditemukan.</p>`;
      }
    }

    function buyProduct(name, price) {
      const message = `Halo, saya ingin membeli ${name} dengan harga ${price}`;
      window.open(`https://wa.me/${adminWa}?text=${encodeURIComponent(message)}`, '_blank');
    }

    function showGallery(id) {
      const product = products.find(p => p.id === id);
      const modal = document.getElementById("modal");
      const modalImages = document.getElementById("modal-images");

      modalImages.innerHTML = "";
      product.images.forEach(url => {
        const img = document.createElement("img");
        img.src = url;
        modalImages.appendChild(img);
      });

      modal.style.display = "flex";
    }

    function closeModal() {
      document.getElementById("modal").style.display = "none";
    }

    document.getElementById("searchInput").addEventListener("input", function() {
      renderProducts(this.value);
    });

    renderProducts();
  </script>

</body>
</html>
