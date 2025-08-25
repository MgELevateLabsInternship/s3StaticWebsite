# 🌍 Scalable Static Website with S3 + Cloudflare + GitHub Actions

### 🔹 1. Create Static Website

1. Make a folder s3StaticWebsite/ with index.html, style.css, etc
2. Initialize GitHub repo and push code.
```
git init
git remote add origin https://github.com/<username>/<repo>.git
git add .
git commit -m "Initial static site"
git push origin main
```

### 🔹 2. Create AWS S3 Bucket

1. Go to AWS S3 → Create Bucket
2. Name: mgs3staticwebsit (it needs to be unique)
3. Region: ap-southeat-1 (your choice)
4. Uncheck "Block all public access"
5. Generate bucket policy
6. Add bucket policy
7. Enable static website hosting under Properties → Static Website Hosting.
8. Upload sample index.html and note Bucket Website Endpoint (e.g. http://mgs3staticwebsit.s3-website-ap-southeast-1.amazonaws.com/).

### 🔹 3. GitHub Actions CI/CD Workflow

1. In your repo, create .github/workflows/deploy.yml

```
name: Deploy Static Website to S3

on:
  push:
    branches: [ "main" ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@v2
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: us-east-1

      - name: Sync files to S3
        run: aws s3 sync . s3://mgs3staticwebsit --delete


```
🔑 Store your AWS credentials in GitHub → Settings → Secrets → Actions.

### 🔹 4. Cloudflare Integration

1. Add your domain in Cloudflare Dashboard
2. Update your Domain Registrar nameservers → point to Cloudflare.
3. Create a CNAME Record in Cloudflare:
www.example.com → [my-static-site.s3-website-us-east-1.amazonaws.com](http://mgs3staticwebsit.s3-website-ap-southeast-1.amazonaws.com/]

4. Proxy status: Proxied (orange cloud)
5. In Cloudflare → SSL/TLS → set Full (Strict).
6. Enable Caching & Page Rules:
7. Cache Level → Cache Everything
8. Edge Cache TTL → 1 day

### 🔹 5. Enable Versioning in S3

- #### Go to S3 → Bucket → Properties → Enable Versioning.
- This helps rollback deployments if needed.

### 🔹 6. Deployment Flow

1. Push code to GitHub → GitHub Actions runs → Deploys to S3.
2. Cloudflare caches globally & serves via HTTPS.


### ✅ Deliverables

- Live URL: http://mgs3staticwebsit.s3-website-ap-southeast-1.amazonaws.com/ (not via Cloudflare HTTPS because domain is paid feature)
- [Scalable_Static_Website_Report.pdf](Scalable_Static_Website_Report.pdf)

### Screenshot Examples

1. GitHub Actions successful run: <img width="1920" height="863" alt="Workflow runs · MgELevateLabsInternship_s3StaticWebsite - Brave 25-08-2025 16_09_06" src="https://github.com/user-attachments/assets/28dd2c69-db8d-45a1-8dcd-60d4a6957ceb" />


2. S3 bucket hosting enabled: <img width="1920" height="792" alt="s3StaticWebsite and 1 more tab - File Explorer 25-08-2025 16_10_15" src="https://github.com/user-attachments/assets/bc16b81a-5396-4a7f-8911-c02e2efd697f" />


3. Live website in browser: <img width="1920" height="972" alt="Photos 25-08-2025 16_10_53" src="https://github.com/user-attachments/assets/30319c88-35b9-434a-8a87-9de104232637" />
