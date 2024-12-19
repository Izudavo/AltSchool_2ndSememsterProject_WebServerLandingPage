# Web Server Landing Page

This repository documents the steps for provisioning an EC2 instance, setting up a web server, deploying an HTML landing page, and configuring networking.

## Provisioning the Server
- **Cloud Provider**: AWS EC2
- **Linux Distribution**: Ubuntu 22.04 LTS
- **Instance Type**: t2.micro (for the free tier)
- **Steps**:
  1. Created a new EC2 instance from the AWS Management Console.
  2. Chose the **Ubuntu Server 22.04 LTS** AMI.
  3. Configured the instance with a **Security Group** that allows HTTP (port 80) traffic and SSH (port 22).
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

## Bonus Task (Optional): Configure HTTPS with Let's Encrypt
- **Steps**:
  1. Install Certbot for Apache:
     ```bash
     sudo apt install certbot python3-certbot-apache -y
     ```
  2. Run Certbot to automatically configure SSL for Apache:
     ```bash
     sudo certbot --apache
     ```
  3. Follow the interactive prompts to choose your domain name and agree to the terms.
  4. Once completed, your site will be available via HTTPS, and Certbot will automatically renew the certificate.

---

### **Additional Notes**
- This repository includes the necessary steps to provision an EC2 instance, set up Apache, deploy a landing page, and ensure networking is properly configured for public access.
