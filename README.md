# Web Server Landing Page

This repository documents the steps for provisioning an EC2 instance, setting up a web server, deploying an HTML landing page, and configuring networking, including setting up SSL for secure access.

## Provisioning the Server
- **Cloud Provider**: AWS EC2
- **Linux Distribution**:  Ubuntu 24.04.1 LTS (GNU/Linux 6.8.0-1018-aws x86_64)
- **Instance Type**: t2.micro (for the free tier)
- **Steps**:
  1. Created a new EC2 instance from the AWS Management Console.
  2. Chose the **Ubuntu Server 24.04.1 LTS** AMI.
  3. Configured the instance with a **Security Group** that allows HTTP (port 80), HTTPS (port 443), and SSH (port 22) traffic.
  4. Created a new SSH key pair (`webkeypair.pem`) for accessing the instance.

## Web Server Setup
- **Web Server**: Apache
- **Steps**:
  1. SSH into the EC2 instance using the SSH key:
     ```bash
     ssh -i webkeypair.pem ubuntu@ec2-52-56-131-120.eu-west-2.compute.amazonaws.com

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
    <!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Welcome to David Obinta Landing Page</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            margin: 0;
            padding: 0;
            background-color: #f4f4f9;
        }
        header {
            background-color: #6200ea;
            color: white;
            padding: 20px;
        }
        main {
            padding: 20px;
        }
        footer {
            margin-top: 20px;
            font-size: 14px;
            color: #555;
        }
    </style>
</head>
<body>
    <header>
        <h1>Welcome to David Obinta's Landing Page</h1>
    </header>
    <main>
        <p>Hi, I'm David Obinta, AltSchool ID: ALT/SOE/024/0930 and this is my DevOps project!</p>
        <h2>Project Title: Landing Page Demo</h2>
        <p>This page demonstrates a simple web server setup using Linux.</p>
        <h2>About Me</h2>
        <p>
            I am an AWS Certified Cloud Practitioner and Solutions Architect with a strong foundation in software development, DevOps, and video editing.
            <br>Proficient in designing and implementing scalable cloud solutions using AWS services, deploying containerized applications, and managing CI/CD pipelines.
            <br>Driven by a passion for learning, I earned my certifications in November and December, reflecting my commitment to advancing in cloud engineering and DevOps.
        </p>
        <p>
            Respect to AltSchool Africa, where I first encountered the term "Cloud Engineering."
            Initially enrolling to study "Backend Engineering," my time at AltSchool sparked a deeper interest in cloud technologies, inspiring me to pivot and pursue this exciting path.
        </p>
        <p>
            Now, I seek to leverage my skills and certifications to contribute to innovative cloud engineering and DevOps projects.
        </p>
        <p>Fun fact: I have a nickname: Izudavo- The "Izu" comes from Izuchukwu(middle name), while "dav" is from David(first name), and the "O" stands for Obinta(Surname)
            <br>Outside of Tech, I'm a Video Editor, driven to create creative visuals for users entertainment.
        <p>Also i'm currently a 300level student studying Biomedical Technology at Federal University of Technology, Akure.(FUTA)
        </p>
    </main>
    <footer>
        <p>&copy; 2024 David Obinta. All rights reserved.</p>
    </footer>
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
- **Public IP**: `52.56.131.120`
- The landing page is accessible from any browser via the public IP address.
- Example: `http://52.56.131.120`

## Screenshot
- The screenshot below shows the landing page running in the browser:
  

## SSL Configuration: Self-Signed SSL Certificate
 Configured a **self-signed SSL certificate** for HTTPS access:
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

- **Public IP**: `52.56.131.120`
- Example: `https://52.56.131.120`

Now, the landing page is accessible via HTTPS, though it may show a browser warning due to the self-signed certificate.


---

### **Additional Notes**
- This repository includes the necessary steps to provision an EC2 instance, set up Apache, deploy a landing page, and ensure networking is properly configured for public access.
- SSL was configured using a self-signed certificate due to issues with Let's Encrypt, but for production environments, a certificate from a trusted authority will be used.

---

