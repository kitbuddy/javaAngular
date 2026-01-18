// Jenkinsfile (Declarative Pipeline)
pipeline {
    agent any

    tools {
        maven 'Maven3'
    }
    stages {
        stage('Build') {
            steps {
                echo 'Building the project...'
                sh 'mvn clean install'
                // Add your build commands here, e.g., sh 'mvn clean install'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                // Add your test commands here, e.g., sh 'mvn test'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying the application...'
                // Add your deploy commands here, e.g., sh 'kubectl apply -f deployment.yaml'
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed.'
        }
    }
}
