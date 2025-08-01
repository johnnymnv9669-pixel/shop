<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>ร้านค้าออนไลน์</title>
  <style>
    body {
      background: #f5f5f5;
      font-family: Arial, sans-serif;
      padding: 20px;
      color: #333;
    }

    h1 {
      text-align: center;
    }

    .horizontal-scroll {
      display: flex;
      overflow-x: auto;
      gap: 20px;
      padding: 10px 0;
    }

    .product {
      flex: 0 0 300px;
      background: white;
      border-radius: 10px;
      box-shadow: 0 0 5px rgba(0,0,0,0.1);
      padding: 15px;
    }

    .product img {
      width: 100%;
      height: 200px;
      object-fit: cover;
      border-radius: 5px;
    }

    .product h3 {
      margin: 10px 0 5px;
    }

    .btn {
      background: #ff5722;
      color: white;
      padding: 8px 12px;
      border: none;
      cursor: pointer;
      border-radius: 5px;
      display: block;
      margin-top: 10px;
    }

    .order-form {
      display: none;
      margin-top: 10px;
    }

    .order-form input,
    .order-form textarea {
      width: 100%;
      margin-top: 5px;
      padding: 8px;
      border-radius: 5px;
      border: 1px solid #ccc;
    }
  </style>
</head>
<body>
  <h1>ร้านค้าออนไลน์</h1>
  <div class="horizontal-scroll" id="productScroll"></div>

  <script>
    const scriptURL = 'https://script.google.com/macros/s/AKfycbwlXpuYEHI_s9_3iFAHtfOeDFQQi3ji8WEvejjEVJeXo-8t07GSrasiiY3FF4HjAzXa/exec';

    fetch(scriptURL)
      .then(res => res.json())
      .then(data => {
        const scroll = document.getElementById('productScroll');
        data.forEach((item, index) => {
          const product = document.createElement('div');
          product.className = 'product';

          product.innerHTML = `
            <img src="${item.pictures}" alt="${item.Name}">
            <h3>${item.Name}</h3>
            <p>${item.Description}</p>
            <strong>${item.price} ກີບ</strong>
            <button class="btn" onclick="toggleForm(${index})">สั่งซื้อ</button>

            <form class="order-form" id="form-${index}" onsubmit="submitOrder(event, '${item.Name}', '${item.price}')">
              <input type="text" name="name" placeholder="ชื่อผู้รับ" required>
              <input type="text" name="phone" placeholder="เบอร์โทร" required>
              <textarea name="address" placeholder="ที่อยู่จัดส่ง" required></textarea>
              <input type="url" name="imageURL" placeholder="แนบลิงก์รูปสลิป (ถ้ามี)">
              <button class="btn">ยืนยันคำสั่งซื้อ</button>
            </form>
          `;
          scroll.appendChild(product);
        });
      });

    function toggleForm(index) {
      const form = document.getElementById(form-${index});
      form.style.display = form.style.display === 'block' ? 'none' : 'block';
    }

    function submitOrder(event, product, price) {
      event.preventDefault();
      const form = event.target;

      const data = {
        product,
        price,
        name: form.name.value,
        phone: form.phone.value,
        address: form.address.value,
        imageURL: form.imageURL.value
      };

      fetch(scriptURL, {
        method: 'POST',
        body: JSON.stringify(data),
        headers: { 'Content-Type': 'application/json' }
      })
      .then(res => res.json())
      .then(res => {
        if (res.status === 'success') {
          alert('สั่งซื้อเรียบร้อยแล้ว!');
          form.reset();
          form.style.display = 'none';
        } else {
          alert('เกิดข้อผิดพลาดในการสั่งซื้อ');
        }
      })
      .catch(err => {
        alert('เชื่อมต่อไม่สำเร็จ');
        console.error(err);
      });
    }
  </script>
</body>
</html>
