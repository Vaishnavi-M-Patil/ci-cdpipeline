pipeline {
  agent { label 'nginx-server' }
}

  stages {
    stage('Checkout Jenkinsfile Repo') {
      steps {
        checkout scm  
      }
    }
    stage('pull'){
        steps{
            git branch: 'main', url: 'https://github.com/Vaishnavi-M-Patil/node-js-sample.git'
            echo "pull successful"
        }
    }
    stage('Install Dependencies') {
      steps {
        sh 'npm install'
      }
    }
    stage('Test') {
      steps {
        sh 'npm test || true'
      }
    }
    stage('Build') {
      steps {
        sh 'npm run build || true'
      }
    }
    stage('Deploy') {
      steps {
        sh '''
          pm2 stop app || true
          pm2 start app.js --name node-app --update-env
          sudo systemctl reload nginx
        '''
      }
    }
  }
}