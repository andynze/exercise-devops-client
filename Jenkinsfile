pipeline {
    agent any
    
    environment {
        AWS_DEFAULT_REGION = 'us-east-1'
    }
    
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

        stage('Build Frontend static website for S3') {
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
        stage("Upload static front end to s3") {
            steps {
                withCredentials([
                    [
                        $class: 'AmazonWebServicesCredentialsBinding',
                        credentialsId: 'awscred',
                        accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                        secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
                    ]
                ]) {
                    sh 'echo $AWS_ACCESS_KEY_ID'
                    sh 'echo $AWS_SECRET_ACCESS_KEY'
                    sh 'aws s3 ls s3://www.nzecruze.com'
                    sh 'aws s3 cp /var/lib/jenkins/workspace/Build-React-NodeJS/build/  s3://www.nzecruze.com --recursive'
                }
            }
        }
    }

    post {
        success {
            script {
                currentBuild.result = 'SUCCESS'
                sh ' echo Static files successfuly uploaded to S3'

            }
        }
    }
}
