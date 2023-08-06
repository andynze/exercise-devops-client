pipeline {
    agent any
    
    tools {nodejs "node"}
  
    stages {
        
        stage('Cleaning') {
            steps {
                // Clean previous build
                sh 'sudo rm -rf /var/lib/jenkins/workspace/Beezwax/*'
            }
        }  
        stage('Clone Frontend Repository') {
            steps {
                // Clone the frontend repository
                git branch: 'master', url: 'https://github.com/andynze1/exercise-devops-client.git'
            }
        }

        stage('Build Frontend') {
            steps {
                // Set up Node.js environment

                // Install dependencies and build the frontend
                sh "npm ci"
                sh "cd /home/ubuntu/exercise-devops-client"
                sh "npm install react"
                sh "npm install"
                sh "npm install run-s"
                sh "pwd"
                sh "sudo npm run build"
            }
        }

        stage('Clone Backend Repository') {
            steps {
                // Clone the backend repository
                git branch: 'master', url: 'https://github.com/andynze1/exercise-devops-server.git'
            }
        }

        stage('Build Backend') {
            steps {
                // Install dependencies and build the backend
                sh "cd /home/ubuntu/exercise-devops-server"
                sh "npm install run-s"
                sh "npm install"
                sh "npm install pm2"
            }
        }

        stage('Test') {
            steps {
                // Run tests for frontend and backend (modify commands if needed)
                sh "cd /home/ubuntu/exercise-devops-client && npm test"
                sh "cd /home/ubuntu/exercise-devops-server && npm test"
            }
        }

    }

    post {
        success {
            script {
                currentBuild.result = 'SUCCESS'
                sh "cd /home/ubuntu/exercise-devops-server"
                sh "pm2 start index.js"
            }
        }
    }
}
