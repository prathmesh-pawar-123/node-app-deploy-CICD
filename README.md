# node-js-app-CICD



---


🚀 Node.js App Deployment using Jenkins CI/CD


This project shows how to deploy a Node.js application automatically using Jenkins CI/CD pipeline connected with GitHub and hosted on AWS EC2.


---

🧩 1. Launch EC2 Instance

Go to AWS Console → EC2 → Launch Instance

Choose Ubuntu Server (or Amazon Linux)

Select instance type (e.g., t2.micro)

Create a key pair and download it

Allow inbound ports:

8080 for Jenkins

3000 for Node.js app


Connect to EC2 using SSH:

ssh -i your-key.pem ubuntu@your-ec2-public-ip

![] (./img/![alt text](<img/screenshot (100).png>))

---

⚙ 2. Create Repository on GitHub

Go to GitHub

Click New Repository → name it node-app-cicd

Don’t add any files yet (we will push code from EC2).

i[] (./img/![alt text](<img/screenshot (101).png>))

---

🔗 3. Add Webhook (GitHub → Jenkins)

Open your GitHub repo → Settings → Webhooks → Add webhook

In Payload URL, paste your Jenkins URL:

http://<your-jenkins-ip>:8080/github-webhook/

Choose application/json as content type.

Select Just the push event → Save webhook.


✅ Now, whenever you push code, Jenkins will automatically get notified.

i[] (./img/![alt text](<img/Screenshot (102).png>))

---

🔑 4. Add Credentials in Jenkins

Go to Manage Jenkins → Credentials → Global → Add Credentials

Choose type → Username and Password (for GitHub)

Enter:

Username: your GitHub username

Password: your GitHub personal access token


Click Save.

i[] (./img/![alt text](<img/Screenshot (103).png>))


---

🏗 5. Create New Item in Jenkins

Open Jenkins Dashboard → Click New Item

Enter project name: node-app-cicd

Select Pipeline → Click OK

Under Pipeline → Definition, choose Pipeline script from SCM

Select Git and paste your GitHub repo link

Choose your credentials (from step 4)

Click Save.

i[] (.![alt text](<img/Screenshot (103).png>)/img/)
i[] (./img/![alt text](<img/Screenshot (104).png>))

i[] (./img/![alt text](<img/Screenshot (105).png>)) 



---

🧾 6. Write Jenkinsfile

Create a file named Jenkinsfile in your project folder:

pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/your-username/node-app-cicd.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build and Test') {
            steps {
                echo 'Building the Node app...'
                sh 'npm test || echo "No tests found"'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Starting the application...'
                sh 'npm start &'
            }
        }
    }

    post {
        success {
            echo '✅ Deployment Successful!'
        }
        failure {
            echo '❌ Deployment Failed!'
        }
    }
}


---

🧠 7. Push Code and Jenkinsfile to Repository

In your EC2 terminal, run:

git init
git add .
git commit -m "Initial Node app and Jenkinsfile"
git branch -M main
git remote add origin https://github.com/prathmesh-pawar-123/node-app-cicd.git
git push -u origin main

✅ Jenkins will automatically trigger the build after this push.


---

🌐 8. Browse Application on Browser

Once Jenkins shows “Build Successful”, open:

http://<your-ec2-public-ip>:3000

You should see:

> Node App Deployed using Jenkins CI/CD 🚀

i[] (./img/![alt text](<img/Screenshot (106).png>))




---

🎯 9. Conclusion

✅ You have successfully:

Launched an EC2 instance

Created a GitHub repo and linked it with Jenkins

Built and deployed a Node.js app automatically using a Jenkins CI/CD pipeline


🎉 Your Node app is now live with Jenkins automation!


---

