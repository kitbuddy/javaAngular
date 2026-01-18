Jenkinsfile (Declarative Pipeline)
/* Requires the Docker Pipeline plugin */
pipeline {
    agent { docker { image 'maven:3.9.11-eclipse-temurin-21-alpine' } }
    stages {

        stage('build') {
            steps {
                echo "----------Here I am printing mvn version from jenkins file: ----------"
                sh 'mvn --version'
            }
        }
    }
}
