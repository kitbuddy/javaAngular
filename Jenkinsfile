pipeline {
    agent any

    environment {
        NODE_ENV = "production"
        JAVA_HOME = "/usr/lib/jvm/java-17-openjdk-amd64"
        PATH = "/Users/ankitjain/.nvm/versions/node/v22.13.1/bin:${JAVA_HOME}/bin:/bin:/usr/bin:${env.PATH}"
    }

    tools {
        nodejs "Node18"
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
                    sh 'npm install --legacy-peer-deps'
                }
            }
        }

        stage('Build Angular Frontend') {
            steps {
                dir('client/frontend') {
                    echo "Building Angular frontend..."
                    sh 'npx ng build --configuration production'
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
                archiveArtifacts artifacts: 'client/frontend/dist/**/*', allowEmptyArchive: true
                archiveArtifacts artifacts: 'server/backend/target/*.jar', allowEmptyArchive: true
            }
        }
    }

    post {
        success { echo 'Build succeeded! ✅' }
        failure { echo 'Build failed. ❌' }
    }
}
