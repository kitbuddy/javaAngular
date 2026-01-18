Jenkinsfile (Declarative Pipeline)
/* Requires the Docker Pipeline plugin */
echo "Here I am printing mvn version from jenkins file "
pipeline {
    agent { docker { image 'maven:3.9.11-eclipse-temurin-21-alpine' } }
    stages {
        stage('build') {
            steps {
                sh 'mvn --version'
            }
        }
    }
}
