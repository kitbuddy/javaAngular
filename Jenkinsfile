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
                echo 'Deploying Angular build to EC2...'
                sh """
                    # Copy build files to EC2
                    scp -i /Users/ankitjain/Desktop/javaAngularAWS/javaAngularAgain.pem \
                        -o StrictHostKeyChecking=no \
                        -r client/frontend/dist/frontend/* \
                        ec2-user@ec2-3-133-115-171.us-east-2.compute.amazonaws.com:/home/ec2-user/angular-app/

                    # SSH into EC2 and move files to web root, create directory if missing, and restart nginx
                    ssh -i /Users/ankitjain/Desktop/javaAngularAWS/javaAngularAgain.pem \
                        -o StrictHostKeyChecking=no \
                        ec2-user@ec2-3-133-115-171.us-east-2.compute.amazonaws.com "
                            # Create target directory if it doesn't exist
                            sudo mkdir -p /var/www/html &&
                            # Move files to Nginx web root
                            sudo mv /home/ec2-user/angular-app/* /var/www/html/ &&
                            # Set correct permissions
                            sudo chown -R nginx:nginx /var/www/html &&
                            # Restart Nginx to serve the new build
                            sudo systemctl restart nginx
                        "
                """
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
