# aws_web_deplyment
# 🎓 Hosting Student Management System in AWS

## 🚀 Project Overview

A cloud-based **Student Management System (SMS)** designed for educational institutions to manage student and course data efficiently using AWS services such as EC2, S3, SNS, CloudWatch, IAM, and VPC. The project ensures scalability, automation, and real-time access.

---

## 🧩 Problem Identification

Traditional on-premise student management systems face these issues:
- Manual maintenance and hardware setup.
- Lack of scalability during admissions or exams.
- No real-time access for remote users.
- Limited monitoring and automation.

Hence, this project uses AWS to build a **scalable, secure, and monitored cloud-based system**.

---

## ☁️ AWS Services Used

| AWS Service | Purpose |
|--------------|----------|
| **Amazon EC2** | Hosts the backend Node.js server (APIs). |
| **Amazon S3** | Hosts the static frontend website (HTML, CSS, JS). |
| **Amazon SNS** | Sends automated email alerts when new data (like a course) is added. |
| **Amazon CloudWatch** | Monitors logs and performance metrics from the EC2 backend. |
| **Amazon IAM** | Manages permissions and user access securely. |
| **Amazon VPC** | Provides a secure and isolated network for the EC2 instance. |
| **MongoDB Atlas (External)** | Stores student and course data, integrated with the backend. |

---

## 🔁 Workflow Diagram

![Workflow Diagram](A_flowchart_diagram_illustrates_a_web_application_.png)

### Explanation
1. User accesses the **frontend** hosted on **Amazon S3**.
2. Frontend sends API requests to the **backend** running on **EC2**.
3. EC2 connects to **MongoDB Atlas** to store and retrieve data.
4. Upon a new course or student addition, **SNS** sends an email notification.
5. **CloudWatch** logs all events and monitors resource usage.
6. **IAM** ensures secure access to AWS resources.

---

## ⚙️ Application Development

### 🧠 Tech Stack
- **Frontend**: HTML, CSS, JavaScript  
- **Backend**: Node.js, Express.js  
- **Database**: MongoDB Atlas  
- **Hosting**: AWS EC2 (backend), AWS S3 (frontend)  
- **Notification**: AWS SNS (email alerts)

### 📦 Backend Folder Structure
```
backend/
├── server.js
├── package.json
├── routes/
├── models/
└── .env
```

---

## 🖥️ EC2 Backend Deployment Steps

```bash
# 1️⃣ Connect to EC2 instance
ssh -i "your-key.pem" ec2-user@your-public-ip

# 2️⃣ Install Node.js, npm, and Git
sudo yum update -y
curl -fsSL https://rpm.nodesource.com/setup_18.x | sudo bash -
sudo yum install -y nodejs git

# 3️⃣ Clone your repository
git clone https://github.com/yourusername/student-management-system.git
cd student-management-system/backend

# 4️⃣ Install dependencies
npm install

# 5️⃣ Configure environment variables
nano .env
# Example content:
# MONGO_URI=your_mongo_uri
# SNS_TOPIC_ARN=arn:aws:sns:us-east-1:xxxxxxxxxx:course-alerts

# 6️⃣ Start the backend server using PM2
sudo npm install -g pm2
pm2 start server.js
pm2 save
pm2 startup

# 7️⃣ Verify it's running
pm2 list
curl http://localhost:3000/api/courses
```

---

## 🌐 S3 Frontend Hosting Setup

```bash
# 1️⃣ Create an S3 bucket (must be unique globally)
aws s3 mb s3://student-management-frontend

# 2️⃣ Enable static website hosting
aws s3 website s3://student-management-frontend/ --index-document index.html --error-document error.html

# 3️⃣ Upload frontend files
aws s3 cp ./frontend s3://student-management-frontend/ --recursive

# 4️⃣ Set public read access policy
# (Replace 'student-management-frontend' with your actual bucket name)
cat > policy.json <<EOL
{
  "Version": "2012-10-17",
  "Statement": [{
    "Sid": "PublicReadGetObject",
    "Effect": "Allow",
    "Principal": "*",
    "Action": "s3:GetObject",
    "Resource": "arn:aws:s3:::student-management-frontend/*"
  }]
}
EOL

aws s3api put-bucket-policy --bucket student-management-frontend --policy file://policy.json

# 5️⃣ Get your website URL
# Example:
# http://student-management-frontend.s3-website-us-east-1.amazonaws.com
```

---

## 📧 SNS (Simple Notification Service) Setup

1. Open **AWS SNS Console** → Create **Topic** → Name it `course-alerts`  
2. Copy the **Topic ARN** (e.g., `arn:aws:sns:us-east-1:xxxxxxx:course-alerts`)  
3. Add your email as a **subscription** and confirm it via email link.  
4. Add this ARN inside your `.env` file.  
5. In backend code, publish messages to SNS whenever a new course or student is added.

---

## 📊 CloudWatch Integration

1. Open **AWS CloudWatch → Logs → Log Groups → /pm2/NodeAppLogs**
2. Enable automatic log streaming from EC2 (via PM2 configuration)
3. View real-time logs and set up alerts for errors or downtime.

---

## 🔒 IAM Role Setup

- Create a new IAM Role for EC2 with:
  - `AmazonSNSFullAccess`
  - `CloudWatchAgentServerPolicy`
- Attach this role to your EC2 instance to enable SNS and CloudWatch integration securely.

---

## 📸 Sample Screenshots

| Section | Screenshot Description |
|----------|------------------------|
| EC2 Instance | Server running via PM2 |
| S3 Static Website | Student Management UI hosted on S3 |
| SNS Email | Notification received when new course added |
| CloudWatch Logs | Live logs from Node.js backend |

---

## 🏁 Conclusion

This project successfully demonstrates hosting a **Student Management System** using AWS with complete scalability, monitoring, and automation via:
- **EC2 (Backend)**  
- **S3 (Frontend)**  
- **SNS (Notifications)**  
- **CloudWatch (Monitoring)**  
- **IAM + VPC (Security)**  

---

🧑‍💻 **Author:** Hari Sankaran  
📅 **Date:** November 2025  
🏫 **Institution:** VIT Vellore  
