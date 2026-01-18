Jenkinsfile (Declarative Pipeline)
/* Requires the Docker Pipeline plugin */
pipeline {
    agent { docker { image 'maven:3.9.11-eclipse-temurin-21-alpine' } }
    stages {
        echo "----------Here I am printing mvn version from jenkins file: ----------"

        stage('build') {
            steps {
                sh 'mvn --version'
            }
        }
    }
}
