<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ăn Vặt Cùng Thiinh</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Be+Vietnam+Pro:wght@400;500;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Be Vietnam Pro', sans-serif;
        }
        ::-webkit-scrollbar {
            width: 8px;
            height: 8px;
        }
        ::-webkit-scrollbar-track {
            background: #f1f1f1;
        }
        ::-webkit-scrollbar-thumb {
            background: #a8a29e;
            border-radius: 8px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #78716c;
        }
        .page-section.hidden {
            display: none;
        }
        .chat-window {
            transition: opacity 0.3s ease-in-out, transform 0.3s ease-in-out;
            transform: translateY(10px);
            opacity: 0;
            pointer-events: none;
        }
        .chat-window.open {
            transform: translateY(0);
            opacity: 1;
            pointer-events: auto;
        }
        .map-placeholder {
            position: relative;
            width: 100%;
            padding-bottom: 56.25%;
            height: 0;
            overflow: hidden;
            background-color: #e5e7eb;
            border-radius: 0.5rem;
        }
        .map-placeholder iframe,
        .map-placeholder div {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            display: flex;
            align-items: center;
            justify-content: center;
            color: #6b7280;
            border: 0;
        }
        .banner-overlay::before {
            content: "";
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background-color: rgba(0, 0, 0, 0.4);
            border-radius: 0.5rem;
            z-index: 1;
        }
        .banner-content {
            position: relative;
            z-index: 2;
        }
    </style>
