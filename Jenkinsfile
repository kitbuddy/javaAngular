pipeline {
    agent any

    environment {
        NODE_ENV = "production"
        JAVA_HOME = "/usr/lib/jvm/java-17-openjdk-amd64"
    }

    tools {
        nodejs "Node 18"
    }

    stages {

        stage('Show Frontend Directory') {
            steps {
                dir('client/frontend') {
                    sh 'pwd'
                    sh 'ls -la'
                    sh 'cat package.json'
                }
            }
        }

        stage('Checkout Code') {
            steps {
                git branch: 'develop',
                    url: 'https://github.com/kitbuddy/javaAngular.git',
                    credentialsId: 'github-pat-credentials'
            }
        }

        stage('Install Angular Dependencies') {
            steps {
                dir('client/frontend') {
                    echo "Installing Angular dependencies..."
                    sh 'npm install --legacy-peer-deps --include=dev'
                }
            }
        }

        stage('Build Angular Frontend') {
            steps {
                dir('client/frontend') {
                    echo "Building Angular frontend..."
                    sh './node_modules/.bin/ng build --configuration production'
                }
            }
        }

        stage('Deploy to EC2') {
            steps {
                sh '''
                echo "Deploying Angular build to EC2..."
                scp -i /Users/ankitjain/Desktop/javaAngularAWS/javaAngularAgain.pem -o StrictHostKeyChecking=no -r client/frontend/dist/frontend/* ec2-user@ec2-3-133-115-171.us-east-2.compute.amazonaws.com:/home/ec2-user/angular-app/

                ssh -i /Users/ankitjain/Desktop/javaAngularAWS/javaAngularAgain.pem -o StrictHostKeyChecking=no ec2-user@ec2-3-133-115-171.us-east-2.compute.amazonaws.com "
                    sudo mv /home/ec2-user/angular-app/* /var/www/html/ &&
                    sudo systemctl restart nginx
                "
                '''
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: 'client/frontend/dist/**/*', allowEmptyArchive: true
            }
        }
    }

    post {
        success { echo 'Build and deployment succeeded! ✅' }
        failure { echo 'Build or deployment failed. ❌' }
    }
}
