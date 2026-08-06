# 🔄 Blue-Green Deployment Model (AWS)

A zero-downtime deployment strategy implemented purely using **AWS services** — running two identical production environments (**Blue** and **Green**) for safe, risk-free application releases with instant rollback.

---

## 📌 What is Blue-Green Deployment?

Blue-Green Deployment is a release technique that reduces downtime and risk by running two identical environments:

- **Blue** → the current live (production) environment
- **Green** → the new version being deployed

Traffic is switched from Blue to Green only after the Green environment is fully tested and verified healthy. If any issue occurs, traffic can be instantly rolled back to Blue.

---

## ✨ Features

- ✅ Zero-downtime deployments
- ✅ Instant rollback in case of failure
- ✅ Safe testing of new releases in an isolated environment
- ✅ Automated traffic switching using AWS services
- ✅ Reduced deployment risk

---

## 🏗️ Architecture

```
                        ┌───────────────────┐
                        │  Route 53 (DNS)    │
                        └─────────┬──────────┘
                                  │
                        ┌─────────▼──────────┐
                        │  Elastic Load       │
                        │  Balancer (ALB)     │
                        └─────────┬──────────┘
                                  │
                 ┌────────────────┴────────────────┐
                 │                                  │
          ┌──────▼──────┐                   ┌───────▼──────┐
          │  BLUE (Live) │                   │  GREEN (New)  │
          │  EC2 / ASG   │                   │  EC2 / ASG    │
          │  v1.0        │                   │  v2.0         │
          └─────────────┘                   └───────────────┘
```

1. Blue environment (EC2 Auto Scaling Group) serves live production traffic.
2. New version is deployed to the Green environment (separate Target Group).
3. Green is tested internally using CloudWatch health checks.
4. Application Load Balancer's Target Group is switched from Blue → Green.
5. Blue is kept idle as a rollback option.

---

## 🛠️ AWS Services Used

| Service | Purpose |
|---|---|
| **EC2** | Hosts the application servers (Blue & Green environments) |
| **Application Load Balancer (ALB)** | Routes traffic between Blue and Green Target Groups |
| **Auto Scaling Group (ASG)** | Maintains desired number of healthy instances per environment |
| **Route 53** | DNS routing to the Load Balancer |
| **CloudWatch** | Monitors health, logs, and triggers alarms |
| **S3** | Stores application build artifacts / deployment packages |
| **IAM** | Manages secure access roles for EC2 and deployment scripts |
| **Elastic Beanstalk** *(optional)* | Native AWS support for Blue-Green deployments |

*(Remove/add rows depending on which services you actually used.)*

---

## 📂 Project Structure

```
blue-green-deployment/
├── blue-environment/
│   └── application files for current live version
├── green-environment/
│   └── application files for new version
├── scripts/
│   ├── deploy.sh              # deploys app to EC2 instances
│   ├── switch-traffic.sh      # updates ALB target group
│   └── health-check.sh        # checks instance health via CloudWatch
└── README.md
```

---

## ⚙️ Setup & Deployment (AWS Console / CLI)

### Prerequisites
- AWS Account with AWS CLI configured (`aws configure`)
- Two EC2 Auto Scaling Groups (Blue & Green)
- One Application Load Balancer with two Target Groups

### Steps

```bash
# 1. Clone the repository
git clone https://github.com/your-username/blue-green-deployment.git
cd blue-green-deployment

# 2. Deploy new version to the idle (Green) environment
./scripts/deploy.sh green

# 3. Run health checks on Green target group
./scripts/health-check.sh green

# 4. Switch ALB traffic from Blue to Green
./scripts/switch-traffic.sh green
```

Traffic switching is done by updating the **ALB Listener Rule** to forward requests to the Green Target Group instead of Blue — no downtime for users.

---

## 🔁 Rollback

If something goes wrong after switching to Green:

```bash
./scripts/switch-traffic.sh blue
```

This instantly updates the ALB Listener back to the Blue Target Group.

---

## 📊 Deployment Flow

1. New application version is deployed to idle environment (Green) EC2 instances
2. CloudWatch health checks confirm Green instances are healthy
3. ALB Target Group is updated to route traffic to Green
4. Blue environment kept on standby for instant rollback
5. Once Green is stable, Blue can be updated for the next release cycle

---

## 🚀 Future Improvements

- [ ] Automate environment switch using AWS CodeDeploy
- [ ] Add CloudWatch Alarms with SNS notifications on failed health checks
- [ ] Use Elastic Beanstalk's built-in Blue-Green deployment feature
- [ ] Multi-region setup using Route 53 failover routing

---

## 🤝 Contributing

Contributions are welcome! Please fork the repo and submit a pull request.

---

## 📄 License

This project is licensed under the MIT License.

---

## 👤 Author

**Your Name**
[GitHub](https://github.com/your-username) • [LinkedIn](https://linkedin.com/in/your-profile)