</head>
<body class="bg-gray-100 font-sans antialiased">

    <header class="bg-white shadow-md sticky top-0 z-50">
        <nav class="container mx-auto px-4 py-3 flex flex-wrap justify-between items-center gap-x-4 gap-y-2">
            <div class="flex items-center space-x-3 cursor-pointer order-1" onclick="navigateTo('home')">
                <img
                    src="https://i.imgur.com/3iKhwod.png"
                    alt="Logo Ăn Vặt Cùng Thiinh"
                    class="h-10 w-10 md:h-12 md:w-12 rounded-full object-cover flex-shrink-0"
                    onerror="this.style.display='none'"
                />
                <span class="text-lg md:text-xl font-bold text-amber-600 hidden sm:inline whitespace-nowrap">
                    Ăn Vặt Cùng Thiinh
                </span>
            </div>

            <div class="flex items-center gap-4 order-3 md:order-2 w-full md:w-auto mt-2 md:mt-0 flex-grow md:flex-grow-0 justify-center md:justify-start">
                <form id="search-form" class="w-full max-w-md md:max-w-xs lg:max-w-sm">
                   <div class="relative flex items-center w-full">
                     <input
                       type="search"
                       id="search-input"
                       placeholder="Tìm kiếm món ngon..."
                       class="border border-gray-300 rounded-l-full px-4 py-1.5 text-sm focus:outline-none focus:ring-1 focus:ring-amber-500 focus:border-amber-500 w-full"
                     />
                     <button
                       type="submit"
                       class="bg-amber-500 hover:bg-amber-600 text-white px-3 py-1.5 rounded-r-full transition duration-300"
                       aria-label="Tìm kiếm"
                     >
                       <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" viewBox="0 0 20 20" fill="currentColor">
                         <path fill-rule="evenodd" d="M8 4a4 4 0 100 8 4 4 0 000-8zM2 8a6 6 0 1110.89 3.476l4.817 4.817a1 1 0 01-1.414 1.414l-4.816-4.816A6 6 0 012 8z" clip-rule="evenodd" />
                       </svg>
                     </button>
                   </div>
                 </form>
            </div>

            <div class="flex items-center space-x-4 order-2 md:order-3 flex-shrink-0">
                <div class="hidden lg:flex space-x-6">
                    <button onclick="navigateTo('home')" class="text-gray-700 hover:text-amber-600 transition duration-300 font-medium">Trang chủ</button>
                    <button onclick="navigateTo('products')" class="text-gray-700 hover:text-amber-600 transition duration-300 font-medium">Sản phẩm</button>
                    <button onclick="navigateTo('contact')" class="text-gray-700 hover:text-amber-600 transition duration-300 font-medium">Liên hệ</button>
                    <button onclick="navigateTo('support')" class="text-gray-700 hover:text-amber-600 transition duration-300 font-medium">Hỗ trợ</button>
                </div>
                <button onclick="navigateTo('cart')" aria-label="Giỏ hàng" class="relative text-gray-600 hover:text-amber-600 transition duration-300">
                  <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 3h2l.4 2M7 13h10l4-8H5.4M7 13L5.4 5M7 13l-2.293 2.293c-.63.63-.184 1.707.707 1.707H17m0 0a2 2 0 100 4 2 2 0 000-4zm-8 2a2 2 0 11-4 0 2 2 0 014 0z" />
                  </svg>
                  <span id="cart-item-count" class="absolute -top-2 -right-2 bg-red-500 text-white text-xs font-bold rounded-full h-5 w-5 flex items-center justify-center hidden">0</span>
                </button>
                <button id="mobile-menu-button" class="lg:hidden text-gray-600 hover:text-amber-600">
                   <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16m-7 6h7" /></svg>
                </button>
            </div>

             <div id="mobile-menu" class="w-full lg:hidden hidden flex-col space-y-2 mt-2 order-last border-t pt-3">
                <button onclick="navigateTo('home'); closeMobileMenu();" class="block text-left w-full text-gray-700 hover:text-amber-600 transition duration-300 py-1">Trang chủ</button>
                <button onclick="navigateTo('products'); closeMobileMenu();" class="block text-left w-full text-gray-700 hover:text-amber-600 transition duration-300 py-1">Sản phẩm</button>
                <button onclick="navigateTo('contact'); closeMobileMenu();" class="block text-left w-full text-gray-700 hover:text-amber-600 transition duration-300 py-1">Liên hệ</button>
                <button onclick="navigateTo('support'); closeMobileMenu();" class="block text-left w-full text-gray-700 hover:text-amber-600 transition duration-300 py-1">Hỗ trợ</button>
            </div>
          </nav>
    </header>

    <main class="container mx-auto px-4 mt-6 mb-12 min-h-[60vh]">

        <section id="home" class="page-section">
            <div
                class="relative text-white p-12 md:p-20 rounded-lg shadow-lg mb-12 text-center overflow-hidden bg-cover bg-center banner-overlay"
                style="background-image: url('https://cdn.tgdd.vn/2022/01/CookDish/tong-hop-16-mon-an-vat-ngay-tet-de-lam-thom-ngon-hap-dan-avt-1200x676.jpg');"
            >
                <div class="banner-content">
                    <h1 class="text-3xl md:text-5xl font-bold mb-4 drop-shadow-md">Thế Giới Đồ Ăn Vặt</h1>
                    <p class="text-lg md:text-xl mb-6 drop-shadow">Khám phá hương vị thơm ngon, độc đáo chỉ có tại Ăn Vặt Cùng Thiinh!</p>
                    <button onclick="navigateTo('products')" class="bg-white text-amber-600 font-semibold px-6 py-2 rounded-full hover:bg-gray-100 transition duration-300 shadow hover:shadow-md">Mua Ngay</button>
                </div>
            </div>

            <section class="mb-12">
                <h2 class="text-2xl font-semibold mb-6 text-center text-gray-800">Sản Phẩm Nổi Bật</h2>
                <div id="featured-products" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6">
                </div>
            </section>

            <section class="mb-12">
                <h2 class="text-2xl font-semibold mb-6 text-center text-gray-800">Sản Phẩm Bán Chạy</h2>
                <div id="bestselling-products" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6">
                </div>
            </section>

            <section class="mb-12 bg-amber-50 py-12 px-4 rounded-lg shadow">
                <h2 class="text-2xl font-semibold mb-8 text-center text-amber-700">Khách Hàng Nói Gì Về Chúng Tôi</h2>
                <div id="customer-feedback" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
                </div>
            </section>

            <section class="mb-12">
                <h2 class="text-2xl font-semibold mb-6 text-center text-gray-800">Danh Mục Sản Phẩm</h2>
                <div id="category-links" class="flex flex-wrap justify-center gap-3 md:gap-4">
                </div>
            </section>

            <section class="bg-white p-8 rounded-lg shadow border">
                <h2 class="text-2xl font-semibold mb-4 text-center text-amber-700">Về Ăn Vặt Cùng Thiinh</h2>
                <p class="text-gray-600 text-center max-w-2xl mx-auto leading-relaxed">
                  Ăn Vặt Cùng Thiinh tự hào mang đến cho bạn những món ăn vặt chất lượng nhất, được chọn lọc kỹ lưỡng từ những nguyên liệu tươi ngon. Chúng tôi cam kết về vệ sinh an toàn thực phẩm và luôn nỗ lực để bạn có trải nghiệm mua sắm tuyệt vời nhất.
                </p>
            </section>
        </section>

        <section id="products" class="page-section hidden">
            <h1 class="text-3xl font-bold mb-6 text-center text-gray-800">Tất Cả Sản Phẩm</h1>
            <div class="flex flex-col md:flex-row justify-between items-center mb-8 p-4 bg-gray-50 rounded-lg gap-4 border">
                <div>
                  <label for="category-filter" class="mr-2 font-medium text-sm text-gray-700">Lọc theo danh mục:</label>
                  <select id="category-filter" class="p-2 border rounded-md focus:ring-amber-500 focus:border-amber-500 text-sm">
                  </select>
                </div>
                <div>
                  <label for="sort-order" class="mr-2 font-medium text-sm text-gray-700">Sắp xếp theo:</label>
                  <select id="sort-order" class="p-2 border rounded-md focus:ring-amber-500 focus:border-amber-500 text-sm">
                    <option value="default">Mặc định</option>
                    <option value="price-asc">Giá tăng dần</option>
                    <option value="price-desc">Giá giảm dần</option>
                    <option value="name-asc">Tên A-Z</option>
                    <option value="name-desc">Tên Z-A</option>
                  </select>
                </div>
            </div>
            <div id="product-list" class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-6">
            </div>
             <p id="no-products-message" class="text-center text-gray-500 mt-10 hidden">Không tìm thấy sản phẩm phù hợp.</p>
        </section>

        <section id="product-detail" class="page-section hidden">
        </section>

        <section id="cart" class="page-section hidden">
             <h1 class="text-3xl font-bold mb-6 text-center text-gray-800">Giỏ Hàng Của Bạn</h1>
             <div id="cart-content">
             </div>
        </section>

        <section id="contact" class="page-section hidden">
             <h1 class="text-3xl font-bold mb-6 text-center text-gray-800">Liên Hệ Với Chúng Tôi</h1>
            <div class="grid grid-cols-1 md:grid-cols-2 gap-10 bg-white p-8 rounded-lg shadow-lg border">
              <div>
                <h2 class="text-xl font-semibold mb-4 text-amber-600">Thông Tin Cửa Hàng</h2>
                <p class="mb-3"><strong class="font-medium">Tên cửa hàng:</strong> Ăn Vặt Cùng Thiinh</p>
                <p class="mb-3"><strong class="font-medium">Địa chỉ:</strong> 47 Ba Hàng, Thành phố Phổ Yên, Thái Nguyên</p>
                <p class="mb-3"><strong class="font-medium">Số điện thoại:</strong> <a href="tel:0987654321" class="text-blue-600 hover:underline">0987 654 321</a> (Hotline)</p>
                <p class="mb-3"><strong class="font-medium">Email:</strong> <a href="mailto:hangocthiinh@yt5mm.onmicrosoft.com" class="text-blue-600 hover:underline">hangocthiinh@yt5mm.onmicrosoft.com</a></p>
                <p class="mb-3"><strong class="font-medium">Giờ mở cửa:</strong> 8:00 - 21:00 (Thứ 2 - Chủ Nhật)</p>
                <div class="mt-6">
                  <h3 class="text-lg font-semibold mb-3">Mạng xã hội</h3>
                  <div class="flex space-x-4" id="contact-social-links">
                  </div>
                </div>
              </div>

              <div>
                <h2 class="text-xl font-semibold mb-4 text-amber-600">Gửi Tin Nhắn Cho Chúng Tôi</h2>
                <form id="contact-form">
                  <div class="mb-4">
                    <label for="contact-name" class="block text-sm font-medium text-gray-700 mb-1">Họ và tên</label>
                    <input type="text" id="contact-name" name="name" required class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-amber-500 focus:border-amber-500"/>
                  </div>
                  <div class="mb-4">
                    <label for="contact-email" class="block text-sm font-medium text-gray-700 mb-1">Email</label>
                    <input type="email" id="contact-email" name="email" required class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-amber-500 focus:border-amber-500"/>
                  </div>
                  <div class="mb-4">
                    <label for="contact-message" class="block text-sm font-medium text-gray-700 mb-1">Nội dung</label>
                    <textarea id="contact-message" name="message" rows="4" required class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-amber-500 focus:border-amber-500"></textarea>
                  </div>
                  <button type="submit" class="w-full bg-amber-500 hover:bg-amber-600 text-white font-bold py-2 px-4 rounded-md transition duration-300">
                    Gửi đi
                  </button>
                </form>
              </div>
            </div>
             <div class="mt-10">
                <h2 class="text-xl font-semibold mb-4 text-center">Tìm chúng tôi trên bản đồ</h2>
                <div class="map-placeholder">
                    <iframe src="https://www.google.com/maps/embed?pb=!1m14!1m12!1m3!1d5364.380583178441!2d105.87202345763764!3d21.42031551948706!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!5e0!3m2!1sen!2s!4v1745264725355!5m2!1sen!2s" width="600" height="450" style="border:0;" allowfullscreen="" loading="lazy" referrerpolicy="no-referrer-when-downgrade"></iframe>
                </div>
            </div>
        </section>

        <section id="support" class="page-section hidden">
             <h1 class="text-3xl font-bold mb-8 text-center text-gray-800">Trung Tâm Hỗ Trợ</h1>
            <div class="space-y-8 max-w-3xl mx-auto">
              <section class="p-6 bg-white rounded-lg shadow border">
                <h2 class="text-xl font-semibold mb-4 text-amber-600">Hướng Dẫn Mua Hàng</h2>
                <ol class="list-decimal list-inside space-y-2 text-gray-700">
                  <li>Tìm kiếm và chọn sản phẩm bạn yêu thích (sử dụng ô tìm kiếm ở đầu trang nếu cần).</li>
                  <li>Xem chi tiết sản phẩm, chọn số lượng và nhấn "Thêm vào giỏ hàng".</li>
                  <li>Vào giỏ hàng (biểu tượng ở góc trên bên phải) để kiểm tra lại đơn hàng.</li>
                  <li>Nhấn "Tiến hành thanh toán" và điền thông tin giao hàng, chọn hình thức thanh toán.</li>
                  <li>Xác nhận đơn hàng và chờ chúng tôi giao hàng đến bạn!</li>
                </ol>
              </section>
              <section class="p-6 bg-white rounded-lg shadow border">
                <h2 class="text-xl font-semibold mb-4 text-amber-600">Hình Thức Thanh Toán</h2>
                <p class="text-gray-700 mb-3">Chúng tôi hiện hỗ trợ các hình thức thanh toán tiện lợi sau:</p>
                <ul class="list-disc list-inside space-y-2 text-gray-700">
                  <li>
                    <strong>Thanh toán khi nhận hàng (COD):</strong> Bạn thanh toán tiền mặt trực tiếp cho nhân viên giao hàng khi nhận được sản phẩm.
                  </li>
                  <li>
                    <strong>Chuyển khoản ngân hàng:</strong> Thông tin tài khoản sẽ được cung cấp sau khi bạn đặt hàng thành công. Vui lòng ghi rõ Mã đơn hàng trong nội dung chuyển khoản.
                  </li>
                  <li>
                    <strong>Thanh toán qua ví MoMo:</strong> Quét mã QR hoặc thanh toán qua ứng dụng MoMo.
                  </li>
                   <li>
                    <strong>Thanh toán qua ví ZaloPay:</strong> Quét mã QR hoặc thanh toán qua ứng dụng ZaloPay.
                  </li>
                </ul>
              </section>
              <section class="p-6 bg-white rounded-lg shadow border">
                <h2 class="text-xl font-semibold mb-4 text-amber-600">Chính Sách Vận Chuyển</h2>
                <p class="text-gray-700 mb-2">Thời gian giao hàng dự kiến (kể từ khi đơn hàng được xác nhận):</p>
                <ul class="list-disc list-inside space-y-1 text-gray-700">
                  <li>Khu vực Thái Nguyên và các tỉnh lân cận: 1-2 ngày làm việc.</li>
                  <li>Các tỉnh thành khác: 2-4 ngày làm việc (tùy thuộc vào địa chỉ nhận hàng).</li>
                </ul>
                 <p class="text-gray-700 mt-3 mb-2">Lưu ý:</p>
                 <ul class="list-disc list-inside space-y-1 text-gray-700 text-sm">
                     <li>Thời gian giao hàng có thể thay đổi tùy thuộc vào điều kiện thời tiết, tình hình giao thông và các yếu tố khách quan khác.</li>
                     <li>Chúng tôi sẽ thông báo cho bạn nếu có sự chậm trễ trong quá trình giao hàng.</li>
                 </ul>
                <p class="text-gray-700 mt-3">Phí vận chuyển sẽ được tính tự động dựa trên địa chỉ nhận hàng và trọng lượng đơn hàng, hiển thị cụ thể ở bước thanh toán.</p>
                <p class="text-gray-700 mt-2">Chúng tôi có thể áp dụng miễn phí vận chuyển cho các đơn hàng đạt giá trị tối thiểu (sẽ được thông báo nếu có chương trình).</p>
              </section>
              <section class="p-6 bg-white rounded-lg shadow border">
                <h2 class="text-xl font-semibold mb-4 text-amber-600">Chính Sách Đổi/Trả Hàng</h2>
                 <p class="text-gray-700 mb-2">Ăn Vặt Cùng Thiinh cam kết chất lượng sản phẩm. Quý khách vui lòng kiểm tra kỹ hàng hóa khi nhận hàng. Chính sách đổi/trả được áp dụng trong các trường hợp sau:</p>
                <ul class="list-disc list-inside space-y-1 text-gray-700 mb-3">
                    <li>Sản phẩm bị lỗi do nhà sản xuất (bao bì rách, sản phẩm bên trong có dấu hiệu bất thường...).</li>
                    <li>Giao sai sản phẩm, sai số lượng so với đơn đặt hàng.</li>
                    <li>Sản phẩm hết hạn sử dụng tại thời điểm nhận hàng.</li>
                </ul>
                 <p class="text-gray-700 mb-2"><strong>Điều kiện đổi/trả:</strong></p>
                 <ul class="list-disc list-inside space-y-1 text-gray-700 mb-3">
                     <li>Yêu cầu đổi/trả được thực hiện trong vòng <strong>03 ngày</strong> kể từ ngày nhận hàng.</li>
                     <li>Sản phẩm còn nguyên vẹn bao bì, tem mác (nếu có), chưa qua sử dụng hoặc khui mở (trừ trường hợp lỗi bên trong không thể phát hiện khi chưa mở).</li>
                     <li>Cung cấp hóa đơn mua hàng hoặc thông tin xác nhận đơn hàng (ảnh chụp/email...).</li>
                     <li>Cung cấp hình ảnh hoặc video chứng minh tình trạng sản phẩm lỗi/sai sót.</li>
                 </ul>
                <p class="text-gray-700">Để yêu cầu đổi/trả, vui lòng liên hệ Hotline <a href="tel:0987654321" class="text-blue-600 hover:underline">0987 654 321</a> hoặc Email <a href="mailto:hangocthiinh@yt5mm.onmicrosoft.com" class="text-blue-600 hover:underline">hangocthiinh@yt5mm.onmicrosoft.com</a> để được hướng dẫn chi tiết. Chúng tôi không áp dụng đổi/trả với các sản phẩm khuyến mãi, giảm giá (trừ trường hợp lỗi) hoặc lý do cảm tính như không hợp khẩu vị.</p>
              </section>
              <section class="p-6 bg-amber-50 rounded-lg shadow border border-amber-200 text-center">
                <h2 class="text-xl font-semibold mb-3 text-amber-700">Cần Hỗ Trợ Thêm?</h2>
                <p class="text-gray-700 mb-4">Nếu bạn có bất kỳ câu hỏi nào khác, đừng ngần ngại liên hệ với chúng tôi qua Hotline hoặc sử dụng Chatbot AI ở góc màn hình.</p>
                <p class="text-lg font-medium">Hotline Hỗ Trợ: <a href="tel:0987654321" class="text-amber-600 hover:underline">0987 654 321</a></p>
              </section>
            </div>
        </section>

    </main>

    <footer id="footer" class="bg-gray-800 text-white pt-10 pb-6">
    </footer>

    <div id="chatbot-container">
        <button id="chatbot-toggle" class="fixed bottom-5 right-5 bg-amber-500 hover:bg-amber-600 text-white rounded-full p-4 shadow-lg z-50 transition-transform duration-300 hover:scale-110" aria-label="Mở chat hỗ trợ">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 12h.01M12 12h.01M16 12h.01M21 12c0 4.418-4.03 8-9 8a9.863 9.863 0 01-4.255-.949L3 20l1.395-3.72C3.512 15.042 3 13.574 3 12c0-4.418 4.03-8 9-8s9 3.582 9 8z" />
            </svg>
        </button>

        <div id="chat-window" class="chat-window fixed bottom-20 right-5 w-80 sm:w-96 max-h-[70vh] bg-white rounded-lg shadow-xl z-40 flex flex-col border border-gray-300 hidden">
            <div class="bg-amber-500 text-white p-3 flex justify-between items-center rounded-t-lg flex-shrink-0">
                <h3 class="font-semibold text-sm">Hỗ trợ trực tuyến</h3>
                <button id="chatbot-close" class="text-white hover:text-gray-200">
                  <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
                  </svg>
                </button>
            </div>
            <div id="chat-messages" class="flex-grow p-3 overflow-y-auto space-y-3 bg-gray-50" style="min-height: 200px;">
            </div>
            <form id="chat-form" class="p-3 border-t flex flex-shrink-0">
                <input
                  type="text"
                  id="chat-input"
                  placeholder="Nhập câu hỏi..."
                  autocomplete="off"
                  class="flex-grow px-3 py-2 border rounded-l-md focus:outline-none focus:ring-1 focus:ring-amber-500 text-sm"
                />
                <button
                  type="submit"
                  class="bg-amber-500 hover:bg-amber-600 text-white px-4 py-2 rounded-r-md transition duration-300 disabled:opacity-50"
                >
                  Gửi
                </button>
            </form>
            <div class="p-2 text-center text-xs text-gray-500 border-t bg-gray-100 rounded-b-lg flex-shrink-0">
                Hoặc gọi Hotline: <a href="tel:0987654321" class="text-amber-600 font-medium">0987 654 321</a>
            </div>
        </div>
    </div>

    <script>
        const mockProducts = [
          { id: 1, name: 'Bánh Tráng Trộn Sài Gòn Đặc Biệt', price: 215000, originalPrice: 250000, image: 'https://i-giadinh.vnecdn.net/2023/08/05/mon-7-1691221823-6409-1691221866.jpg', description: 'Hương vị Sài Gòn đích thực, cay nồng đậm đà, topping đầy đặn.', category: 'Món trộn', stock: 15, featured: true, bestSeller: false },
          { id: 2, name: 'Khô Gà Lá Chanh Thượng Hạng (Hũ 500g)', price: 350000, originalPrice: null, image: 'https://i-giadinh.vnecdn.net/2022/01/14/Thanh-pham-1-1-9068-1642155637.jpg', description: 'Thịt gà dai ngon, thơm lừng vị lá chanh, chuẩn vị nhà làm.', category: 'Khô các loại', stock: 30, featured: false, bestSeller: true },
          { id: 3, name: 'Da Heo Cháy Tỏi Ớt Siêu Giòn (Hũ Lớn)', price: 280000, originalPrice: 320000, image: 'https://i.ytimg.com/vi/nIxIwNjV-mM/maxresdefault.jpg', description: 'Giòn rụm tan trong miệng, cay cay, ăn là ghiền.', category: 'Món chiên', stock: 5, featured: true, bestSeller: true },
          { id: 4, name: 'Combo Tokbokki Hoàng Gia Kèm Phô Mai', price: 299000, originalPrice: null, image: 'https://hfood.com.vn/images/images/H%C6%B0%E1%BB%9Bng_d%E1%BA%ABn_chi_ti%E1%BA%BFt_c%C3%A1ch_l%C3%A0m_tokbokki_kh%C3%B4ng_cay.jpg', description: 'Chuẩn vị Hàn Quốc, tiện lợi, kèm sốt phô mai béo ngậy.', category: 'Món Hàn Quốc', stock: 20, featured: false, bestSeller: false },
          { id: 5, name: 'Mực Bento Thái Lan Cay Cấp Độ 7 (Set 5 Gói)', price: 310000, originalPrice: null, image: 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRePtL_WouaGSzBf2vP_9FDl2sZL1122Kzs7Q&s', description: 'Cay xé lưỡi, đặc sản Thái Lan không thể bỏ qua, siêu tiết kiệm.', category: 'Món Thái Lan', stock: 12, featured: true, bestSeller: false },
          { id: 6, name: 'Rong Biển Cháy Tỏi Hàn Quốc (Hộp 100g)', price: 205000, originalPrice: null, image: 'https://phucthinhfood.com/uploads/plugin/product_items/384/krs-0029.jpg', description: 'Giòn tan, thơm nức mùi tỏi phi, nhập khẩu Hàn Quốc.', category: 'Món chiên', stock: 0, featured: false, bestSeller: true },
          { id: 7, name: 'Hạt Óc Chó Mỹ Chandler (Túi 500g)', price: 450000, originalPrice: 500000, image: 'https://hatdieu.org/storage/images/IvILrlLJ4wdI7ke94v5S7HCBbXQY2I78CdkUK9ft.jpeg', description: 'Giàu dinh dưỡng Omega-3, tốt cho trí não và tim mạch.', category: 'Các loại hạt', stock: 25, featured: false, bestSeller: true },
          { id: 8, name: 'Hạt Điều Rang Muối Bình Phước Loại A (Hũ 500g)', price: 480000, originalPrice: null, image: 'https://fonut.vn/wp-content/uploads/2020/06/A2-scaled.jpg', description: 'Hạt điều loại A thượng hạng, rang muối giòn thơm, vị đậm đà.', category: 'Các loại hạt', stock: 40, featured: true, bestSeller: true },
          { id: 9, name: 'Hạt Hạnh Nhân Rang Bơ Mỹ (Hũ 400g)', price: 360000, originalPrice: null, image: 'https://dainganfruit.com/wp-content/uploads/2021/07/HAT-HANH-NHAN-RANG-BO-DINH-DUONG-2.jpg', description: 'Thơm ngậy vị bơ, giòn tan, cung cấp năng lượng dồi dào.', category: 'Các loại hạt', stock: 18, featured: false, bestSeller: false },
          { id: 10, name: 'Hạt Dẻ Cười Mỹ Không Tẩy Trắng (Túi 500g)', price: 495000, originalPrice: 550000, image: 'https://holinut.com/wp-content/uploads/2022/10/hat-de-cuoi.jpg', description: 'Hạt tự nhiên, vị bùi ngọt, giàu chất xơ, tốt cho sức khỏe.', category: 'Các loại hạt', stock: 22, featured: false, bestSeller: false },
          { id: 11, name: 'Khô Bò Miếng Cay Đặc Sản (Hộp 250g)', price: 390000, originalPrice: null, image: 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQ7UK0eLDYTmBqlyhN16gEtrXEsdlT1Ce4lww&s', description: 'Thịt bò tươi ngon, tẩm ướp gia vị cay đậm đà theo công thức gia truyền.', category: 'Khô các loại', stock: 35, featured: false, bestSeller: false },
        ];
        const mockCategories = ['Tất cả', 'Món trộn', 'Khô các loại', 'Món chiên', 'Món Hàn Quốc', 'Món Thái Lan', 'Các loại hạt'];
        // Updated feedback data
        const mockFeedbacks = [
            { id: 1, name: 'Hà Ngọc Thịnh', quote: 'Đồ ăn vặt ở đây ngon tuyệt vời, đóng gói cẩn thận, giao hàng nhanh. Sẽ ủng hộ shop dài dài!', avatar: 'https://i.imgur.com/ReZZoPu.jpeg' },
            { id: 2, name: 'Nhữ Ngọc Hà', quote: 'Hạt điều rang muối rất giòn và thơm, không bị hôi dầu. Giá cả hợp lý so với chất lượng.', avatar: 'https://i.imgur.com/VCd5abf.png' },
            { id: 3, name: 'Hà Thị Kiều Trang', quote: 'Bánh tráng trộn đậm đà, cay vừa phải, đúng vị mình thích. Chatbot tư vấn cũng nhiệt tình.', avatar: 'https://i.imgur.com/Fpzjoo1.jpeg' },
            { id: 4, name: 'Đỗ Mai Yến Nhi', quote: 'Khô gà lá chanh ăn rất cuốn, không quá khô. Mua làm quà ai cũng khen ngon.', avatar: 'https://i.imgur.com/ZVlgGa3.jpeg' },
        ];

        let cartItems = [];
        let currentFilterCategory = 'Tất cả';
        let currentSortOrder = 'default';
        let currentSearchTerm = '';

        const formatCurrency = (amount) => {
            if (amount === null || typeof amount === 'undefined') return '';
            return new Intl.NumberFormat('vi-VN', { style: 'currency', currency: 'VND' }).format(amount);
        };

        const pageSections = document.querySelectorAll('.page-section');
        const productListContainer = document.getElementById('product-list');
        const featuredProductsContainer = document.getElementById('featured-products');
        const bestsellingProductsContainer = document.getElementById('bestselling-products');
        const productDetailContainer = document.getElementById('product-detail');
        const cartContentContainer = document.getElementById('cart-content');
        const cartItemCountElement = document.getElementById('cart-item-count');
        const categoryFilterElement = document.getElementById('category-filter');
        const sortOrderElement = document.getElementById('sort-order');
        const noProductsMessage = document.getElementById('no-products-message');
        const categoryLinksContainer = document.getElementById('category-links');
        const customerFeedbackContainer = document.getElementById('customer-feedback');
        const footerContainer = document.getElementById('footer');
        const contactSocialLinksContainer = document.getElementById('contact-social-links');
        const searchForm = document.getElementById('search-form');
        const searchInput = document.getElementById('search-input');
        const chatbotToggle = document.getElementById('chatbot-toggle');
        const chatWindow = document.getElementById('chat-window');
        const chatbotClose = document.getElementById('chatbot-close');
        const chatMessagesContainer = document.getElementById('chat-messages');
        const chatForm = document.getElementById('chat-form');
        const chatInput = document.getElementById('chat-input');
        const mobileMenuButton = document.getElementById('mobile-menu-button');
        const mobileMenu = document.getElementById('mobile-menu');

        function navigateTo(pageId, params = null) {
            pageSections.forEach(section => {
                section.classList.add('hidden');
            });
            const targetPage = document.getElementById(pageId);
            if (targetPage) {
                targetPage.classList.remove('hidden');
            } else {
                document.getElementById('home').classList.remove('hidden');
            }

            if (pageId === 'products') {
                currentFilterCategory = params?.category || 'Tất cả';
                currentSearchTerm = params?.search || '';
                searchInput.value = currentSearchTerm;
                categoryFilterElement.value = currentFilterCategory;
                renderProductList();
            } else if (pageId === 'product-detail' && params?.productId) {
                renderProductDetail(params.productId);
            } else if (pageId === 'cart') {
                renderCart();
            } else if (pageId === 'home') {
                currentFilterCategory = 'Tất cả';
                currentSearchTerm = '';
                searchInput.value = '';
                categoryFilterElement.value = currentFilterCategory;
                sortOrderElement.value = 'default';
                renderHomePageContent();
            }

            window.scrollTo(0, 0);
        }

        function closeMobileMenu() {
            mobileMenu?.classList.add('hidden');
        }

        function createProductCardHTML(product) {
            const hasDiscount = product.originalPrice && product.originalPrice > product.price;
            const discountPercent = hasDiscount ? Math.round(((product.originalPrice - product.price) / product.originalPrice) * 100) : 0;

            return `
                <div class="border rounded-lg overflow-hidden shadow-lg hover:shadow-xl transition-shadow duration-300 bg-white flex flex-col relative group h-full">
                    ${hasDiscount ? `<div class="absolute top-2 right-2 bg-red-500 text-white text-xs font-bold px-2 py-1 rounded-md z-10">GIẢM ${discountPercent}%</div>` : ''}
                    <div class="aspect-square overflow-hidden">
                        <img
                            src="${product.image}"
                            alt="[Hình ảnh của ${product.name}]"
                            class="w-full h-full object-cover cursor-pointer group-hover:scale-105 transition-transform duration-300"
                            onerror="this.onerror=null; this.src='https://placehold.co/300x300/cccccc/ffffff?text=Image+Not+Found';"
                            onclick="navigateTo('product-detail', { productId: ${product.id} })"
                        />
                    </div>
                    <div class="p-4 flex flex-col flex-grow">
                        <h3 class="text-base font-semibold mb-2 truncate group-hover:text-amber-700 transition duration-200" title="${product.name}">${product.name}</h3>
                        <div class="mb-3">
                            <p class="text-amber-600 font-bold text-md">${formatCurrency(product.price)}</p>
                            ${hasDiscount ? `<p class="text-xs text-gray-500 line-through">${formatCurrency(product.originalPrice)}</p>` : ''}
                        </div>
                        <div class="mt-auto flex justify-between items-center pt-2">
                            <button onclick="navigateTo('product-detail', { productId: ${product.id} })" class="text-xs text-blue-600 hover:text-blue-800 transition duration-300">
                                Xem chi tiết
                            </button>
                            ${product.stock > 0 ?
                                `<button onclick="addToCart(${product.id})" class="bg-amber-500 hover:bg-amber-600 text-white text-xs px-3 py-1 rounded-md transition duration-300">
                                    Thêm giỏ
                                </button>` :
                                `<span class="text-xs text-red-500 font-medium">Hết hàng</span>`
                            }
                        </div>
                    </div>
                </div>
            `;
        }

        function renderProductList() {
            if (!productListContainer) return;
            let productsToRender = [...mockProducts];
            if (currentFilterCategory !== 'Tất cả') {
                productsToRender = productsToRender.filter(p => p.category === currentFilterCategory);
            }
            if (currentSearchTerm) {
                const lowerSearchTerm = currentSearchTerm.toLowerCase();
                productsToRender = productsToRender.filter(p =>
                    p.name.toLowerCase().includes(lowerSearchTerm) ||
                    (p.description && p.description.toLowerCase().includes(lowerSearchTerm)) ||
                    p.category.toLowerCase().includes(lowerSearchTerm)
                );
            }
            switch (currentSortOrder) {
                case 'price-asc': productsToRender.sort((a, b) => a.price - b.price); break;
                case 'price-desc': productsToRender.sort((a, b) => b.price - a.price); break;
                case 'name-asc': productsToRender.sort((a, b) => a.name.localeCompare(b.name, 'vi')); break;
                case 'name-desc': productsToRender.sort((a, b) => b.name.localeCompare(a.name, 'vi')); break;
            }
            productListContainer.innerHTML = productsToRender.map(createProductCardHTML).join('');
            noProductsMessage.classList.toggle('hidden', productsToRender.length > 0);
        }

        function renderProductDetail(productId) {
             if (!productDetailContainer) return;
            const product = mockProducts.find(p => p.id === productId);
            if (!product) {
                productDetailContainer.innerHTML = `<div class="text-center py-10"><h1 class="text-2xl font-semibold text-red-600 mb-4">Sản phẩm không tồn tại!</h1><button onclick="navigateTo('products')" class="bg-amber-500 hover:bg-amber-600 text-white px-4 py-2 rounded-md transition duration-300">Quay lại sản phẩm</button></div>`;
                return;
            }
            const hasDiscount = product.originalPrice && product.originalPrice > product.price;
            const discountPercent = hasDiscount ? Math.round(((product.originalPrice - product.price) / product.originalPrice) * 100) : 0;
            productDetailContainer.innerHTML = `
                <button onclick="navigateTo('products')" class="mb-6 text-blue-600 hover:text-blue-800 flex items-center text-sm">
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 mr-1" viewBox="0 0 20 20" fill="currentColor"><path fill-rule="evenodd" d="M12.707 5.293a1 1 0 010 1.414L9.414 10l3.293 3.293a1 1 0 01-1.414 1.414l-4-4a1 1 0 010-1.414l4-4a1 1 0 011.414 0z" clip-rule="evenodd" /></svg>
                    Quay lại sản phẩm
                </button>
                <div class="grid grid-cols-1 md:grid-cols-2 gap-8 lg:gap-12 bg-white p-6 rounded-lg shadow border">
                    <div class="relative">
                        ${hasDiscount ? `<div class="absolute top-3 left-3 bg-red-500 text-white text-sm font-bold px-3 py-1 rounded-md z-10 shadow">GIẢM ${discountPercent}%</div>` : ''}
                        <img src="${product.image}" alt="[Hình ảnh chi tiết của ${product.name}]" class="w-full h-auto max-h-[500px] object-contain rounded-lg shadow-md border" onerror="this.onerror=null; this.src='https://placehold.co/600x600/cccccc/ffffff?text=Image+Not+Found';"/>
                    </div>
                    <div>
                        <h1 class="text-2xl lg:text-3xl font-bold mb-3">${product.name}</h1>
                        <div class="mb-4 flex items-baseline space-x-3">
                            <p class="text-2xl lg:text-3xl text-red-600 font-bold">${formatCurrency(product.price)}</p>
                            ${hasDiscount ? `<p class="text-lg lg:text-xl text-gray-500 line-through">${formatCurrency(product.originalPrice)}</p>` : ''}
                        </div>
                        <p class="text-gray-700 mb-4 leading-relaxed text-sm">${product.description}</p>
                        <p class="mb-4 text-sm"><span class="font-semibold">Tình trạng: </span>${product.stock > 0 ? `<span class="text-green-600 font-medium">Còn hàng (${product.stock})</span>` : `<span class="text-red-600 font-medium">Hết hàng</span>`}</p>
                        <p class="mb-4 text-sm"><span class="font-semibold">Danh mục: </span> ${product.category}</p>
                        ${product.stock > 0 ? `
                            <div class="flex items-center mb-6">
                                <span class="mr-3 font-semibold text-sm">Số lượng:</span>
                                <button id="decrease-qty-${productId}" onclick="updateQuantityInput(${productId}, -1)" class="bg-gray-200 px-3 py-1 rounded-l hover:bg-gray-300 disabled:opacity-50 transition duration-200">-</button>
                                <input type="number" id="quantity-input-${productId}" value="1" min="1" max="${product.stock}" class="w-12 text-center border-t border-b border-gray-300 py-1 focus:outline-none text-sm" onchange="handleQuantityInputChange(${productId})">
                                <button id="increase-qty-${productId}" onclick="updateQuantityInput(${productId}, 1)" class="bg-gray-200 px-3 py-1 rounded-r hover:bg-gray-300 disabled:opacity-50 transition duration-200">+</button>
                            </div>
                            <button onclick="handleAddToCartFromDetail(${productId})" class="w-full bg-amber-500 hover:bg-amber-600 text-white font-bold py-3 px-6 rounded-lg transition duration-300 flex items-center justify-center text-base shadow hover:shadow-lg">
                                <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-2" viewBox="0 0 20 20" fill="currentColor"><path d="M3 1a1 1 0 000 2h1.22l.305 1.222a.997.997 0 00.922.778h9.906l-1.17 4.681a1.001 1.001 0 01-.97.719H6.878a1 1 0 01-.996-.858L5.5 6.281A1.001 1.001 0 004.504 5H3a1 1 0 000 2h1.05l1.737 6.948A1 1 0 007.28 15h7.44a1 1 0 00.97-.719l2.716-10.862A1 1 0 0017.437 3H5.219L4.914 1.778A1 1 0 003.992 1H3zm4.28 12a1.5 1.5 0 100 3 1.5 1.5 0 000-3zm8 0a1.5 1.5 0 100 3 1.5 1.5 0 000-3z" /></svg>
                                Thêm vào giỏ hàng
                            </button>
                        ` : `<button disabled class="w-full bg-gray-400 text-white font-bold py-3 px-6 rounded-lg cursor-not-allowed text-base">Đã hết hàng</button>`}
                    </div>
                </div>`;
            updateQuantityButtonStates(productId);
        }

        function renderCart() {
             if (!cartContentContainer) return;
            if (cartItems.length === 0) {
                cartContentContainer.innerHTML = `<div class="text-center py-10 bg-white rounded-lg shadow border"><p class="text-xl text-gray-500 mb-4">Giỏ hàng của bạn đang trống.</p><button onclick="navigateTo('products')" class="bg-amber-500 hover:bg-amber-600 text-white px-6 py-2 rounded-md transition duration-300">Tiếp tục mua sắm</button></div>`;
                return;
            }
            const itemsHTML = cartItems.map(item => {
                 const hasDiscount = item.originalPrice && item.originalPrice > item.price;
                 return `
                    <div class="flex flex-col sm:flex-row items-center justify-between p-4 border rounded-lg bg-white shadow-sm gap-4">
                        <div class="flex items-center space-x-4 flex-grow w-full sm:w-auto">
                            <img src="${item.image}" alt="[Hình ảnh nhỏ của ${item.name}]" class="w-16 h-16 object-cover rounded flex-shrink-0" onerror="this.onerror=null; this.src='https://placehold.co/64x64/cccccc/ffffff?text=N/A';"/>
                            <div class="flex-grow"><h3 class="font-semibold text-sm sm:text-base">${item.name}</h3><p class="text-sm ${hasDiscount ? 'text-red-600 font-medium' : 'text-gray-600'}">${formatCurrency(item.price)}</p>${hasDiscount ? `<p class="text-xs text-gray-500 line-through">${formatCurrency(item.originalPrice)}</p>` : ''}</div>
                        </div>
                        <div class="flex items-center space-x-3 flex-shrink-0 w-full sm:w-auto justify-end">
                            <div class="flex items-center"><button onclick="updateCartQuantity(${item.id}, ${item.quantity - 1})" ${item.quantity <= 1 ? 'disabled' : ''} class="bg-gray-200 px-2 py-0.5 rounded-l hover:bg-gray-300 disabled:opacity-50 text-sm"> - </button><span class="px-3 py-0.5 border-t border-b text-sm bg-white">${item.quantity}</span><button onclick="updateCartQuantity(${item.id}, ${item.quantity + 1})" ${item.quantity >= item.stock ? 'disabled' : ''} class="bg-gray-200 px-2 py-0.5 rounded-r hover:bg-gray-300 disabled:opacity-50 text-sm">+</button></div>
                            <button onclick="removeFromCart(${item.id})" class="text-red-500 hover:text-red-700 transition duration-300 ml-2"><svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" viewBox="0 0 20 20" fill="currentColor"><path fill-rule="evenodd" d="M9 2a1 1 0 00-.894.553L7.382 4H4a1 1 0 000 2v10a2 2 0 002 2h8a2 2 0 002-2V6a1 1 0 100-2h-3.382l-.724-1.447A1 1 0 0011 2H9zM7 8a1 1 0 012 0v6a1 1 0 11-2 0V8zm4 0a1 1 0 012 0v6a1 1 0 11-2 0V8z" clip-rule="evenodd" /></svg></button>
                        </div>
                    </div>`;
            }).join('');
            const total = cartItems.reduce((sum, item) => sum + item.price * item.quantity, 0);
            cartContentContainer.innerHTML = `<div class="grid grid-cols-1 lg:grid-cols-3 gap-8"><div class="lg:col-span-2 space-y-4">${itemsHTML}</div><div class="lg:col-span-1"><div class="bg-gray-50 p-6 rounded-lg shadow-sm border sticky top-24"><h2 class="text-xl font-semibold mb-4">Tóm Tắt Đơn Hàng</h2><div class="flex justify-between mb-2 text-sm"><span>Tạm tính:</span><span>${formatCurrency(total)}</span></div><div class="flex justify-between mb-4 pb-4 border-b text-sm"><span>Phí vận chuyển:</span><span>(Tính khi thanh toán)</span></div><div class="flex justify-between font-bold text-lg mb-6"><span>Tổng cộng:</span><span class="text-red-600">${formatCurrency(total)}</span></div><button onclick="alert('Chức năng thanh toán chưa được triển khai!')" class="w-full bg-amber-500 hover:bg-amber-600 text-white font-bold py-3 rounded-lg transition duration-300 shadow hover:shadow-lg">Tiến hành thanh toán</button></div></div></div>`;
        }

        function renderCategoryFilters() {
            if (!categoryFilterElement) return;
            categoryFilterElement.innerHTML = mockCategories.map(cat => `<option value="${cat}">${cat}</option>`).join('');
        }

        function renderCategoryLinks() {
            if (!categoryLinksContainer) return;
            const categoriesToLink = mockCategories.filter(cat => cat !== 'Tất cả');
            categoryLinksContainer.innerHTML = categoriesToLink.map(cat => `<button onclick="navigateTo('products', { category: '${cat}' })" class="bg-gray-200 hover:bg-amber-200 text-gray-700 px-4 py-2 rounded-full transition duration-300 text-sm">${cat}</button>`).join('');
        }

        function renderCustomerFeedback() {
            if (!customerFeedbackContainer) return;
            customerFeedbackContainer.innerHTML = mockFeedbacks.map(feedback => `<div class="bg-white p-6 rounded-lg shadow-md border border-amber-100 text-center flex flex-col items-center h-full"><img src="${feedback.avatar}" alt="[Ảnh đại diện của ${feedback.name}]" class="w-20 h-20 rounded-full mb-4 object-cover border-2 border-amber-300 flex-shrink-0" onerror="this.onerror=null; this.src='https://placehold.co/100x100/cccccc/ffffff?text=?';"/><p class="text-gray-600 italic mb-3 text-sm flex-grow">"${feedback.quote}"</p><p class="font-semibold text-amber-600 mt-auto text-sm">${feedback.name}</p></div>`).join('');
        }

        function renderSocialLinks(containerId) {
            const container = document.getElementById(containerId);
            if (!container) return;
            const socialLinks = [
                { name: 'Facebook', url: '#', iconClass: 'hover:text-blue-500', svgPath: 'M22 12c0-5.523-4.477-10-10-10S2 6.477 2 12c0 4.991 3.657 9.128 8.438 9.878v-6.987h-2.54V12h2.54V9.797c0-2.506 1.492-3.89 3.777-3.89 1.094 0 2.238.195 2.238.195v2.46h-1.26c-1.243 0-1.63.771-1.63 1.562V12h2.773l-.443 2.89h-2.33v6.988C18.343 21.128 22 16.991 22 12z' },
                { name: 'Instagram', url: '#', iconClass: 'hover:text-pink-500', svgPath: 'M12.315 2.104a5.87 5.87 0 00-4.63 0C3.381 2.56 1.087 4.854.622 9.135a5.87 5.87 0 000 4.63c.465 4.281 2.76 6.575 7.045 7.04a5.87 5.87 0 004.63 0c4.285-.465 6.58-2.76 7.04-7.04a5.87 5.87 0 000-4.63c-.46-4.281-2.755-6.575-7.04-7.04zm-1.19 16.097a4.08 4.08 0 01-3.42-.465c-2.91-.93-4.516-3.006-4.837-6.14a4.08 4.08 0 010-3.42c.321-3.135 1.927-5.21 4.837-6.14a4.08 4.08 0 013.42-.465c2.91.93 4.516 3.006 4.837 6.14a4.08 4.08 0 010 3.42c-.321 3.135-1.927 5.21-4.837 6.14a4.08 4.08 0 01-3.42.465zM12 6.865a5.135 5.135 0 100 10.27 5.135 5.135 0 000-10.27zm0 8.468a3.333 3.333 0 110-6.666 3.333 3.333 0 010 6.666zm5.338-9.87a1.2 1.2 0 100 2.4 1.2 1.2 0 000-2.4z' },
                { name: 'TikTok', url: '#', iconClass: 'hover:text-black', svgPath: 'M12.525.02c1.31-.02 2.61-.01 3.91-.02.08 1.53.63 3.09 1.75 4.17 1.12 1.11 2.7 1.62 4.24 1.79v4.03c-1.44-.05-2.89-.35-4.2-.97-.57-.26-1.1-.59-1.62-.93-.01 2.92.01 5.84-.02 8.75-.08 1.4-.54 2.79-1.35 3.94-1.31 1.92-3.58 3.17-5.91 3.21-1.43.08-2.86-.31-4.08-1.03-2.02-1.19-3.44-3.37-3.65-5.71-.02-.5-.03-1-.01-1.49.18-1.9 1.12-3.72 2.58-4.96 1.66-1.44 3.98-2.13 6.15-1.72.02 1.48-.04 2.96-.04 4.44-.99-.32-2.15-.23-3.02.37-.63.41-1.11 1.04-1.36 1.75-.21.51-.15 1.07-.14 1.61.24 1.64 1.82 3.02 3.5 2.87 1.12-.01 2.19-.66 2.77-1.61.19-.33.4-.67.41-1.06.1-1.79.06-3.57.07-5.36.01-4.03-.01-8.05.02-12.07z' },
                { name: 'YouTube', url: '#', iconClass: 'hover:text-red-600', svgPath: 'M19.812 5.418c.861.23 1.538.907 1.768 1.768C21.998 8.78 22 12 22 12s0 3.22-.418 4.814a2.504 2.504 0 0 1-1.768 1.768c-1.594.418-7.814.418-7.814.418s-6.22 0-7.814-.418a2.505 2.505 0 0 1-1.768-1.768C2 15.22 2 12 2 12s0-3.22.418-4.814a2.505 2.505 0 0 1 1.768-1.768C5.78 5 12 5 12 5s6.22 0 7.812.418ZM9.545 15.568V8.432L15.818 12l-6.273 3.568Z' }
            ];
            const iconSizeClass = containerId === 'footer-social-links' ? 'w-6 h-6' : 'w-8 h-8';
            const baseIconClass = containerId === 'footer-social-links' ? 'text-gray-400' : 'text-gray-600';
            container.innerHTML = socialLinks.map(link => `<a href="${link.url}" target="_blank" rel="noopener noreferrer" title="${link.name}" class="${baseIconClass} ${link.iconClass} transition duration-300"><svg class="${iconSizeClass}" fill="currentColor" viewBox="0 0 24 24" aria-hidden="true"><path fill-rule="evenodd" d="${link.svgPath}" clip-rule="evenodd" /></svg></a>`).join('');
        }

        function renderFooter() {
            if(!footerContainer) return;
            footerContainer.innerHTML = `
                <div class="container mx-auto px-4 grid grid-cols-1 md:grid-cols-3 gap-8">
                  <div>
                     <div class="flex items-center space-x-2 mb-4">
                        <img src="https://i.imgur.com/3iKhwod.png" alt="Logo Ăn Vặt Cùng Thiinh" class="h-10 w-10 rounded-full object-cover" onerror="this.style.display = 'none';"/>
                        <h3 class="text-xl font-semibold text-amber-400">Ăn Vặt Cùng Thiinh</h3>
                     </div>
                    <p class="text-gray-400 text-sm">Chuyên cung cấp các loại đồ ăn vặt thơm ngon, chất lượng, giao hàng tận nơi.</p>
                    <div id="footer-social-links" class="flex space-x-4 mt-4"></div>
                  </div>
                  <div>
                    <h3 class="text-lg font-semibold mb-4">Liên kết nhanh</h3>
                    <ul class="space-y-2">
                      <li><button onclick="navigateTo('home')" class="text-gray-400 hover:text-white text-sm">Trang chủ</button></li>
                      <li><button onclick="navigateTo('products')" class="text-gray-400 hover:text-white text-sm">Sản phẩm</button></li>
                      <li><button onclick="navigateTo('contact')" class="text-gray-400 hover:text-white text-sm">Liên hệ</button></li>
                      <li><button onclick="navigateTo('support')" class="text-gray-400 hover:text-white text-sm">Hỗ trợ</button></li>
                    </ul>
                  </div>
                  <div>
                    <h3 class="text-lg font-semibold mb-4">Thông tin liên hệ</h3>
                    <p class="text-gray-400 mb-2 text-sm">Địa chỉ: 47 Ba Hàng, Thành phố Phổ Yên, Thái Nguyên</p>
                    <p class="text-gray-400 mb-2 text-sm">Điện thoại: <a href="tel:0987654321" class="hover:text-white">0987 654 321</a></p>
                    <p class="text-gray-400 text-sm">Email: <a href="mailto:hangocthiinh@yt5mm.onmicrosoft.com" class="hover:text-white">hangocthiinh@yt5mm.onmicrosoft.com</a></p>
                  </div>
                </div>
                <div class="text-center text-gray-500 mt-8 pt-4 border-t border-gray-700 text-sm">
                  © ${new Date().getFullYear()} Ăn Vặt Cùng Thiinh. Đã đăng ký bản quyền.
                </div>`;
            renderSocialLinks('footer-social-links');
        }

        function loadCart() { const savedCart = localStorage.getItem('snackCart'); cartItems = savedCart ? JSON.parse(savedCart) : []; updateCartIcon(); }
        function saveCart() { localStorage.setItem('snackCart', JSON.stringify(cartItems)); }
        function addToCart(productId, quantity = 1) { const product = mockProducts.find(p => p.id === productId); if (!product) { alert('Sản phẩm không tồn tại!'); return; } if (product.stock <= 0) { alert(`Sản phẩm "${product.name}" đã hết hàng.`); return; } const existingItem = cartItems.find(item => item.id === productId); if (existingItem) { const newQuantity = existingItem.quantity + quantity; if (newQuantity <= product.stock) { existingItem.quantity = newQuantity; } else { alert(`Chỉ còn ${product.stock} sản phẩm "${product.name}" trong kho. Đã thêm tối đa vào giỏ.`); existingItem.quantity = product.stock; } } else { const addQuantity = Math.min(quantity, product.stock); if(addQuantity > 0) { cartItems.push({ ...product, quantity: addQuantity }); } else { return; } } saveCart(); updateCartIcon(); alert(`Đã thêm ${quantity} "${product.name}" vào giỏ hàng.`); if (!document.getElementById('cart')?.classList.contains('hidden')) { renderCart(); } }
        function handleAddToCartFromDetail(productId) { const quantityInput = document.getElementById(`quantity-input-${productId}`); const quantity = quantityInput ? parseInt(quantityInput.value, 10) : 1; if (isNaN(quantity) || quantity < 1) { addToCart(productId, 1); } else { addToCart(productId, quantity); } }
        function updateCartQuantity(productId, newQuantity) { const itemIndex = cartItems.findIndex(item => item.id === productId); if (itemIndex > -1) { const product = mockProducts.find(p => p.id === productId); if (newQuantity <= 0) { removeFromCart(productId); } else if (product && newQuantity <= product.stock) { cartItems[itemIndex].quantity = newQuantity; } else if (product) { alert(`Chỉ còn ${product.stock} sản phẩm "${product.name}" trong kho.`); cartItems[itemIndex].quantity = product.stock; } } saveCart(); updateCartIcon(); renderCart(); }
        function removeFromCart(productId) { cartItems = cartItems.filter(item => item.id !== productId); saveCart(); updateCartIcon(); renderCart(); }
        function updateCartIcon() { const totalItems = cartItems.reduce((sum, item) => sum + item.quantity, 0); if (cartItemCountElement) { cartItemCountElement.textContent = totalItems; cartItemCountElement.classList.toggle('hidden', totalItems === 0); } }

        function updateQuantityInput(productId, change) { const input = document.getElementById(`quantity-input-${productId}`); const product = mockProducts.find(p => p.id === productId); if (input && product) { let currentValue = parseInt(input.value, 10); let newValue = currentValue + change; newValue = Math.max(1, newValue); newValue = Math.min(newValue, product.stock); input.value = newValue; updateQuantityButtonStates(productId); } }
        function handleQuantityInputChange(productId) { const input = document.getElementById(`quantity-input-${productId}`); const product = mockProducts.find(p => p.id === productId); if (input && product) { let value = parseInt(input.value, 10); if (isNaN(value) || value < 1) { input.value = 1; } else if (value > product.stock) { alert(`Chỉ còn ${product.stock} sản phẩm.`); input.value = product.stock; } updateQuantityButtonStates(productId); } }
        function updateQuantityButtonStates(productId) { const input = document.getElementById(`quantity-input-${productId}`); const decreaseBtn = document.getElementById(`decrease-qty-${productId}`); const increaseBtn = document.getElementById(`increase-qty-${productId}`); const product = mockProducts.find(p => p.id === productId); if (input && decreaseBtn && increaseBtn && product) { const currentValue = parseInt(input.value, 10); decreaseBtn.disabled = currentValue <= 1; increaseBtn.disabled = currentValue >= product.stock; } }

        let chatMessages = [ { sender: 'ai', text: 'Xin chào! Tôi là trợ lý ảo của Ăn Vặt Cùng Thiinh. Tôi có thể giúp gì cho bạn?' } ];
        let isChatbotTyping = false;
        function renderChatMessages() { if (!chatMessagesContainer) return; chatMessagesContainer.innerHTML = chatMessages.map(msg => `<div class="flex ${msg.sender === 'user' ? 'justify-end' : 'justify-start'}"><div class="px-3 py-2 rounded-lg max-w-[80%] text-sm ${ msg.sender === 'user' ? 'bg-blue-500 text-white' : msg.isError ? 'bg-red-100 text-red-700' : 'bg-gray-200 text-gray-800' }">${msg.text}</div></div>`).join(''); if (isChatbotTyping) { chatMessagesContainer.innerHTML += `<div class="flex justify-start typing-indicator"><div class="px-3 py-2 rounded-lg bg-gray-200 text-gray-500 italic text-sm">Đang trả lời...</div></div>`; } chatMessagesContainer.scrollTop = chatMessagesContainer.scrollHeight; }
        async function simulateBackendChatResponse(userMessage) { isChatbotTyping = true; renderChatMessages(); await new Promise(resolve => setTimeout(resolve, 1500)); let aiResponseText = `(Phản hồi mô phỏng cho: "${userMessage}") - Tôi chỉ có thể trả lời các câu hỏi về cửa hàng thôi ạ.`; const lowerMsg = userMessage.toLowerCase(); if (lowerMsg.includes("vận chuyển") || lowerMsg.includes("giao hàng")) { aiResponseText = "Shop giao hàng toàn quốc ạ. Khu vực Thái Nguyên và tỉnh lân cận mất 1-2 ngày, các tỉnh khác 2-4 ngày. Phí ship sẽ được tính khi thanh toán."; } else if (lowerMsg.includes("thanh toán")) { aiResponseText = "Bạn có thể thanh toán khi nhận hàng (COD), chuyển khoản, hoặc qua ví MoMo/ZaloPay nhé."; } else if (lowerMsg.includes("đổi trả")) { aiResponseText = "Shop hỗ trợ đổi trả trong 3 ngày nếu sản phẩm lỗi hoặc giao sai. Bạn xem chi tiết ở trang Hỗ trợ hoặc liên hệ hotline nha."; } else if (lowerMsg.includes("bánh tráng")) { aiResponseText = "Bánh tráng trộn Sài Gòn đặc biệt bên shop topping đầy đặn, vị đậm đà lắm ạ!"; } else if (lowerMsg.includes("hạt điều") || lowerMsg.includes("hạt óc chó")) { aiResponseText = "Các loại hạt bên shop đều là hàng loại 1, đảm bảo chất lượng ạ. Hạt điều giòn thơm, óc chó thì bổ dưỡng lắm."; } else if (lowerMsg.includes("chào") || lowerMsg.includes("hello")) { aiResponseText = "Chào bạn! Bạn cần Thiinh hỗ trợ thông tin gì về đồ ăn vặt ạ?"; } else if (lowerMsg.includes("cảm ơn") || lowerMsg.includes("thank")) { aiResponseText = "Dạ không có gì ạ! Rất vui được hỗ trợ bạn."; } isChatbotTyping = false; chatMessages.push({ sender: 'ai', text: aiResponseText }); renderChatMessages(); }

        function initializePage() {
            loadCart();
            renderHomePageContent();
            renderCategoryFilters();
            renderFooter();
            renderSocialLinks('contact-social-links');
            navigateTo('home');

            categoryFilterElement?.addEventListener('change', (e) => { currentFilterCategory = e.target.value; renderProductList(); });
            sortOrderElement?.addEventListener('change', (e) => { currentSortOrder = e.target.value; renderProductList(); });
            searchForm?.addEventListener('submit', (e) => { e.preventDefault(); currentSearchTerm = searchInput.value; navigateTo('products', { search: currentSearchTerm }); });
            document.getElementById('contact-form')?.addEventListener('submit', (e) => { e.preventDefault(); alert('Chức năng gửi liên hệ chưa được triển khai!'); });
            mobileMenuButton?.addEventListener('click', () => { mobileMenu?.classList.toggle('hidden'); });
            chatbotToggle?.addEventListener('click', () => { chatWindow.classList.toggle('hidden'); if (!chatWindow.classList.contains('hidden')) { setTimeout(() => chatWindow.classList.add('open'), 10); renderChatMessages(); } else { chatWindow.classList.remove('open'); } });
            chatbotClose?.addEventListener('click', () => { chatWindow.classList.add('hidden'); chatWindow.classList.remove('open'); });
            chatForm?.addEventListener('submit', (e) => { e.preventDefault(); const userInput = chatInput.value.trim(); if (userInput && !isChatbotTyping) { chatMessages.push({ sender: 'user', text: userInput }); renderChatMessages(); chatInput.value = ''; simulateBackendChatResponse(userInput); } });
            renderChatMessages();
        }

        function renderHomePageContent() {
            const featured = mockProducts.filter(p => p.featured).slice(0, 4);
            const bestselling = mockProducts.filter(p => p.bestSeller).slice(0, 4);
            if (featuredProductsContainer) { featuredProductsContainer.innerHTML = featured.map(createProductCardHTML).join(''); }
            if (bestsellingProductsContainer) { bestsellingProductsContainer.innerHTML = bestselling.map(createProductCardHTML).join(''); }
            renderCategoryLinks();
            renderCustomerFeedback();
        }

        document.addEventListener('DOMContentLoaded', initializePage);

    </script>

</body>
</html>
