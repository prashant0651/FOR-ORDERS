<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Neonbyte Web Hosting Company Ltd.</title>
    <link rel="stylesheet" href="styles.css">
    <script defer src="script.js"></script>
</head>
<body>
    <header>
        <img src="Screenshot 2025-02-14 050038.png" alt="Neonbyte Logo" class="logo">
        <h1>Neonbyte Web Hosting Company Ltd.</h1>
        <img src="IMG_20241222_204244.jpg" alt="Prashant Kumar Singh" class="profile-pic">
        <p>Director & Co-founder: Prashant Kumar Singh</p>
    </header>
    
    <nav>
        <ul>
            <li><a href="#web-development">Web Development</a></li>
            <li><a href="#web-hosting">Web Hosting</a></li>
            <li><a href="#thumbnails">Thumbnails</a></li>
            <li><a href="#logos">Logos</a></li>
            <li><a href="#custom-order">Custom Order</a></li>
            <li><a href="#contact">Contact</a></li>
        </ul>
    </nav>
    
    <section id="web-development">
        <h2>Web Development</h2>
        <button onclick="placeOrder('Web Development')">Order Now</button>
    </section>
    
    <section id="web-hosting">
        <h2>Web Hosting</h2>
        <button onclick="placeOrder('Web Hosting')">Order Now</button>
    </section>
    
    <section id="thumbnails">
        <h2>Thumbnails</h2>
        <button onclick="placeOrder('Thumbnails')">Order Now</button>
    </section>
    
    <section id="logos">
        <h2>Logos</h2>
        <button onclick="placeOrder('Logos')">Order Now</button>
    </section>
    
    <section id="custom-order">
        <h2>Custom Order</h2>
        <form id="customOrderForm">
            <textarea placeholder="Describe your custom idea..."></textarea>
            <button type="submit">Submit</button>
        </form>
    </section>
    
    <section id="contact">
        <h2>Contact Us</h2>
        <p><img src="phone-icon.png" alt="Phone" class="icon"> +91 7067185187</p>
        <p><img src="email-icon.png" alt="Email" class="icon"> <a href="mailto:prashantkumarsingh065@gmail.com">prashantkumarsingh065@gmail.com</a></p>
        <p><img src="instagram-icon.png" alt="Instagram" class="icon"> <a href="https://www.instagram.com/o_o.prashantkumarsingh.o_o?igsh=em9tN3dlMGhwcDJ1" target="_blank">Instagram Profile</a></p>
        <p><img src="facebook-icon.png" alt="Facebook" class="icon"> <a href="https://facebook.com/share/15d7hCXisD/" target="_blank">Facebook Profile</a></p>
    </section>
    
    <script>
        function generateOrderID() {
            return 'NB-' + Math.floor(100000 + Math.random() * 900000);
        }

        function placeOrder(service) {
            let customerEmail = prompt("Enter your email for order confirmation:");
            if (!customerEmail) {
                alert("Order canceled. Email is required.");
                return;
            }

            let orderID = generateOrderID();
            let orderDate = new Date().toLocaleString();
            let qrData = `Order ID: ${orderID}\nService: ${service}\nEmail: ${customerEmail}\nDate: ${orderDate}`;
            let qrCodeURL = `https://chart.googleapis.com/chart?chs=150x150&cht=qr&chl=${encodeURIComponent(qrData)}`;

            let receiptContent = `
                <html>
                <head>
                    <title>Order Receipt</title>
                    <style>
                        body { font-family: Arial, sans-serif; padding: 20px; text-align: center; }
                        .receipt { border: 2px solid #000; padding: 20px; width: 300px; margin: auto; }
                        h2 { color: #007BFF; }
                        p { font-size: 16px; }
                        img { margin-top: 10px; }
                    </style>
                </head>
                <body>
                    <div class="receipt">
                        <h2>Neonbyte Web Hosting Company Ltd.</h2>
                        <p><strong>Order ID:</strong> ${orderID}</p>
                        <p><strong>Service Ordered:</strong> ${service}</p>
                        <p><strong>Email:</strong> ${customerEmail}</p>
                        <p><strong>Date:</strong> ${orderDate}</p>
                        <img src="${qrCodeURL}" alt="QR Code">
                        <p>Thank you for your order!</p>
                    </div>
                </body>
                </html>
            `;

            let receiptWindow = window.open("", "_blank");
            receiptWindow.document.write(receiptContent);
            receiptWindow.document.close();
            receiptWindow.print();

            sendOrderEmail(service, customerEmail, orderID);
        }

        function sendOrderEmail(service, customerEmail, orderID) {
            let formData = new FormData();
            formData.append("service", service);
            formData.append("email", customerEmail);
            formData.append("orderID", orderID);

            fetch("send_mail.php", {
                method: "POST",
                body: formData
            })
            .then(response => response.text())
            .then(data => {
                if (data === "success") {
                    console.log("Order email sent successfully.");
                } else {
                    console.log("Error sending order email.");
                }
            })
            .catch(error => console.error("Error:", error));
        }
    </script>
</body>
</html>
