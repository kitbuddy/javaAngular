pipeline {
    agent any   // Run on any available Jenkins executor

    environment {
        NODE_ENV = "production"
        JAVA_HOME = "/usr/lib/jvm/java-17-openjdk-amd64" // adjust for your EC2
        PATH = "${JAVA_HOME}/bin:${env.PATH}"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main', 
                    url: 'https://github.com/kitbuddy/javaAngular.git', 
                    credentialsId: 'github-pat-credentials'  // Replace with your Jenkins credential ID
            }
        }

        stage('Install Angular Dependencies') {
            steps {
                dir('client/frontend') {
                    echo "Installing Angular dependencies..."
                    sh 'npm cache clean --force'
                    sh 'npm install'
                }
            }
        }

        stage('Build Angular Frontend') {
            steps {
                dir('client/frontend') {
                    echo "Building Angular frontend..."
                    sh 'npx ng build --prod'
                }
            }
        }

        stage('Build Java Backend') {
            steps {
                dir('server/backend') {
                    echo "Building Java backend..."
                    sh 'mvn clean install'
                }
            }
        }

        stage('Archive Artifacts') {
            steps {
                // Angular dist folder
                archiveArtifacts artifacts: 'client/frontend/dist/**/*', allowEmptyArchive: true
                // Java backend JARs
                archiveArtifacts artifacts: 'server/backend/target/*.jar', allowEmptyArchive: true
            }
        }

    }

    post {
        success {
            echo 'Build succeeded! ✅'
        }
        failure {
            echo 'Build failed. ❌'
        }
    }
}
