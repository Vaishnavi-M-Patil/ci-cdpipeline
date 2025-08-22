# DevOps Engineer - Junior Role Assignment
### Objective:
Set up a complete CI/CD workflow and deployment pipeline for a Node.js application with secure
access and monitoring.
#### 1. Clone and Validate Application
- Clone code from github repository
```
git clone https://github.com/Vaishnavi-M-Patil/node-js-sample.git
```
- Validate it using `npm install` and `npm start`
![validate](https://github.com/Vaishnavi-M-Patil/ci-cdpipeline/blob/main/screenshots/1.png)
![app](https://github.com/Vaishnavi-M-Patil/ci-cdpipeline/blob/main/screenshots/2.png)

#### 2. CI/CD Pipeline with Jenkins
- Install Jenkins on a Linux server:
```
sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
```
```
echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
```
```
sudo apt-get update
sudo apt-get install jenkins -y
```
```
sudo apt update
sudo apt install fontconfig openjdk-21-jre -y
sudo apt install openjdk-11-jdk -y
java -version
```
```
#  enable the Jenkins service 
sudo systemctl enable jenkins
# start the Jenkins service 
sudo systemctl start jenkins
# status of the Jenkins service
sudo systemctl status jenkins
```
![jenkins](https://github.com/Vaishnavi-M-Patil/ci-cdpipeline/blob/main/screenshots/3.png)

- Pulls code from the Git repository
```
stage('pull'){
        steps{
            git branch: 'main', url: 'https://github.com/Vaishnavi-M-Patil/node-js-sample.git'
            echo "pull successful"
        }
    }
```

- Installs dependencies
```
stage('Install Dependencies') {
      steps {
        sh 'npm install'
      }
    }
```

- Runs tests
```
stage('Test') {
      steps {
        sh 'npm test || true'
      }
    }
```
- Builds and deploys the application
```
stage('Build') {
      steps {
        sh 'npm run build || true-'
      }
    }
    stage('Deploy') {
      steps {
        sh '''
          pm2 stop app || true
          pm2 start app.js --name node-js-sample --update-env
          sudo systemctl reload nginx
        '''
      }
    }
```


#### 3. Implement Matrix-Based Security in Jenkins
- Enabled Matrix-based security
- Created roles with appropriate permissions (e.g., admin, developer)
- Restricted anonymous access

![matrix-security](https://github.com/Vaishnavi-M-Patil/ci-cdpipeline/blob/main/screenshots/4.png)

#### 4. Deploy Using NGINX with SSL
- Set Up Reverse Proxy and SSL
```
 server {
    listen 80;
    server_name devlogin.nextastra.com;

    location / {
        proxy_pass http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

- Install and Configure Encrypt SSL
```
sudo apt install certbot python3-certbot-nginx -y
sudo certbot certonly --manual --preferred-challenges dns -d devlogin.nextastra.com
```
![dns](https://github.com/Vaishnavi-M-Patil/ci-cdpipeline/blob/main/screenshots/5.png)

![ssl](https://github.com/Vaishnavi-M-Patil/ci-cdpipeline/blob/main/screenshots/6.png)

