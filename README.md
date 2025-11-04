# waroengmasheride<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <meta name="mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <title>WAROENG MASHER - Panel Kasir (POS) Mobile</title>
    
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css" rel="stylesheet">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.3/font/bootstrap-icons.min.css">
    
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Lora:wght@400;700&family=Montserrat:wght@400;500;700&display=swap" rel="stylesheet">

    <style>
        /* GAYA UNTUK PANEL KASIR (POS) MOBILE */
        html, body { 
            font-family: 'Montserrat', sans-serif; 
            background-color: #1A1A1A; /* Latar belakang sangat gelap */
            color: #E0E0E0;
            height: 100%; /* Gunakan height 100% untuk html dan body */
            margin: 0;
            padding: 0;
            overflow-x: hidden; /* Mencegah scroll horizontal */
            touch-action: pan-y; /* Optimasi untuk sentuhan */
        }
        body {
            display: flex; /* Menggunakan Flexbox untuk full height */
            flex-direction: column;
        }
        h1, h2, h3, h4, h5, h6, .navbar-brand { 
            font-family: 'Lora', serif; 
            font-weight: 700; 
            color: #FFFFFF;
        }
        .navbar-brand { 
            color: #FFD700 !important; /* Brand dengan warna emas/kuning cerah */
            font-size: 1.5rem;
            letter-spacing: 1px;
        }
        .navbar {
            background-color: #282828 !important; /* Navbar gelap */
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
            padding: 0.5rem 1rem;
            flex-shrink: 0; /* Mencegah navbar menyusut */
        }
        /* Konten utama untuk tata letak mobile */
        #pos-content {
            display: flex;
            flex-direction: column; /* Ubah dari flex menjadi kolom untuk mobile */
            flex: 1; /* Konten mengisi sisa ruang */
            overflow: hidden; /* Mencegah konten meluap dari container */
        }
        #menu-panel {
            flex: 1; /* Panel Menu mengisi ruang yang tersedia */
            padding: 0.5rem;
            overflow-y: auto; /* Scrollable jika menu banyak */
            -webkit-overflow-scrolling: touch; /* Scroll halus di iOS */
        }
        #checkout-panel {
            flex-shrink: 0; /* Panel Checkout tidak menyusut */
            background-color: #282828; /* Warna kontras untuk keranjang */
            padding: 0.5rem;
            display: flex;
            flex-direction: column;
            border-top: 2px solid #333333;
        }
        .product-card { 
            border: none;
            background-color: #333333; /* Card lebih gelap dari body */
            transition: transform 0.2s, box-shadow 0.2s;
            border-radius: 8px;
            overflow: hidden; 
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.4);
            cursor: pointer; /* Menandakan bisa diklik */
            position: relative; /* Diperlukan untuk posisi tombol detail */
            margin-bottom: 0.5rem;
        }
        .product-card:hover { 
            transform: translateY(-3px); 
            box-shadow: 0 8px 16px rgba(255, 215, 0, 0.2); 
            border: 1px solid #FFD700;
        }
        .product-card .card-img-top { 
            width: 100%; /* Make the image fill the container width */
            aspect-ratio: 1 / 1; /* Set the aspect ratio to 1:1 (square) */
            object-fit: contain; /* Keep the image from being cropped */
            background-color: #222; /* Background for transparent images */
            padding: 5px; /* Add some space around the image */
        }
        .product-card .card-body { 
            padding: 0.5rem; 
            min-height: 80px; /* Tambahkan tinggi minimum untuk card body */
            display: flex;
            flex-direction: column;
            justify-content: space-between;
        }
        .product-card .card-title { 
            color: #FFD700;
            font-size: 0.9rem;
            margin-bottom: 0.2rem;
            /* Menghapus white-space: nowrap, overflow: hidden, dan text-overflow: ellipsis */
            /* Mengganti dengan animasi scroll untuk teks panjang */
            display: -webkit-box;
            -webkit-line-clamp: 2; /* Maksimal 2 baris */
            -webkit-box-orient: vertical;
            overflow: hidden;
            text-overflow: ellipsis;
            transition: all 0.3s ease;
            min-height: 2.4em; /* Minimal tinggi untuk 2 baris */
        }
        .product-card:hover .card-title {
            -webkit-line-clamp: unset; /* Tampilkan semua baris saat hover */
            max-height: none;
            overflow: visible;
            text-overflow: unset;
            animation: fadeIn 0.3s ease;
        }
        @keyframes fadeIn {
            from { opacity: 0.7; }
            to { opacity: 1; }
        }
        .product-card h6 { 
            color: #FFFFFF; 
            font-size: 1rem;
            margin: 0;
        }
        .section-title {
            position: sticky;
            top: 0;
            background-color: #1A1A1A;
            z-index: 10;
            padding: 10px 0;
            margin-top: -10px;
            font-size: 1.2rem;
        }
        /* Tombol Detail Produk */
        .btn-detail {
            position: absolute;
            top: 5px;
            right: 5px;
            background-color: rgba(40, 40, 40, 0.8);
            color: #FFD700;
            border: none;
            border-radius: 50%;
            width: 30px;
            height: 30px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1rem;
            z-index: 2;
            transition: background-color 0.2s;
        }
        .btn-detail:hover {
            background-color: rgba(0, 0, 0, 0.9);
            color: #FFFFFF;
        }

        /* Styling Keranjang di Panel Kasir Mobile */
        #cart-items-container-pos {
            flex-grow: 1;
            overflow-y: auto;
            padding-right: 5px;
            margin-bottom: 10px;
            max-height: 150px; /* Batasi tinggi maksimum untuk keranjang */
            -webkit-overflow-scrolling: touch; /* Scroll halus di iOS */
        }
        .cart-item-pos {
            border-bottom: 1px solid #333333;
            padding: 8px 0;
        }
        .cart-item-pos h6 {
            color: #E0E0E0;
            font-size: 0.9rem;
        }
        .cart-item-pos .price-qty {
            font-size: 0.8rem;
            color: #B0B0B0;
        }
        .btn-qty {
            width: 30px;
            height: 30px;
            padding: 0;
            font-size: 1rem;
            line-height: 1;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        .btn-success {
            background-color: #4CAF50;
            border-color: #4CAF50;
        }
        .btn-success:hover {
            background-color: #388E3C;
            border-color: #388E3C;
        }
        .btn-primary {
            background-color: #FFD700;
            border-color: #FFD700;
            color: #1A1A1A;
        }
        .btn-primary:hover {
            background-color: #E0B400;
            border-color: #E0B400;
        }
        .payment-summary {
            background-color: #1A1A1A;
            padding: 0.8rem;
            border-radius: 8px;
        }
        .form-control-dark {
            background-color: #333333;
            color: #FFFFFF;
            border: 1px solid #555555;
        }
        .form-control-dark:focus {
            background-color: #333333;
            color: #FFFFFF;
            border-color: #FFD700;
            box-shadow: 0 0 0 0.25rem rgba(255, 215, 0, 0.25);
        }
        /* Gaya untuk tombol diskon */
        .discount-buttons {
            display: flex;
            flex-wrap: wrap;
            gap: 5px;
            margin-bottom: 10px;
        }
        .discount-btn {
            flex: 1;
            min-width: 50px;
            padding: 5px;
            font-size: 0.7rem;
        }
        .discount-btn.active {
            background-color: #FFD700;
            color: #1A1A1A;
            border-color: #FFD700;
        }
        .discount-info {
            display: flex;
            justify-content: space-between;
            margin-bottom: 10px;
            font-size: 0.8rem;
        }
        .original-total {
            text-decoration: line-through;
            color: #999;
        }
        .discount-amount {
            color: #4CAF50;
        }
        /* Gaya Modal */
        .modal-content {
            background-color: #282828;
            color: #E0E0E0;
        }
        .modal-header {
            border-bottom: 1px solid #444;
        }
        .modal-footer {
            border-top: 1px solid #444;
        }
        .btn-close {
            filter: invert(1);
        }
        /* Gaya untuk Receipt/Struk */
        #receipt-content {
            font-family: 'Courier New', Courier, monospace;
            background-color: #fff;
            color: #000;
            padding: 15px;
            width: 100%;
            max-width: 400px;
            margin: 0 auto;
            border-radius: 5px;
        }
        #receipt-content .receipt-header {
            text-align: center;
            border-bottom: 2px dashed #000;
            padding-bottom: 10px;
            margin-bottom: 15px;
        }
        #receipt-content .receipt-header h3 {
            margin: 0;
            font-size: 1.5rem;
            color: #000;
        }
        #receipt-content .receipt-header p {
            margin: 5px 0;
            font-size: 0.8rem;
        }
        #receipt-content .receipt-body {
            margin-bottom: 15px;
        }
        #receipt-content .receipt-item {
            display: flex;
            justify-content: space-between;
            margin-bottom: 5px;
            font-size: 0.9rem;
        }
        #receipt-content .receipt-footer {
            border-top: 2px dashed #000;
            padding-top: 10px;
            text-align: center;
        }
        #receipt-content .receipt-footer p {
            margin: 5px 0;
            font-size: 0.8rem;
        }
        /* Gaya untuk QR dengan gambar struk */
        .qr-receipt-container {
            display: flex;
            flex-direction: column;
            align-items: center;
            margin: 20px 0;
            padding: 15px;
            background-color: #f8f9fa;
            border-radius: 10px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
        }
        .qr-receipt-title {
            font-size: 1rem;
            font-weight: bold;
            color: #333;
            margin-bottom: 15px;
            text-align: center;
        }
        .qr-receipt-content {
            display: flex;
            gap: 20px;
            align-items: center;
            justify-content: center;
            flex-wrap: wrap;
        }
        .qr-code-wrapper {
            background-color: #ffffff;
            padding: 15px;
            border-radius: 8px;
            box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
            position: relative;
        }
        .qr-code-wrapper::before {
            content: '';
            position: absolute;
            top: -2px;
            left: -2px;
            right: -2px;
            bottom: -2px;
            background: linear-gradient(45deg, #FFD700, #FFA500, #FFD700);
            border-radius: 10px;
            z-index: -1;
            opacity: 0.7;
        }
        .qr-code-wrapper img {
            display: block;
            width: 150px !important; /* Lebih kecil untuk mobile */
            height: 150px !important; /* Lebih kecil untuk mobile */
        }
        .receipt-preview {
            background-color: #ffffff;
            padding: 15px;
            border-radius: 8px;
            max-width: 180px; /* Lebih kecil untuk mobile */
            font-family: 'Courier New', monospace;
            font-size: 0.65rem; /* Lebih kecil untuk mobile */
            color: #000;
            border: 1px solid #ddd;
        }
        .receipt-preview .receipt-preview-header {
            text-align: center;
            border-bottom: 1px dashed #000;
            padding-bottom: 5px;
            margin-bottom: 10px;
        }
        .receipt-preview .receipt-preview-header h4 {
            margin: 0;
            font-size: 0.8rem; /* Lebih kecil untuk mobile */
            font-weight: bold;
            color: #000; /* Pastikan warna h4 juga hitam */
        }
        .receipt-preview .receipt-preview-body {
            margin-bottom: 10px;
        }
        .receipt-preview .receipt-preview-item {
            display: flex;
            justify-content: space-between;
            margin-bottom: 3px;
            font-size: 0.6rem; /* Lebih kecil untuk mobile */
        }
        .receipt-preview .receipt-preview-footer {
            border-top: 1px dashed #000;
            padding-top: 5px;
            text-align: center;
            font-size: 0.55rem; /* Lebih kecil untuk mobile */
        }
        .qr-instruction {
            text-align: center;
            margin-top: 10px;
            font-size: 0.7rem; /* Lebih kecil untuk mobile */
            color: #666;
        }
        /* Gaya untuk cetak */
        @media print {
            body * {
                visibility: hidden;
            }
            #receiptModal, #receiptModal * {
                visibility: visible;
            }
            #receiptModal {
                position: absolute;
                left: 0;
                top: 0;
                width: 100%;
            }
            .modal-header, .modal-footer {
                display: none !important;
            }
            .modal-dialog {
                margin: 0;
                max-width: 100%;
            }
            .qr-receipt-container {
                box-shadow: none !important;
            }
            .receipt-preview {
                box-shadow: none !important;
            }
        }
        /* Gaya untuk badge kategori */
        .category-badge {
            position: absolute;
            top: 5px;
            left: 5px;
            background-color: rgba(255, 215, 0, 0.8);
            color: #1A1A1A;
            font-size: 0.6rem; /* Lebih kecil untuk mobile */
            font-weight: bold;
            padding: 2px 4px; /* Padding lebih kecil */
            border-radius: 4px;
            z-index: 2;
        }
        .hot-badge {
            background-color: rgba(255, 87, 34, 0.8);
            color: #FFFFFF;
        }
        .cold-badge {
            background-color: rgba(33, 150, 243, 0.8);
            color: #FFFFFF;
        }
        .coffee-bean-badge {
            background-color: rgba(139, 69, 19, 0.8);
            color: #FFFFFF;
        }
        /* Animasi pulse untuk QR */
        @keyframes pulse {
            0% {
                box-shadow: 0 0 0 0 rgba(255, 215, 0, 0.7);
            }
            70% {
                box-shadow: 0 0 0 10px rgba(255, 215, 0, 0);
            }
            100% {
                box-shadow: 0 0 0 0 rgba(255, 215, 0, 0);
            }
        }
        .qr-code-wrapper {
            animation: pulse 2s infinite;
        }

        /* --- TAMBAHAN STYLE UNTUK LOGIN MOBILE --- */
        #login-container {
            display: flex;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
            background-color: #1A1A1A;
            padding: 1rem;
        }
        .login-box {
            background-color: #282828;
            padding: 2rem;
            border-radius: 10px;
            box-shadow: 0 8px 20px rgba(0,0,0,0.5);
            width: 100%;
            max-width: 350px; /* Lebih kecil untuk mobile */
        }
        .login-box h3 {
            color: #FFD700;
            font-family: 'Lora', serif;
            font-weight: 700;
            text-align: center;
            margin-bottom: 1.5rem;
            font-size: 1.5rem; /* Lebih kecil untuk mobile */
        }
        
        /* Optimasi untuk sentuhan */
        .btn {
            min-height: 44px; /* Minimal tinggi untuk sentuhan */
            display: flex;
            align-items: center;
            justify-content: center;
        }
        
        /* Optimasi untuk scroll di mobile */
        ::-webkit-scrollbar {
            width: 6px;
        }
        ::-webkit-scrollbar-track {
            background: #1A1A1A;
        }
        ::-webkit-scrollbar-thumb {
            background: #555;
            border-radius: 3px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #777;
        }
        
        /* Optimasi untuk modal di mobile */
        .modal-dialog {
            margin: 0.5rem;
            max-width: calc(100% - 1rem);
        }
        
        /* Optimasi untuk input di mobile */
        input[type="number"] {
            -moz-appearance: textfield;
        }
        input[type="number"]::-webkit-outer-spin-button,
        input[type="number"]::-webkit-inner-spin-button {
            -webkit-appearance: none;
            margin: 0;
        }
        
        /* Optimasi untuk kategori di mobile - PERBAIKAN */
        .category-tabs {
            display: flex;
            overflow-x: auto;
            -webkit-overflow-scrolling: touch;
            margin-bottom: 1rem;
            padding-bottom: 0.5rem;
            flex-shrink: 0; /* Mencegah tab kategori menyusut */
            background-color: #222;
            border-radius: 8px;
            padding: 0.5rem;
        }
        .category-tab {
            flex: 0 0 auto;
            padding: 0.5rem 1rem;
            margin-right: 0.5rem;
            background-color: #444;
            border-radius: 20px;
            color: #E0E0E0;
            font-size: 0.9rem;
            white-space: nowrap;
            transition: all 0.3s ease;
        }
        .category-tab.active {
            background-color: #FFD700;
            color: #1A1A1A;
            font-weight: bold;
        }
        .category-tab:hover {
            background-color: #555;
        }
        .category-tab.active:hover {
            background-color: #E0B400;
        }
        
        /* Optimasi untuk tombol selesaikan di mobile */
        #complete-sale-btn {
            position: sticky;
            bottom: 0;
            z-index: 100;
            border-radius: 0;
            padding: 1rem;
            font-size: 1.1rem;
        }
        
        /* Perbaikan untuk tampilan menu yang lebih terpisah */
        .menu-section {
            margin-bottom: 1.5rem;
        }
        .menu-section-title {
            color: #FFD700;
            font-family: 'Lora', serif;
            font-weight: 700;
            font-size: 1.2rem;
            margin-bottom: 0.8rem;
            padding-bottom: 0.5rem;
            border-bottom: 2px solid #444;
            display: flex;
            align-items: center;
        }
        .menu-section-title i {
            margin-right: 0.5rem;
        }
        .menu-section-content {
            display: flex;
            flex-wrap: wrap;
            gap: 0.5rem;
        }
        
        /* MODIFIKASI UNTUK ANDROID - RESPONSIVITAS GAMBAR */
        /* Optimasi gambar untuk berbagai ukuran layar Android */
        .product-card .card-img-top {
            width: 100%;
            height: auto;
            object-fit: cover;
            transition: transform 0.3s ease;
        }
        
        /* Responsivitas untuk layar kecil (Android kecil) */
        @media (max-width: 360px) {
            .product-card .card-title {
                font-size: 0.8rem;
            }
            .product-card h6 {
                font-size: 0.9rem;
            }
            .category-tab {
                font-size: 0.8rem;
                padding: 0.4rem 0.8rem;
            }
            .btn-qty {
                width: 28px;
                height: 28px;
                font-size: 0.9rem;
            }
            .discount-btn {
                font-size: 0.65rem;
                padding: 4px;
            }
            .navbar-brand {
                font-size: 1.3rem;
            }
        }
        
        /* Responsivitas untuk layar sedang (Android menengah) */
        @media (min-width: 361px) and (max-width: 480px) {
            .product-card .card-title {
                font-size: 0.85rem;
            }
            .product-card h6 {
                font-size: 0.95rem;
            }
            .category-tab {
                font-size: 0.85rem;
            }
        }
        
        /* Responsivitas untuk layar besar (Android besar/tablet) */
        @media (min-width: 481px) {
            .menu-section-content {
                display: grid;
                grid-template-columns: repeat(3, 1fr);
                gap: 0.75rem;
            }
            .product-card .card-title {
                font-size: 0.95rem;
            }
            .product-card h6 {
                font-size: 1.05rem;
            }
            .category-tab {
                font-size: 0.95rem;
            }
        }
        
        /* Responsivitas untuk tablet dalam orientasi landscape */
        @media (min-width: 768px) and (orientation: landscape) {
            #pos-content {
                flex-direction: row;
            }
            #menu-panel {
                flex: 2;
                max-height: calc(100vh - 120px);
            }
            #checkout-panel {
                flex: 1;
                max-height: calc(100vh - 120px);
                overflow-y: auto;
            }
            .menu-section-content {
                grid-template-columns: repeat(4, 1fr);
            }
        }
        
        /* Optimasi untuk gambar produk di berbagai ukuran layar */
        .product-card {
            overflow: hidden;
        }
        
        .product-card .card-img-container {
            position: relative;
            width: 100%;
            padding-top: 100%; /* Rasio aspek 1:1 */
            overflow: hidden;
        }
        
        .product-card .card-img-top {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            object-fit: cover;
            transition: transform 0.3s ease;
        }
        
        .product-card:hover .card-img-top {
            transform: scale(1.05);
        }
        
        /* Optimasi untuk gambar di modal */
        #modal-product-image {
            max-height: 300px;
            object-fit: contain;
        }
        
        /* Optimasi untuk gambar QR di berbagai ukuran layar */
        @media (max-width: 480px) {
            .qr-receipt-content {
                flex-direction: column;
            }
            .qr-code-wrapper img {
                width: 120px !important;
                height: 120px !important;
            }
            .receipt-preview {
                max-width: 100%;
                margin-top: 15px;
            }
        }
        
        /* Optimasi untuk gambar struk di layar kecil */
        @media (max-width: 360px) {
            #receipt-content {
                padding: 10px;
                font-size: 0.8rem;
            }
            #receipt-content .receipt-header h3 {
                font-size: 1.2rem;
            }
            #receipt-content .receipt-item {
                font-size: 0.8rem;
            }
        }
        
        /* Optimasi untuk gambar di login */
        @media (max-width: 480px) {
            .login-box {
                padding: 1.5rem;
            }
            .login-box h3 {
                font-size: 1.3rem;
            }
        }
        
        /* Optimasi untuk gambar di kategori */
        .category-badge {
            font-size: 0.55rem;
            padding: 2px 3px;
        }
        
        /* Optimasi untuk gambar di tombol detail */
        .btn-detail {
            width: 28px;
            height: 28px;
            font-size: 0.9rem;
        }
        
        /* Optimasi untuk gambar di navbar */
        .navbar {
            padding: 0.4rem 0.8rem;
        }
        
        /* Optimasi untuk gambar di panel checkout */
        #cart-items-container-pos {
            max-height: 120px;
        }
        
        /* Optimasi untuk gambar di panel pembayaran */
        .payment-summary {
            padding: 0.6rem;
        }
        
        /* Optimasi untuk gambar di tombol selesaikan */
        #complete-sale-btn {
            padding: 0.8rem;
            font-size: 1rem;
        }
        
        /* Optimasi untuk gambar di input pembayaran */
        #bayar-input {
            padding: 0.5rem;
            font-size: 0.9rem;
        }
        
        /* Optimasi untuk gambar di tombol diskon */
        .discount-buttons {
            gap: 4px;
        }
        
        /* Optimasi untuk gambar di tombol kuantitas */
        .btn-qty {
            width: 32px;
            height: 32px;
        }
        
        /* Optimasi untuk gambar di keranjang */
        .cart-item-pos {
            padding: 6px 0;
        }
        
        /* Optimasi untuk gambar di total pembayaran */
        #cart-total-pos {
            font-size: 1.1rem;
        }
        
        /* Optimasi untuk gambar di kembalian */
        #kembalian-text {
            font-size: 1.1rem;
        }
        
        /* Optimasi untuk gambar di subtotal */
        #cart-subtotal-pos {
            font-size: 0.9rem;
        }
        
        /* Optimasi untuk gambar di diskon */
        #discount-amount {
            font-size: 0.9rem;
        }
        
        /* Optimasi untuk gambar di pesan keranjang kosong */
        #cart-empty-message {
            font-size: 0.8rem;
        }
        
        /* Optimasi untuk gambar di judul panel checkout */
        #checkout-panel h4 {
            font-size: 1.1rem;
        }
        
        /* Optimasi untuk gambar di judul panel menu */
        #menu-panel h2 {
            font-size: 1.2rem;
        }
        
        /* Optimasi untuk gambar di input pencarian */
        #menu-search-input {
            padding: 0.5rem;
            font-size: 0.9rem;
        }
        
        /* Optimasi untuk gambar di judul kategori */
        .menu-section-title {
            font-size: 1.1rem;
        }
        
        /* Optimasi untuk gambar di judul produk */
        .product-card .card-title {
            font-size: 0.85rem;
        }
        
        /* Optimasi untuk gambar di harga produk */
        .product-card h6 {
            font-size: 0.95rem;
        }
        
        /* Optimasi untuk gambar di deskripsi produk */
        .product-card .card-text {
            font-size: 0.75rem;
        }
        
        /* Optimasi untuk gambar di tombol tambah ke keranjang */
        .btn-add-to-cart {
            padding: 0.4rem 0.8rem;
            font-size: 0.8rem;
        }
        
        /* Optimasi untuk gambar di modal detail produk */
        .modal-body {
            padding: 1rem;
        }
        
        /* Optimasi untuk gambar di judul modal */
        .modal-title {
            font-size: 1.2rem;
        }
        
        /* Optimasi untuk gambar di tombol tutup modal */
        .btn-close {
            font-size: 1.2rem;
        }
        
        /* Optimasi untuk gambar di tombol tambah ke keranjang di modal */
        #modal-add-to-cart-btn {
            padding: 0.6rem 1rem;
            font-size: 0.9rem;
        }
        
        /* Optimasi untuk gambar di deskripsi produk di modal */
        #modal-product-description {
            font-size: 0.9rem;
        }
        
        /* Optimasi untuk gambar di harga produk di modal */
        #modal-product-price {
            font-size: 1.3rem;
        }
        
        /* Optimasi untuk gambar di nama produk di modal */
        #modal-product-name {
            font-size: 1.4rem;
        }
        
        /* Optimasi untuk gambar di modal struk */
        .modal-dialog {
            max-width: 95%;
        }
        
        /* Optimasi untuk gambar di judul modal struk */
        #receiptModalLabel {
            font-size: 1.2rem;
        }
        
        /* Optimasi untuk gambar di tombol cetak struk */
        .btn-print {
            padding: 0.6rem 1rem;
            font-size: 0.9rem;
        }
        
        /* Optimasi untuk gambar di tombol tutup modal struk */
        .btn-close-receipt {
            padding: 0.6rem 1rem;
            font-size: 0.9rem;
        }
        
        /* Optimasi untuk gambar di judul struk */
        .receipt-header h3 {
            font-size: 1.4rem;
        }
        
        /* Optimasi untuk gambar di alamat struk */
        .receipt-header p {
            font-size: 0.8rem;
        }
        
        /* Optimasi untuk gambar di tanggal struk */
        #receipt-date {
            font-size: 0.8rem;
        }
        
        /* Optimasi untuk gambar di ID transaksi struk */
        #receipt-trans-id {
            font-size: 0.8rem;
        }
        
        /* Optimasi untuk gambar di item struk */
        .receipt-item {
            font-size: 0.85rem;
        }
        
        /* Optimasi untuk gambar di subtotal struk */
        #receipt-subtotal {
            font-size: 0.9rem;
        }
        
        /* Optimasi untuk gambar di diskon struk */
        #receipt-discount-percent {
            font-size: 0.9rem;
        }
        
        /* Optimasi untuk gambar di jumlah diskon struk */
        #receipt-discount-amount {
            font-size: 0.9rem;
        }
        
        /* Optimasi untuk gambar di total struk */
        #receipt-total {
            font-size: 1rem;
        }
        
        /* Optimasi untuk gambar di tunai struk */
        #receipt-cash {
            font-size: 0.9rem;
        }
        
        /* Optimasi untuk gambar di kembalian struk */
        #receipt-change {
            font-size: 0.9rem;
        }
        
        /* Optimasi untuk gambar di footer struk */
        .receipt-footer p {
            font-size: 0.8rem;
        }
        
        /* Optimasi untuk gambar di judul QR */
        .qr-receipt-title {
            font-size: 0.9rem;
        }
        
        /* Optimasi untuk gambar di instruksi QR */
        .qr-instruction {
            font-size: 0.65rem;
        }
        
        /* Optimasi untuk gambar di preview struk */
        .receipt-preview {
            font-size: 0.6rem;
        }
        
        /* Optimasi untuk gambar di header preview struk */
        .receipt-preview-header h4 {
            font-size: 0.75rem;
        }
        
        /* Optimasi untuk gambar di tanggal preview struk */
        #preview-date {
            font-size: 0.6rem;
        }
        
        /* Optimasi untuk gambar di ID transaksi preview struk */
        #preview-trans-id {
            font-size: 0.6rem;
        }
        
        /* Optimasi untuk gambar di item preview struk */
        .receipt-preview-item {
            font-size: 0.55rem;
        }
        
        /* Optimasi untuk gambar di footer preview struk */
        .receipt-preview-footer p {
            font-size: 0.5rem;
        }
        
        /* Optimasi untuk gambar di wrapper QR */
        .qr-code-wrapper {
            padding: 12px;
        }
        
        /* Optimasi untuk gambar di gambar QR */
        .qr-code-wrapper img {
            width: 130px !important;
            height: 130px !important;
        }
        
        /* Optimasi untuk gambar di container QR */
        .qr-receipt-container {
            padding: 12px;
        }
        
        /* Optimasi untuk gambar di content QR */
        .qr-receipt-content {
            gap: 15px;
        }
        
        /* Optimasi untuk gambar di judul QR */
        .qr-receipt-title {
            margin-bottom: 12px;
        }
        
        /* Optimasi untuk gambar di instruksi QR */
        .qr-instruction {
            margin-top: 8px;
        }
        
        /* Optimasi untuk gambar di tombol detail produk */
        .btn-detail {
            width: 26px;
            height: 26px;
            font-size: 0.85rem;
        }
        
        /* Optimasi untuk gambar di badge kategori */
        .category-badge {
            font-size: 0.5rem;
            padding: 1px 3px;
        }
        
        /* Optimasi untuk gambar di card produk */
        .product-card {
            margin-bottom: 0.4rem;
        }
        
        /* Optimasi untuk gambar di body card produk */
        .product-card .card-body {
            padding: 0.4rem;
            min-height: 70px;
        }
        
        /* Optimasi untuk gambar di judul card produk */
        .product-card .card-title {
            font-size: 0.8rem;
            min-height: 2.2em;
        }
        
        /* Optimasi untuk gambar di harga card produk */
        .product-card h6 {
            font-size: 0.9rem;
        }
        
        /* Optimasi untuk gambar di gambar card produk */
        .product-card .card-img-top {
            padding: 3px;
        }
        
        /* Optimasi untuk gambar di container gambar card produk */
        .product-card .card-img-container {
            padding-top: 100%;
        }
        
        /* Optimasi untuk gambar di hover card produk */
        .product-card:hover .card-img-top {
            transform: scale(1.03);
        }
        
        /* Optimasi untuk gambar di shadow card produk */
        .product-card {
            box-shadow: 0 3px 6px rgba(0, 0, 0, 0.3);
        }
        
        /* Optimasi untuk gambar di hover shadow card produk */
        .product-card:hover {
            box-shadow: 0 6px 12px rgba(255, 215, 0, 0.15);
        }
        
        /* Optimasi untuk gambar di border card produk */
        .product-card {
            border-radius: 6px;
        }
        
        /* Optimasi untuk gambar di hover border card produk */
        .product-card:hover {
            border: 1px solid #FFD700;
        }
        
        /* Optimasi untuk gambar di transition card produk */
        .product-card {
            transition: transform 0.2s, box-shadow 0.2s;
        }
        
        /* Optimasi untuk gambar di transform card produk */
        .product-card:hover {
            transform: translateY(-2px);
        }
        
        /* Optimasi untuk gambar di judul section menu */
        .menu-section-title {
            font-size: 1.1rem;
            margin-bottom: 0.6rem;
            padding-bottom: 0.4rem;
        }
        
        /* Optimasi untuk gambar di icon section menu */
        .menu-section-title i {
            margin-right: 0.4rem;
        }
        
        /* Optimasi untuk gambar di section menu */
        .menu-section {
            margin-bottom: 1.2rem;
        }
        
        /* Optimasi untuk gambar di content section menu */
        .menu-section-content {
            gap: 0.4rem;
        }
        
        /* Optimasi untuk gambar di tab kategori */
        .category-tabs {
            padding: 0.4rem;
            margin-bottom: 0.8rem;
        }
        
        /* Optimasi untuk gambar di tab kategori individual */
        .category-tab {
            padding: 0.4rem 0.8rem;
            margin-right: 0.4rem;
            font-size: 0.85rem;
        }
        
        /* Optimasi untuk gambar di tab kategori aktif */
        .category-tab.active {
            font-weight: bold;
        }
        
        /* Optimasi untuk gambar di hover tab kategori */
        .category-tab:hover {
            background-color: #555;
        }
        
        /* Optimasi untuk gambar di hover tab kategori aktif */
        .category-tab.active:hover {
            background-color: #E0B400;
        }
        
        /* Optimasi untuk gambar di input pencarian menu */
        #menu-search-input {
            padding: 0.4rem;
            font-size: 0.85rem;
            margin-bottom: 0.8rem;
        }
        
        /* Optimasi untuk gambar di panel menu */
        #menu-panel {
            padding: 0.4rem;
        }
        
        /* Optimasi untuk gambar di panel checkout */
        #checkout-panel {
            padding: 0.4rem;
        }
        
        /* Optimasi untuk gambar di container item keranjang */
        #cart-items-container-pos {
            padding: 0.4rem;
            margin-bottom: 0.8rem;
            max-height: 120px;
        }
        
        /* Optimasi untuk gambar di item keranjang */
        .cart-item-pos {
            padding: 6px 0;
        }
        
        /* Optimasi untuk gambar di judul item keranjang */
        .cart-item-pos h6 {
            font-size: 0.85rem;
        }
        
        /* Optimasi untuk gambar di harga dan kuantitas item keranjang */
        .cart-item-pos .price-qty {
            font-size: 0.75rem;
        }
        
        /* Optimasi untuk gambar di tombol kuantitas */
        .btn-qty {
            width: 28px;
            height: 28px;
            font-size: 0.9rem;
        }
        
        /* Optimasi untuk gambar di pesan keranjang kosong */
        #cart-empty-message {
            font-size: 0.75rem;
        }
        
        /* Optimasi untuk gambar di judul panel checkout */
        #checkout-panel h4 {
            font-size: 1rem;
            margin-bottom: 0.6rem;
        }
        
        /* Optimasi untuk gambar di ringkasan pembayaran */
        .payment-summary {
            padding: 0.6rem;
        }
        
        /* Optimasi untuk gambar di subtotal */
        #cart-subtotal-pos {
            font-size: 0.85rem;
        }
        
        /* Optimasi untuk gambar di label diskon */
        .form-label {
            font-size: 0.8rem;
        }
        
        /* Optimasi untuk gambar di tombol diskon */
        .discount-buttons {
            gap: 4px;
            margin-bottom: 8px;
        }
        
        /* Optimasi untuk gambar di tombol diskon individual */
        .discount-btn {
            min-width: 45px;
            padding: 4px;
            font-size: 0.65rem;
        }
        
        /* Optimasi untuk gambar di info diskon */
        .discount-info {
            margin-bottom: 8px;
            font-size: 0.75rem;
        }
        
        /* Optimasi untuk gambar di jumlah diskon */
        #discount-amount {
            font-size: 0.85rem;
        }
        
        /* Optimasi untuk gambar di total pembayaran */
        #cart-total-pos {
            font-size: 1rem;
        }
        
        /* Optimasi untuk gambar di label pembayaran */
        .form-label {
            font-size: 0.8rem;
        }
        
        /* Optimasi untuk gambar di input pembayaran */
        #bayar-input {
            padding: 0.4rem;
            font-size: 0.85rem;
        }
        
        /* Optimasi untuk gambar di kembalian */
        #kembalian-text {
            font-size: 1rem;
        }
        
        /* Optimasi untuk gambar di tombol selesaikan penjualan */
        #complete-sale-btn {
            padding: 0.8rem;
            font-size: 1rem;
        }
        
        /* Optimasi untuk gambar di navbar */
        .navbar {
            padding: 0.4rem 0.8rem;
        }
        
        /* Optimasi untuk gambar di brand navbar */
        .navbar-brand {
            font-size: 1.3rem;
        }
        
        /* Optimasi untuk gambar di teks navbar */
        .navbar-text {
            font-size: 0.8rem;
        }
        
        /* Optimasi untuk gambar di tombol logout */
        #logout-btn {
            padding: 0.3rem 0.6rem;
            font-size: 0.8rem;
        }
        
        /* Optimasi untuk gambar di container login */
        #login-container {
            padding: 0.8rem;
        }
        
        /* Optimasi untuk gambar di box login */
        .login-box {
            padding: 1.5rem;
            max-width: 320px;
        }
        
        /* Optimasi untuk gambar di judul login */
        .login-box h3 {
            font-size: 1.3rem;
            margin-bottom: 1.2rem;
        }
        
        /* Optimasi untuk gambar di label form login */
        .form-label {
            font-size: 0.8rem;
        }
        
        /* Optimasi untuk gambar di input form login */
        .form-control {
            padding: 0.4rem;
            font-size: 0.85rem;
        }
        
        /* Optimasi untuk gambar di tombol login */
        .btn {
            padding: 0.6rem;
            font-size: 0.9rem;
        }
        
        /* Optimasi untuk gambar di pesan error login */
        .alert {
            padding: 0.5rem;
            font-size: 0.8rem;
        }
        
        /* Optimasi untuk gambar di modal */
        .modal-content {
            border-radius: 6px;
        }
        
        /* Optimasi untuk gambar di header modal */
        .modal-header {
            padding: 0.8rem;
        }
        
        /* Optimasi untuk gambar di body modal */
        .modal-body {
            padding: 0.8rem;
        }
        
        /* Optimasi untuk gambar di footer modal */
        .modal-footer {
            padding: 0.8rem;
        }
        
        /* Optimasi untuk gambar di judul modal */
        .modal-title {
            font-size: 1.1rem;
        }
        
        /* Optimasi untuk gambar di tombol tutup modal */
        .btn-close {
            font-size: 1.1rem;
        }
        
        /* Optimasi untuk gambar di gambar produk modal */
        #modal-product-image {
            max-height: 250px;
        }
        
        /* Optimasi untuk gambar di nama produk modal */
        #modal-product-name {
            font-size: 1.3rem;
        }
        
        /* Optimasi untuk gambar di deskripsi produk modal */
        #modal-product-description {
            font-size: 0.85rem;
        }
        
        /* Optimasi untuk gambar di harga produk modal */
        #modal-product-price {
            font-size: 1.2rem;
        }
        
        /* Optimasi untuk gambar di tombol tambah ke keranjang modal */
        #modal-add-to-cart-btn {
            padding: 0.5rem 0.8rem;
            font-size: 0.85rem;
        }
        
        /* Optimasi untuk gambar di modal struk */
        .modal-dialog {
            max-width: 95%;
        }
        
        /* Optimasi untuk gambar di content struk */
        #receipt-content {
            padding: 12px;
            max-width: 350px;
        }
        
        /* Optimasi untuk gambar di header struk */
        .receipt-header {
            padding-bottom: 8px;
            margin-bottom: 12px;
        }
        
        /* Optimasi untuk gambar di judul struk */
        .receipt-header h3 {
            font-size: 1.3rem;
        }
        
        /* Optimasi untuk gambar di alamat struk */
        .receipt-header p {
            font-size: 0.75rem;
        }
        
        /* Optimasi untuk gambar di body struk */
        .receipt-body {
            margin-bottom: 12px;
        }
        
        /* Optimasi untuk gambar di item struk */
        .receipt-item {
            font-size: 0.8rem;
            margin-bottom: 4px;
        }
        
        /* Optimasi untuk gambar di footer struk */
        .receipt-footer {
            padding-top: 8px;
        }
        
        /* Optimasi untuk gambar di teks footer struk */
        .receipt-footer p {
            font-size: 0.75rem;
        }
        
        /* Optimasi untuk gambar di container QR */
        .qr-receipt-container {
            padding: 12px;
            margin: 15px 0;
        }
        
        /* Optimasi untuk gambar di judul QR */
        .qr-receipt-title {
            font-size: 0.9rem;
            margin-bottom: 12px;
        }
        
        /* Optimasi untuk gambar di content QR */
        .qr-receipt-content {
            gap: 15px;
        }
        
        /* Optimasi untuk gambar di wrapper QR */
        .qr-code-wrapper {
            padding: 12px;
        }
        
        /* Optimasi untuk gambar di gambar QR */
        .qr-code-wrapper img {
            width: 120px !important;
            height: 120px !important;
        }
        
        /* Optimasi untuk gambar di preview struk */
        .receipt-preview {
            padding: 12px;
            max-width: 160px;
            font-size: 0.6rem;
        }
        
        /* Optimasi untuk gambar di header preview struk */
        .receipt-preview-header {
            padding-bottom: 4px;
            margin-bottom: 8px;
        }
        
        /* Optimasi untuk gambar di judul preview struk */
        .receipt-preview-header h4 {
            font-size: 0.7rem;
        }
        
        /* Optimasi untuk gambar di body preview struk */
        .receipt-preview-body {
            margin-bottom: 8px;
        }
        
        /* Optimasi untuk gambar di item preview struk */
        .receipt-preview-item {
            font-size: 0.55rem;
            margin-bottom: 2px;
        }
        
        /* Optimasi untuk gambar di footer preview struk */
        .receipt-preview-footer {
            padding-top: 4px;
            font-size: 0.5rem;
        }
        
        /* Optimasi untuk gambar di instruksi QR */
        .qr-instruction {
            margin-top: 8px;
            font-size: 0.65rem;
        }
        
        /* Optimasi untuk gambar di tombol cetak */
        .btn-print {
            padding: 0.5rem 0.8rem;
            font-size: 0.85rem;
        }
        
        /* Optimasi untuk gambar di tombol tutup struk */
        .btn-close-receipt {
            padding: 0.5rem 0.8rem;
            font-size: 0.85rem;
        }
        
        /* Tambahkan animasi untuk QR */
        .qr-animation {
            position: relative;
            overflow: hidden;
        }
        
        .qr-animation::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255, 215, 0, 0.2), transparent);
            animation: qrShine 3s infinite;
        }
        
        @keyframes qrShine {
            0% { left: -100%; }
            20% { left: 100%; }
            100% { left: 100%; }
        }
        
        /* Tambahkan efek untuk tombol bagikan */
        .share-btn {
            background: linear-gradient(135deg, #4CAF50, #45a049);
            border: none;
            color: white;
            padding: 8px 16px;
            text-align: center;
            text-decoration: none;
            display: inline-block;
            font-size: 14px;
            margin: 4px 2px;
            cursor: pointer;
            border-radius: 4px;
            transition: all 0.3s ease;
        }
        
        .share-btn:hover {
            background: linear-gradient(135deg, #45a049, #4CAF50);
            transform: translateY(-2px);
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
        }
        
        /* Tambahkan tombol unduh struk */
        .download-btn {
            background: linear-gradient(135deg, #2196F3, #1976D2);
            border: none;
            color: white;
            padding: 8px 16px;
            text-align: center;
            text-decoration: none;
            display: inline-block;
            font-size: 14px;
            margin: 4px 2px;
            cursor: pointer;
            border-radius: 4px;
            transition: all 0.3s ease;
        }
        
        .download-btn:hover {
            background: linear-gradient(135deg, #1976D2, #2196F3);
            transform: translateY(-2px);
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
        }
        
        /* Tambahkan style untuk notifikasi */
        .notification {
            position: fixed;
            top: 20px;
            right: 20px;
            padding: 15px;
            background-color: #4CAF50;
            color: white;
            border-radius: 4px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
            z-index: 1000;
            transform: translateX(120%);
            transition: transform 0.3s ease;
        }
        
        .notification.show {
            transform: translateX(0);
        }
        
        /* Tambahkan style untuk tombol close notifikasi */
        .notification-close {
            position: absolute;
            top: 5px;
            right: 10px;
            background: none;
            border: none;
            color: white;
            font-size: 16px;
            cursor: pointer;
        }
        
        /* Style untuk loading spinner */
        .loading-spinner {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.5);
            z-index: 9999;
            justify-content: center;
            align-items: center;
        }
        
        .spinner {
            width: 50px;
            height: 50px;
            border: 5px solid #f3f3f3;
            border-top: 5px solid #FFD700;
            border-radius: 50%;
            animation: spin 1s linear infinite;
        }
        
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
        
        /* Style untuk tombol aksi struk */
        .receipt-actions {
            display: flex;
            justify-content: center;
            gap: 10px;
            margin-top: 15px;
        }
        
        .receipt-actions button {
            min-width: 120px;
        }
    </style>
</head>
<body>

    <div id="login-container">
        <div class="login-box">
            <h3>LOGIN KASIR</h3>
            <div id="login-error" class="alert alert-danger d-none" role="alert">
                Email atau Password salah!
            </div>
            <form id="login-form">
                <div class="mb-3">
                    <label for="login-email" class="form-label text-white-50">Email</label>
                    <input type="email" class="form-control form-control-dark" id="login-email" required>
                </div>
                <div class="mb-3">
                    <label for="login-password" class="form-label text-white-50">Password</label>
                    <input type="password" class="form-control form-control-dark" id="login-password" required>
                </div>
                <button type="submit" class="btn btn-primary w-100 py-2 fw-bold mt-3">
                    <i class="bi bi-box-arrow-in-right"></i> Login
                </button>
            </form>
        </div>
    </div>
    <div id="pos-container" class="d-none">

        <nav class="navbar navbar-expand-lg navbar-dark bg-dark shadow-sm">
            <div class="container-fluid px-2">
                <a class="navbar-brand" href="#">💰 KASIR</a>
                <span class="navbar-text text-white-50 ms-auto me-2" id="user-email-display">
                    </span>
                <button class="btn btn-sm btn-outline-warning" id="logout-btn">
                    <i class="bi bi-box-arrow-right"></i> Logout
                </button>
            </div>
        </nav>

        <div id="pos-content" class="container-fluid">
            
            <!-- Tab Kategori untuk Mobile -->
            <div class="category-tabs">
                <div class="category-tab active" data-category="all">Semua</div>
                <div class="category-tab" data-category="hot">☕ Panas</div>
                <div class="category-tab" data-category="cold">🧊 Dingin</div>
                <div class="category-tab" data-category="food">🍽️ Makanan</div>
                <div class="category-tab" data-category="beans">🫘 Biji Kopi</div>
            </div>
            
            <div id="menu-panel">
                <input type="text" id="menu-search-input" class="form-control form-control-dark mb-3" placeholder="Cari Menu...">

                <div id="product-list-container">
                    <!-- Menu akan dimuat di sini dengan JavaScript -->
                </div>
            </div>

            <div id="checkout-panel">
                <h4 class="text-center text-white mb-2">Rincian Pesanan</h4>

                <div id="cart-items-container-pos" class="bg-dark rounded p-2 mb-2">
                    <p class="text-center text-muted small mt-3" id="cart-empty-message">Ketuk menu di atas untuk memulai pesanan.</p>
                </div>

                <div class="payment-summary mt-auto">
                    <div class="d-flex justify-content-between text-white mb-2">
                        <span>Subtotal:</span>
                        <span id="cart-subtotal-pos" class="text-white">Rp 0</span>
                    </div>
                    
                    <div class="mb-3">
                        <label class="form-label small text-white">Diskon:</label>
                        <div class="discount-buttons">
                            <button class="btn btn-outline-warning discount-btn" onclick="applyDiscount(0)">Tidak Ada</button>
                            <button class="btn btn-outline-warning discount-btn" onclick="applyDiscount(10)">10%</button>
                            <button class="btn btn-outline-warning discount-btn" onclick="applyDiscount(20)">20%</button>
                            <button class="btn btn-outline-warning discount-btn" onclick="applyDiscount(30)">30%</button>
                            <button class="btn btn-outline-warning discount-btn" onclick="applyDiscount(40)">40%</button>
                            <button class="btn btn-outline-warning discount-btn" onclick="applyDiscount(50)">50%</button>
                        </div>
                        <div class="discount-info">
                            <span>Potongan Diskon:</span>
                            <span id="discount-amount" class="discount-amount">Rp 0</span>
                        </div>
                    </div>
                    
                    <hr class="border-secondary">

                    <div class="d-flex justify-content-between text-white fw-bold fs-5 mb-2">
                        <span>TOTAL BAYAR:</span>
                        <span id="cart-total-pos" class="text-warning">Rp 0</span>
                    </div>
                    
                    <hr class="border-secondary">

                    <div class="mb-3">
                        <label for="bayar-input" class="form-label small text-white">Jumlah Dibayarkan (Cash):</label>
                        <input type="number" id="bayar-input" class="form-control form-control-dark" placeholder="Masukkan jumlah uang" min="0" oninput="calculateChange()">
                    </div>

                    <div class="d-flex justify-content-between text-white fw-bold fs-5 mb-3">
                        <span>Kembalian:</span>
                        <span id="kembalian-text" class="text-success">Rp 0</span>
                    </div>
                    
                    <button type="button" class="btn btn-primary w-100 py-3 fw-bold" id="complete-sale-btn" onclick="completeSale()" disabled>
                        <i class="bi bi-cash-stack"></i> Selesaikan Penjualan
                    </button>
                    
                </div>
            </div>
        </div>

    </div>
    <div class="modal fade" id="productDetailModal" tabindex="-1" aria-labelledby="productDetailModalLabel" aria-hidden="true">
        <div class="modal-dialog modal-lg">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title" id="productDetailModalLabel">Detail Produk</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body text-center">
                    <div class="card-img-container">
                        <img id="modal-product-image" src="" class="card-img-top" alt="Product Image">
                    </div>
                    <h4 id="modal-product-name" class="text-warning"></h4>
                    <p id="modal-product-description" class="text-white-50"></p>
                    <h3 id="modal-product-price" class="text-white fw-bold"></h3>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Tutup</button>
                    <button type="button" id="modal-add-to-cart-btn" class="btn btn-primary">
                        <i class="bi bi-cart-plus"></i> Tambah ke Keranjang
                    </button>
                </div>
            </div>
        </div>
    </div>

    <div class="modal fade" id="receiptModal" tabindex="-1" aria-labelledby="receiptModalLabel" aria-hidden="true">
        <div class="modal-dialog">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title" id="receiptModalLabel">Struk Pembayaran</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">
                    <div id="receipt-content">
                        <div class="receipt-header">
                            <h3>WAROENG MASHER</h3>
                            <p>Jl. pancing mabar,pasar 4</p>
                            <p>Telp: (Nomor Telepon)</p>
                            <p id="receipt-date"></p>
                            <p id="receipt-trans-id"></p>
                        </div>
                        <div class="receipt-body">
                            <div id="receipt-items"></div>
                            <hr>
                            <div class="receipt-item">
                                <span>Subtotal:</span>
                                <span id="receipt-subtotal"></span>
                            </div>
                            <div class="receipt-item">
                                <span>Diskon (<span id="receipt-discount-percent"></span>%):</span>
                                <span id="receipt-discount-amount"></span>
                            </div>
                            <div class="receipt-item fw-bold">
                                <span>Total:</span>
                                <span id="receipt-total"></span>
                            </div>
                            <div class="receipt-item">
                                <span>Tunai:</span>
                                <span id="receipt-cash"></span>
                            </div>
                            <div class="receipt-item">
                                <span>Kembalian:</span>
                                <span id="receipt-change"></span>
                            </div>
                        </div>
                        
                        <div class="qr-receipt-container">
                            <div class="qr-receipt-title">📱 SCAN QR UNTUK STRUK DIGITAL</div>
                            <div class="qr-receipt-content">
                                <div class="qr-code-wrapper qr-animation">
                                    <div id="receipt-qr"></div>
                                </div>
                                <div class="receipt-preview" id="receipt-preview">
                                    <div class="receipt-preview-header">
                                        <h4>WAROENG MASHER</h4>
                                        <p id="preview-date"></p>
                                        <p id="preview-trans-id"></p>
                                    </div>
                                    <div class="receipt-preview-body" id="preview-items">
                                    </div>
                                    <div class="receipt-preview-footer">
                                        <p>Terima Kasih</p>
                                        <p>Selamat Berbelanja Kembali</p>
                                    </div>
                                </div>
                            </div>
                            <div class="qr-instruction">
                                <p><i class="bi bi-qr-code-scan"></i> Scan QR untuk membuka struk di browser</p>
                            </div>
                            <div class="receipt-actions">
                                <button class="btn share-btn" onclick="shareReceipt()">
                                    <i class="bi bi-share"></i> Bagikan
                                </button>
                                <button class="btn download-btn" onclick="downloadReceipt()">
                                    <i class="bi bi-download"></i> Unduh
                                </button>
                            </div>
                        </div>

                        <div class="receipt-footer">
                            <p>Terima Kasih</p>
                            <p>Selamat Berbelanja Kembali</p>
                        </div>
                    </div>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Tutup</button>
                    <button type="button" class="btn btn-primary" onclick="printReceipt()">
                        <i class="bi bi-printer"></i> Cetak
                    </button>
                </div>
            </div>
        </div>
    </div>
    
    <!-- Loading Spinner -->
    <div class="loading-spinner" id="loading-spinner">
        <div class="spinner"></div>
    </div>
    
    <!-- Notifikasi -->
    <div id="notification" class="notification">
        <button class="notification-close" onclick="hideNotification()">&times;</button>
        <span id="notification-message"></span>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/js/bootstrap.bundle.min.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-auth-compat.js"></script> 
    <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-firestore-compat.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/qrcodejs@1.0.0/qrcode.min.js"></script>
    <script src="https://html2canvas.hertzen.com/dist/html2canvas.min.js"></script>
    
    <script>
        // --- DATA PRODUK (PINDAHKAN KE ATAS) ---
        // Data produk kita letakkan di sini agar tidak terpengaruh error Firebase
        const products = [
            { id: 'INDONESIANO HOT', name: 'INDONESIANO HOT', price: 10000, image: 'sasya.jpg', description: 'Kopi hitam klasik Indonesia disajikan hangat dengan rasa otentik.', category: 'minuman', temperature: 'hot' },
            { id: 'LONGBLACK HOT', name: 'LONGBLACK HOT', price: 10000, image: 'sasya.jpg', description: 'Espresso ganda yang diekstraksi ke dalam air panas, kuat dan menyegarkan.', category: 'minuman', temperature: 'hot' },
            { id: 'FILTER COFFE HOT', name: 'FILTER KOPI HOT', price: 10000, image: '3.jpg', description: 'Kopi yang diseduh dengan metode filter, menonjolkan karakter biji kopi.', category: 'minuman', temperature: 'hot' },
            { id: 'INDONESIANO ICED', name: 'INDONESIANO ICED', price: 12000, image: 'americano.jpg', description: 'Kopi Indonesia disajikan dingin, segar dan penuh semangat.', category: 'minuman', temperature: 'cold' },
            { id: 'LONG BLACK ICED', name: 'LONG BLACK ICED', price: 12000, image: 'americano.jpg', description: 'Long Black dingin, pendamping sempurna di hari yang panas.', category: 'minuman', temperature: 'cold' },
            { id: 'SANGER ICED', name: 'SANGER ICED', price: 15000, image: 'b.jpg', description: 'Sanger dingin yang creamy, minuman kopi favorit untuk penyuka rasa manis.', category: 'minuman', temperature: 'cold' },
            { id: 'FILTER ICED', name: 'FILTER ICED', price: 12000, image: '4.jpg', description: 'Filter Kopi disajikan dingin, lebih lembut dan mudah dinikmati.', category: 'minuman', temperature: 'cold' },
            { id: 'ESPRESSO', name: 'ESPRESSO', price: 8000, image: '6.jpg', description: 'kopi yang memiliki rasa yang sangat stroong.', category: 'minuman', temperature: 'hot' },
            { id: 'TEH HIJAU HOT&COLD', name: 'TEH HIJAU HOT&COLD', price: 5000, image: '8.jpg', description: 'Teh hijau kaya antioksidan, bisa disajikan panas atau dingin.', category: 'minuman', temperature: 'both' },
            { id: 'TEH MANIS HOT&COLD', name: 'TEH MANIS HOT&COLD', price: 5000, image: '7.jpg', description: 'Teh diseduh manis dan disajikan dingin, klasik dan melegakan dahaga.', category: 'minuman', temperature: 'both' },
            { id: 'WEDANG JAHE HOT', name: 'WEDANG JAHE HOT', price: 10000, image: '9.jpg', description: 'Minuman jahe hangat yang menenangkan, cocok untuk cuaca dingin.', category: 'minuman', temperature: 'hot' },
            { id: 'MILO COLD&HOT', name: 'MILO COLD&HOT', price: 10000, image: '10.jpg', description: 'Minuman cokelat malt favorit, bisa disajikan dingin atau hangat.', category: 'minuman', temperature: 'both' },
            { id: 'KENTANG GORENG', name: 'KENTANG GORENG', price: 10000, image: '11.jpg', description: 'Kentang goreng renyah disajikan dengan saus pilihan.', category: 'makanan', temperature: 'none' },
            { id: 'ROTI BAKAR', name: 'ROTI BAKAR', price: 7000, image: '12.jpg', description: 'Roti bakar dengan berbagai topping manis.', category: 'makanan', temperature: 'none' },
            { id: 'AYAM PENYET', name: 'AYAM PENYET', price: 10000, image: '13.jpg', description: 'Ayam goreng dengan sambal penyet pedas, disajikan dengan nasi hangat.', category: 'makanan', temperature: 'none' },
            { id: 'BAKARAN', name: 'ANEKA BAKARAN', price: 1000, image: '17.jpg', description: 'Pilihan sate-satean bakar (per tusuk) dengan bumbu spesial.', category: 'makanan', temperature: 'none' },
            { id: 'AYAM PENYET SAMBEL IJO', name: 'AYAM PENYET SAMBEL IJO', price: 10000, image: '14.jpg', description: 'Ayam goreng dengan sambal hijau pedas yang khas.', category: 'makanan', temperature: 'none' },
            // Tambahkan 5 menu biji kopi
            { id: 'ARABICA GAYO', name: 'ARABICA GAYO', price: 45000, image: 'arabica_gayo.jpg', description: 'Biji kopi Arabica dari Aceh Gayo dengan rasa yang khas dan aroma yang kuat.', category: 'biji_kopi', temperature: 'none' },
            { id: 'ROBUSTA MANDAILING', name: 'ROBUSTA MANDAILING', price: 35000, image: 'robusta_mandailing.jpg', description: 'Biji kopi Robusta dari Mandailing dengan rasa yang kuat dan pahit yang khas.', category: 'biji_kopi', temperature: 'none' },
            { id: 'ARABICA TORAJA', name: 'ARABICA TORAJA', price: 50000, image: 'arabica_toraja.jpg', description: 'Biji kopi Arabica dari Toraja dengan aroma yang khas dan rasa yang seimbang.', category: 'biji_kopi', temperature: 'none' },
            { id: 'ROBUSTA DAMPIT', name: 'ROBUSTA DAMPIT', price: 30000, image: 'robusta_dampit.jpg', description: 'Biji kopi Robusta dari Dampit dengan rasa yang pahit dan aroma yang kuat.', category: 'biji_kopi', temperature: 'none' },
            { id: 'ARABICA JAVA', name: 'ARABICA JAVA', price: 40000, image: 'arabica_java.jpg', description: 'Biji kopi Arabica dari Jawa dengan rasa yang seimbang dan aroma yang khas.', category: 'biji_kopi', temperature: 'none' }
        ];

        // --- INISIALISASI FIREBASE ---
        const firebaseConfig = {
          apiKey: "AIzaSyBhRCNri0TjBdMx9fEN8jgAfDD-jV_FdNo",
          authDomain: "waroengmasher.firebaseapp.com",
          projectId: "waroengmasher",
          storageBucket: "waroengmasher.firebasestorage.app",
          messagingSenderId: "984134106741",
          appId: "1:984134106741:web:81d129033d09475173c23c",
          measurementId: "G-7RRW43T1LT"
        };

        // Inisialisasi Firebase
        let db;
        let auth; // Variabel untuk Firebase Auth
        try {
            firebase.initializeApp(firebaseConfig);
            db = firebase.firestore();
            auth = firebase.auth(); // Inisialisasi Auth
        } catch (e) {
            console.error("Firebase Gagal diinisialisasi: ", e);
            alert("Gagal memuat koneksi database. Harap refresh halaman.");
        }


        // --- LOGIKA AUTENTIKASI (LOGIN/LOGOUT) ---
        
        // Ambil elemen UI untuk Auth
        const loginContainer = document.getElementById('login-container');
        const posContainer = document.getElementById('pos-container');
        const loginForm = document.getElementById('login-form');
        const loginError = document.getElementById('login-error');
        const logoutBtn = document.getElementById('logout-btn');
        const userEmailDisplay = document.getElementById('user-email-display');

        // 1. Fungsi untuk menangani Login
        function handleLogin(e) {
            e.preventDefault(); // Mencegah form reload halaman
            const email = document.getElementById('login-email').value;
            const password = document.getElementById('login-password').value;
            loginError.classList.add('d-none'); // Sembunyikan error lama

            auth.signInWithEmailAndPassword(email, password)
                .then((userCredential) => {
                    // Login berhasil, 'onAuthStateChanged' akan menangani sisanya
                    console.log('Login berhasil:', userCredential.user.email);
                })
                .catch((error) => {
                    // Tampilkan error
                    loginError.textContent = 'Email atau Password salah!';
                    loginError.classList.remove('d-none');
                    console.error('Login Gagal:', error.message);
                });
        }

        // 2. Fungsi untuk menangani Logout
        function handleLogout() {
            if (confirm('Apakah Anda yakin ingin logout?')) {
                auth.signOut();
            }
        }

        // 3. Listener Status Autentikasi (Paling Penting)
        // Ini akan otomatis berjalan saat halaman dimuat dan saat login/logout
        auth.onAuthStateChanged((user) => {
            if (user) {
                // --- PENGGUNA SUDAH LOGIN ---
                console.log('User sedang login:', user.email);
                
                // Tampilkan panel kasir dan sembunyikan login
                posContainer.classList.remove('d-none');
                loginContainer.classList.add('d-none');
                
                // Tampilkan email kasir di navbar
                userEmailDisplay.textContent = user.email;

                // Muat data menu dan keranjang HANYA setelah login berhasil
                renderProducts();
                renderCartPOS();

            } else {
                // --- PENGGUNA SUDAH LOGOUT ---
                console.log('User tidak login.');
                
                // Tampilkan login dan sembunyikan panel kasir
                posContainer.classList.add('d-none');
                loginContainer.classList.remove('d-none');
                
                // Bersihkan data kasir
                userEmailDisplay.textContent = '';
                
                // Reset keranjang saat logout
                cart = [];
                discountPercentage = 0;
                if(bayarInput) bayarInput.value = '';
                // Panggil renderCartPOS untuk membersihkan tampilan keranjang
                renderCartPOS(); 
            }
        });


        // --- LOGIKA KASIR (POS) ---
        
        let cart = []; // Keranjang sementara untuk kasir
        let discountPercentage = 0; // Persentase diskon default
        const productListContainer = document.getElementById('product-list-container');
        const cartItemsContainerPOS = document.getElementById('cart-items-container-pos');
        const cartSubtotalPOS = document.getElementById('cart-subtotal-pos');
        const cartTotalPOS = document.getElementById('cart-total-pos');
        const discountAmount = document.getElementById('discount-amount');
        const bayarInput = document.getElementById('bayar-input');
        const kembalianText = document.getElementById('kembalian-text');
        const completeSaleBtn = document.getElementById('complete-sale-btn');
        const cartEmptyMessage = document.getElementById('cart-empty-message');

        // Elemen Modal
        const productDetailModal = new bootstrap.Modal(document.getElementById('productDetailModal'));
        const receiptModal = new bootstrap.Modal(document.getElementById('receiptModal'));
        const modalProductName = document.getElementById('modal-product-name');
        const modalProductImage = document.getElementById('modal-product-image');
        const modalProductDescription = document.getElementById('modal-product-description');
        const modalProductPrice = document.getElementById('modal-product-price');
        const modalAddToCartBtn = document.getElementById('modal-add-to-cart-btn');

        function formatRupiah(number) {
            return new Intl.NumberFormat('id-ID', { style: 'currency', currency: 'IDR', minimumFractionDigits: 0 }).format(number);
        }

        function calculateSubtotal() {
            return cart.reduce((sum, item) => sum + (item.price * item.quantity), 0);
        }

        function calculateTotal() {
            const subtotal = calculateSubtotal();
            const discountValue = subtotal * (discountPercentage / 100);
            return subtotal - discountValue;
        }

        // --- RENDER MENU ---
        function renderProducts(filteredProducts = products) {
            // Cek apakah elemen ada sebelum mengisinya
            if (productListContainer) productListContainer.innerHTML = '';
            
            // Filter berdasarkan kategori yang aktif
            const activeCategory = document.querySelector('.category-tab.active').dataset.category;
            let productsToShow = filteredProducts;
            
            if (activeCategory !== 'all') {
                if (activeCategory === 'hot') {
                    productsToShow = filteredProducts.filter(p => p.category === 'minuman' && (p.temperature === 'hot' || p.temperature === 'both'));
                } else if (activeCategory === 'cold') {
                    productsToShow = filteredProducts.filter(p => p.category === 'minuman' && (p.temperature === 'cold' || p.temperature === 'both'));
                } else if (activeCategory === 'food') {
                    productsToShow = filteredProducts.filter(p => p.category === 'makanan');
                } else if (activeCategory === 'beans') {
                    productsToShow = filteredProducts.filter(p => p.category === 'biji_kopi');
                }
            }
            
            // Kelompokkan produk berdasarkan kategori
            const minumanPanas = productsToShow.filter(p => p.category === 'minuman' && p.temperature === 'hot');
            const minumanDingin = productsToShow.filter(p => p.category === 'minuman' && p.temperature === 'cold');
            const minumanKeduanya = productsToShow.filter(p => p.category === 'minuman' && p.temperature === 'both');
            const makanan = productsToShow.filter(p => p.category === 'makanan');
            const bijiKopi = productsToShow.filter(p => p.category === 'biji_kopi');
            
            // Render setiap kategori
            if (minumanPanas.length > 0) {
                productListContainer.innerHTML += createMenuSection('☕ Minuman Panas', minumanPanas);
            }
            
            if (minumanDingin.length > 0) {
                productListContainer.innerHTML += createMenuSection('🧊 Minuman Dingin', minumanDingin);
            }
            
            if (minumanKeduanya.length > 0) {
                productListContainer.innerHTML += createMenuSection('🥤 Minuman Panas/Dingin', minumanKeduanya);
            }
            
            if (makanan.length > 0) {
                productListContainer.innerHTML += createMenuSection('🍽️ Makanan', makanan);
            }
            
            if (bijiKopi.length > 0) {
                productListContainer.innerHTML += createMenuSection('🫘 Biji Kopi', bijiKopi);
            }
            
            // Tambahkan event listener untuk kartu yang diklik
            document.querySelectorAll('.product-card').forEach(card => {
                card.addEventListener('click', (e) => {
                    // Jika yang diklik adalah tombol detail, jangan tambahkan ke keranjang
                    if (e.target.closest('.btn-detail')) {
                        return;
                    }
                    e.preventDefault();
                    addToCart(e.currentTarget.dataset.id);
                });
            });
        }
        
        // Fungsi untuk membuat bagian menu
        function createMenuSection(title, products) {
            let html = `
                <div class="menu-section">
                    <div class="menu-section-title">
                        <i class="bi bi-tag-fill"></i> ${title}
                    </div>
                    <div class="menu-section-content row row-cols-2 row-cols-sm-3 g-2">
            `;
            
            products.forEach(product => {
                html += createProductCard(product);
            });
            
            html += `
                    </div>
                </div>
            `;
            
            return html;
        }

        function createProductCard(product) {
            // Tentukan badge berdasarkan kategori
            let badgeHtml = '';
            if (product.temperature === 'hot') {
                badgeHtml = '<span class="category-badge hot-badge">PANAS</span>';
            } else if (product.temperature === 'cold') {
                badgeHtml = '<span class="category-badge cold-badge">DINGIN</span>';
            } else if (product.temperature === 'both') {
                badgeHtml = '<span class="category-badge">PANAS/DINGIN</span>';
            } else if (product.category === 'biji_kopi') {
                badgeHtml = '<span class="category-badge coffee-bean-badge">BIJI KOPI</span>';
            }
            
            return `
                <div class="col">
                    <div class="card product-card h-100" data-id="${product.id}">
                        ${badgeHtml}
                        <button class="btn-detail" onclick="showProductDetail('${product.id}')">
                            <i class="bi bi-info-circle"></i>
                        </button>
                        <div class="card-img-container">
                            <img src="${product.image}" class="card-img-top" alt="${product.name}">
                        </div>
                        <div class="card-body d-flex flex-column justify-content-between">
                            <h5 class="card-title text-center">${product.name}</h5>
                            <h6 class="text-center text-white">${formatRupiah(product.price)}</h6>
                        </div>
                    </div>
                </div>
            `;
        }

        // --- FUNGSI DETAIL PRODUK ---
        function showProductDetail(productId) {
            const product = products.find(p => p.id === productId);
            if (!product) return;

            // Isi modal dengan data produk
            modalProductName.textContent = product.name;
            modalProductImage.src = product.image;
            modalProductDescription.textContent = product.description;
            modalProductPrice.textContent = formatRupiah(product.price);

            // Set event listener untuk tombol tambah ke keranjang di modal
            modalAddToCartBtn.onclick = () => {
                addToCart(productId);
                productDetailModal.hide(); // Tutup modal setelah menambahkan
            };

            // Tampilkan modal
            productDetailModal.show();
        }

        // --- FUNGSI KERANJANG KASIR ---
        function addToCart(productId) {
            const existingItemIndex = cart.findIndex(item => item.id === productId);
            const product = products.find(p => p.id === productId);
            
            if (!product) return;

            if (existingItemIndex > -1) {
                cart[existingItemIndex].quantity += 1;
            } else {
                cart.push({ id: product.id, name: product.name, price: product.price, quantity: 1 });
            }
            renderCartPOS();
        }
        
        function updateQuantityPOS(productId, change) {
            const itemIndex = cart.findIndex(item => item.id === productId);
            if (itemIndex > -1) {
                cart[itemIndex].quantity += change;
                if (cart[itemIndex].quantity <= 0) {
                    cart.splice(itemIndex, 1); // Hapus jika kuantitas 0
                }
            }
            renderCartPOS();
        }

        function renderCartPOS() {
            // Cek jika elemen ada (karena bisa dipanggil saat logout)
            if (!cartItemsContainerPOS) return; 

            cartItemsContainerPOS.innerHTML = '';
            
            if (cart.length === 0) {
                cartEmptyMessage.style.display = 'block';
                completeSaleBtn.disabled = true;
            } else {
                cartEmptyMessage.style.display = 'none';
                completeSaleBtn.disabled = false; // Akan di-cek ulang oleh calculateChange
                
                let html = ``;
                cart.forEach(item => {
                    const subtotal = item.price * item.quantity;
                    html += `
                        <div class="d-flex cart-item-pos align-items-center">
                            <div class="flex-grow-1">
                                <h6 class="mb-0">${item.name}</h6>
                                <p class="price-qty mb-0">${formatRupiah(item.price)}</p>
                            </div>
                            <div class="d-flex align-items-center me-2">
                                <button class="btn btn-sm btn-outline-warning btn-qty" onclick="updateQuantityPOS('${item.id}', -1)"><i class="bi bi-dash"></i></button>
                                <span class="text-white fw-bold mx-2">${item.quantity}</span>
                                <button class="btn btn-sm btn-success btn-qty" onclick="updateQuantityPOS('${item.id}', 1)"><i class="bi bi-plus"></i></button>
                            </div>
                            <span class="text-warning fw-bold">${formatRupiah(subtotal)}</span>
                        </div>
                    `;
                });
                cartItemsContainerPOS.innerHTML = html;
            }

            // Update Subtotal, Diskon, dan Total
            const subtotal = calculateSubtotal();
            const discountValue = subtotal * (discountPercentage / 100);
            const total = subtotal - discountValue;
            
            cartSubtotalPOS.textContent = formatRupiah(subtotal);
            discountAmount.textContent = formatRupiah(discountValue);
            cartTotalPOS.textContent = formatRupiah(total);
            
            calculateChange(); // Hitung kembalian setiap ada perubahan keranjang
        }

        // --- FUNGSI DISKON ---
        function applyDiscount(percentage) {
            discountPercentage = percentage;
            
            // Update UI untuk menunjukkan tombol diskon yang aktif
            document.querySelectorAll('.discount-btn').forEach(btn => {
                btn.classList.remove('active');
            });
            event.target.classList.add('active');
            
            renderCartPOS();
        }

        // --- FUNGSI PEMBAYARAN DAN KEMBALIAN ---
        function calculateChange() {
            // Cek jika elemen ada
            if (!kembalianText || !completeSaleBtn || !bayarInput) return;

            const total = calculateTotal();
            const bayar = parseInt(bayarInput.value) || 0;
            const kembalian = bayar - total;

            if (total === 0) {
                kembalianText.textContent = formatRupiah(0);
                completeSaleBtn.disabled = true;
                return;
            }

            if (kembalian >= 0) {
                kembalianText.textContent = formatRupiah(kembalian);
                kembalianText.classList.remove('text-danger');
                kembalianText.classList.add('text-success');
                completeSaleBtn.disabled = false;
            } else {
                kembalianText.textContent = formatRupiah(kembalian);
                kembalianText.classList.remove('text-success');
                kembalianText.classList.add('text-danger');
                completeSaleBtn.disabled = true;
            }

            if (cart.length === 0) {
                completeSaleBtn.disabled = true;
                bayarInput.value = '';
                kembalianText.textContent = formatRupiah(0);
            }
        }

        function completeSale() {
            // Cek apakah db (Firebase) berhasil diinisialisasi
            if (!db) {
                alert('Error! Koneksi database Firebase gagal. Transaksi tidak dapat disimpan. Harap periksa konfigurasi API Key Anda.');
                return;
            }
            
            const subtotal = calculateSubtotal();
            const discountValue = subtotal * (discountPercentage / 100);
            const total = calculateTotal();
            const bayar = parseInt(bayarInput.value) || 0;
            const kembalian = bayar - total;
            
            if (cart.length === 0) {
                alert('Gagal! Keranjang masih kosong.');
                return;
            }

            if (kembalian < 0) {
                alert('Gagal! Jumlah bayar kurang.');
                return;
            }
            
            // Logika Penyelesaian Transaksi
            const transactionId = 'TRX-' + Date.now();
            const transactionData = {
                transactionId: transactionId,
                timestamp: firebase.firestore.FieldValue.serverTimestamp(),
                subtotal: subtotal,
                discount: discountPercentage,
                discountAmount: discountValue,
                total: total,
                bayar: bayar,
                kembalian: kembalian,
                items: cart.map(item => ({ id: item.id, name: item.name, price: item.price, qty: item.quantity })),
                
                // --- TAMBAHAN PENTING ---
                // Menyimpan info kasir yang sedang login
                kasirEmail: auth.currentUser ? auth.currentUser.email : 'Tidak Diketahui',
                kasirUid: auth.currentUser ? auth.currentUser.uid : 'Tidak Diketahui',
                // -------------------------

                status: 'Selesai'
            };

            // Simpan ke Firestore
            // INI SEKARANG AKAN BERHASIL karena user sudah login
            db.collection('transactions').add(transactionData)
                .then(() => {
                    // Tampilkan struk pembayaran
                    showReceipt(transactionData);
                    
                    // Reset Kasir
                    cart = [];
                    discountPercentage = 0;
                    bayarInput.value = '';
                    
                    // Reset UI diskon
                    document.querySelectorAll('.discount-btn').forEach(btn => {
                        btn.classList.remove('active');
                    });
                    document.querySelector('.discount-btn').classList.add('active'); // Set "Tidak Ada" sebagai default
                    
                    renderCartPOS();
                    calculateChange();
                })
                .catch(error => {
                    // Ini adalah error jika DITOLAK OLEH RULES
                    console.error("Error writing document: ", error);
                    alert('Error! Gagal menyimpan transaksi ke database. (Kode: ' + error.code + ')');
                });
        }
        
        // --- FUNGSI STRUK/RECEIPT ---
        function showReceipt(transactionData) {
            // Format tanggal
            const date = new Date();
            const formattedDate = date.toLocaleDateString('id-ID') + ' ' + date.toLocaleTimeString('id-ID');
            
            // Isi data struk
            document.getElementById('receipt-date').textContent = formattedDate;
            document.getElementById('receipt-trans-id').textContent = 'ID: ' + transactionData.transactionId;
            document.getElementById('receipt-subtotal').textContent = formatRupiah(transactionData.subtotal);
            document.getElementById('receipt-discount-percent').textContent = transactionData.discount;
            document.getElementById('receipt-discount-amount').textContent = formatRupiah(transactionData.discountAmount);
            document.getElementById('receipt-total').textContent = formatRupiah(transactionData.total);
            document.getElementById('receipt-cash').textContent = formatRupiah(transactionData.bayar);
            document.getElementById('receipt-change').textContent = formatRupiah(transactionData.kembalian);
            
            // Isi item-item
            let itemsHtml = '';
            transactionData.items.forEach(item => {
                itemsHtml += `
                    <div class="receipt-item">
                        <span>${item.name} (${item.qty}x)</span>
                        <span>${formatRupiah(item.price * item.qty)}</span>
                    </div>
                `;
            });
            document.getElementById('receipt-items').innerHTML = itemsHtml;
            
            // Update receipt preview
            updateReceiptPreview(transactionData, formattedDate);
            
            // Generate QR Code dengan DATA DETAIL dan GAMBAR STRUK
            generateReceiptQRWithImage(transactionData, formattedDate);
            
            // Tampilkan modal struk
            receiptModal.show();
            
            // Tambahkan notifikasi
            setTimeout(() => {
                showNotification('QR Code struk digital berhasil dibuat! Scan untuk menyimpan struk Anda.');
            }, 500);
        }
        
        // Update receipt preview
        function updateReceiptPreview(transactionData, formattedDate) {
            document.getElementById('preview-date').textContent = formattedDate;
            document.getElementById('preview-trans-id').textContent = 'ID: ' + transactionData.transactionId;
            
            let previewItemsHtml = '';
            transactionData.items.forEach(item => {
                previewItemsHtml += `
                    <div class="receipt-preview-item">
                        <span>${item.name} (${item.qty}x)</span>
                        <span>${formatRupiah(item.price * item.qty)}</span>
                    </div>
                `;
            });
            
            // Add summary items
            previewItemsHtml += `
                <div class="receipt-preview-item" style="border-top: 1px dashed #000; padding-top: 3px; margin-top: 3px;">
                    <span>Total:</span>
                    <span>${formatRupiah(transactionData.total)}</span>
                </div>
            `;
            
            document.getElementById('preview-items').innerHTML = previewItemsHtml;
        }
        
        // --- FUNGSI UNTUK GENERATE QR DENGAN GAMBAR STRUK ---
        function generateReceiptQRWithImage(transactionData, formattedDate) {
            const qrContainer = document.getElementById('receipt-qr');
            qrContainer.innerHTML = ''; // Kosongkan dulu

            // Tampilkan loading spinner
            document.getElementById('loading-spinner').style.display = 'flex';

            // Konversi receipt content menjadi gambar menggunakan html2canvas
            html2canvas(document.getElementById('receipt-content'), {
                scale: 2,
                useCORS: true,
                logging: false,
                backgroundColor: '#ffffff'
            }).then(canvas => {
                // Convert canvas to base64 image
                const receiptImage = canvas.toDataURL('image/png');
                
                try {
                    // 1. Buat data terstruktur dengan gambar struk
                    const qrData = {
                        merchant: "WAROENG MASHER",
                        id: transactionData.transactionId,
                        date: formattedDate,
                        items: transactionData.items.map(item => ({ 
                            n: item.name,
                            q: item.qty,
                            p: item.price
                        })),
                        sub: transactionData.subtotal,
                        disc_p: transactionData.discount,
                        disc_a: transactionData.discountAmount,
                        total: transactionData.total,
                        paid: transactionData.bayar,
                        change: transactionData.kembalian,
                        // Tambahkan gambar struk ke data
                        receiptImage: receiptImage
                    };

                    // 2. Ubah data JSON menjadi string
                    const dataString = JSON.stringify(qrData);
                    
                    // 3. ENCODE string itu ke Base64 (agar aman ditaruh di URL)
                    const encodedData = btoa(dataString);

                    // 4. Buat URL yang berisi data ter-encode itu
                    const strukUrl = `https://waroengmasher.firebaseapp.com/struk.html?data=${encodedData}`;

                    // 5. Generate QR Code dengan URL yang berisi data dan gambar
                    new QRCode(qrContainer, {
                        text: strukUrl,
                        width: 150,
                        height: 150,
                        colorDark: "#000000",
                        colorLight: "#ffffff",
                        correctLevel: QRCode.CorrectLevel.L,
                        margin: 2
                    });
                    
                    // Sembunyikan loading spinner
                    document.getElementById('loading-spinner').style.display = 'none';
                    
                } catch (error) {
                    console.error("Gagal generate QR Code:", error);
                    
                    // Sembunyikan loading spinner
                    document.getElementById('loading-spinner').style.display = 'none';
                    
                    // Fallback ke QR tanpa gambar
                    const fallbackText = `ID Transaksi: ${transactionData.transactionId}\nTotal: ${formatRupiah(transactionData.total)}`;
                    new QRCode(qrContainer, {
                        text: fallbackText,
                        width: 150,
                        height: 150,
                        correctLevel: QRCode.CorrectLevel.M
                    });
                }
            }).catch(error => {
                console.error("Gagal mengkonversi struk ke gambar:", error);
                
                // Sembunyikan loading spinner
                document.getElementById('loading-spinner').style.display = 'none';
                
                // Fallback ke QR tanpa gambar
                const fallbackText = `ID Transaksi: ${transactionData.transactionId}\nTotal: ${formatRupiah(transactionData.total)}`;
                new QRCode(qrContainer, {
                    text: fallbackText,
                    width: 150,
                    height: 150,
                    correctLevel: QRCode.CorrectLevel.M
                });
            });
        }
        
        // Fungsi untuk menampilkan notifikasi
        function showNotification(message) {
            const notification = document.getElementById('notification');
            const notificationMessage = document.getElementById('notification-message');
            
            notificationMessage.textContent = message;
            notification.classList.add('show');
            
            setTimeout(() => {
                hideNotification();
            }, 5000);
        }
        
        function hideNotification() {
            const notification = document.getElementById('notification');
            notification.classList.remove('show');
        }
        
        // Fungsi untuk berbagikan struk
        function shareReceipt() {
            const receiptContent = document.getElementById('receipt-content');
            
            if (navigator.share) {
                // Gunakan Web Share API jika tersedia
                const receiptText = extractReceiptText();
                
                navigator.share({
                    title: 'Struk Pembayaran - WAROENG MASHER',
                    text: receiptText,
                    url: window.location.href
                }).then(() => {
                    showNotification('Struk berhasil dibagikan!');
                }).catch((error) => {
                    console.log('Error sharing:', error);
                    copyToClipboard(receiptText);
                });
            } else {
                // Fallback ke clipboard
                const receiptText = extractReceiptText();
                copyToClipboard(receiptText);
            }
        }
        
        // Fungsi untuk menyalin ke clipboard
        function copyToClipboard(text) {
            const textarea = document.createElement('textarea');
            textarea.value = text;
            textarea.style.position = 'fixed';
            textarea.style.opacity = '0';
            document.body.appendChild(textarea);
            textarea.select();
            document.execCommand('copy');
            document.body.removeChild(textarea);
            
            showNotification('Struk berhasil disalin ke clipboard!');
        }
        
        // Fungsi untuk mengekstrak teks struk
        function extractReceiptText() {
            const receiptContent = document.getElementById('receipt-content');
            const receiptText = receiptContent.innerText || receiptContent.textContent;
            return receiptText;
        }
        
        // Fungsi untuk mengunduh struk sebagai gambar
        function downloadReceipt() {
            html2canvas(document.getElementById('receipt-content'), {
                scale: 2,
                useCORS: true,
                logging: false,
                backgroundColor: '#ffffff'
            }).then(canvas => {
                // Convert canvas to blob
                canvas.toBlob(function(blob) {
                    // Create download link
                    const url = URL.createObjectURL(blob);
                    const a = document.createElement('a');
                    a.href = url;
                    a.download = `struk_${new Date().toISOString().split('T')[0]}.png`;
                    document.body.appendChild(a);
                    a.click();
                    document.body.removeChild(a);
                    URL.revokeObjectURL(url);
                    
                    showNotification('Struk berhasil diunduh!');
                });
            });
        }
        
        function printReceipt() {
            window.print();
        }
        
        // --- FUNGSI FILTER/PENCARIAN ---
        function filterProducts() {
            const searchTerm = document.getElementById('menu-search-input').value.toLowerCase();
            const filtered = products.filter(product => 
                product.name.toLowerCase().includes(searchTerm) || 
                product.description.toLowerCase().includes(searchTerm)
            );
            renderProducts(filtered);
        }

        // --- INISIALISASI ---
        document.addEventListener('DOMContentLoaded', () => {
            // 'onAuthStateChanged' akan menangani pemanggilan renderProducts dan renderCartPOS
            
            // --- TAMBAHKAN EVENT LISTENER UNTUK AUTH ---
            loginForm.addEventListener('submit', handleLogin);
            logoutBtn.addEventListener('click', handleLogout);
            
            // Panggil filterproducts saat mengetik di search bar
            document.getElementById('menu-search-input').addEventListener('input', filterProducts);
            
            // Mencegah form submission default pada input pembayaran dan memicu completeSale
            bayarInput.addEventListener('keydown', (e) => {
                if (e.key === 'Enter') {
                    e.preventDefault(); 
                    if (!completeSaleBtn.disabled) {
                        completeSale();
                    }
                }
            });
            
            // Set tombol diskon default
            document.querySelector('.discount-btn').classList.add('active');
            
            // Tambahkan event listener untuk tab kategori
            document.querySelectorAll('.category-tab').forEach(tab => {
                tab.addEventListener('click', function() {
                    // Hapus kelas active dari semua tab
                    document.querySelectorAll('.category-tab').forEach(t => t.classList.remove('active'));
                    // Tambahkan kelas active ke tab yang diklik
                    this.classList.add('active');
                    // Render ulang produk berdasarkan kategori yang dipilih
                    renderProducts();
                });
            });
        });
    </script>
</body>
</html>
