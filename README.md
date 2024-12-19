Here's the updated version of your document with the recent progress and the SSL configuration:

---

# Web Server Landing Page

This repository documents the steps for provisioning an EC2 instance, setting up a web server, deploying an HTML landing page, and configuring networking, including setting up SSL for secure access.

## Provisioning the Server
- **Cloud Provider**: AWS EC2
- **Linux Distribution**: Ubuntu 22.04 LTS
- **Instance Type**: t2.micro (for the free tier)
- **Steps**:
  1. Created a new EC2 instance from the AWS Management Console.
  2. Chose the **Ubuntu Server 22.04 LTS** AMI.
  3. Configured the instance with a **Security Group** that allows HTTP (port 80), HTTPS (port 443), and SSH (port 22) traffic.
  4. Created a new SSH key pair (`<YourKeyName>.pem`) for accessing the instance.

## Web Server Setup
- **Web Server**: Apache
- **Steps**:
  1. SSH into the EC2 instance using the SSH key:
     ```bash
     ssh -i <YourKeyName>.pem ubuntu@<Your-EC2-Public-IP>
     ```
  2. Update the package list:
     ```bash
     sudo apt update
     ```
  3. Install Apache:
     ```bash
     sudo apt install apache2 -y
     ```
  4. Start the Apache service:
     ```bash
     sudo systemctl start apache2
     ```
  5. Enable Apache to start on boot:
     ```bash
     sudo systemctl enable apache2
     ```

## HTML Page Deployment
- **HTML Page**: A simple landing page showcasing your project
- **Steps**:
  1. Create an `index.html` file in the Apache web server directory (`/var/www/html/`):
     ```bash
     sudo nano /var/www/html/index.html
     ```
  2. Add your custom HTML content:
     ```html
     <html>
       <head><title>Welcome to [Your Name] Landing Page</title></head>
       <body>
         <h1>Welcome to [Your Name] Landing Page</h1>
         <p>This is a simple HTML page showcasing my project.</p>
       </body>
     </html>
     ```
  3. Save the file and exit the editor (`Ctrl + O`, `Enter`, `Ctrl + X`).
  4. Set the correct file permissions:
     ```bash
     sudo chmod 644 /var/www/html/index.html
     sudo chown www-data:www-data /var/www/html/index.html
     ```
  5. Restart Apache to ensure the changes take effect:
     ```bash
     sudo systemctl restart apache2
     ```

## Networking
- **Public IP**: `<Your-EC2-Public-IP>`
- The landing page is accessible from any browser via the public IP address.
- Example: `http://<Your-EC2-Public-IP>`

## Screenshot
- The screenshot below shows the landing page running in the browser:
  ![Landing Page Screenshot](screenshot.png)  <!-- Replace with the actual screenshot file name -->

## SSL Configuration: Self-Signed SSL Certificate
Since Let's Encrypt was not available, we configured a **self-signed SSL certificate** for HTTPS access:
- **Steps**:
  1. Generate a self-signed certificate:
     ```bash
     sudo openssl req -x509 -newkey rsa:4096 -keyout /etc/ssl/private/webkey.key -out /etc/ssl/certs/webkey.crt -days 365 -nodes
     ```
  2. Edit the Apache SSL configuration to point to the newly generated certificate:
     ```bash
     sudo nano /etc/apache2/sites-available/default-ssl.conf
     ```
  3. Modify the following lines:
     ```bash
     SSLCertificateFile      /etc/ssl/certs/webkey.crt
     SSLCertificateKeyFile   /etc/ssl/private/webkey.key
     ```
  4. Enable the SSL module and the SSL site:
     ```bash
     sudo a2enmod ssl
     sudo a2ensite default-ssl.conf
     ```
  5. Restart Apache to apply the changes:
     ```bash
     sudo systemctl restart apache2
     ```

Now, the landing page is accessible via HTTPS, though it may show a browser warning due to the self-signed certificate.

---

### **Additional Notes**
- This repository includes the necessary steps to provision an EC2 instance, set up Apache, deploy a landing page, and ensure networking is properly configured for public access.
- SSL was configured using a self-signed certificate due to issues with Let's Encrypt, but for production environments, a certificate from a trusted authority should be used.

---

Feel free to add the actual screenshot and any additional details as needed. Let me know if you need further adjustments!
