Jenkinsfile (Declarative Pipeline)
/* Requires the Docker Pipeline plugin */
// pipeline {
//     agent {
//         docker { image 'node:24.13.0-alpine3.23' }
//     }
//     stages {
//         stage('Test') {
//             steps {
//                 echo "updating java file for jeenkins"
//                 sh 'node --eval "console.log(process.platform,process.env.CI)"'
//             }
//         }
//     }
// }

// pipeline {
//     agent { docker { image 'maven:3.9.11-eclipse-temurin-21-alpine' } }
//     stages {
//         stage('build') {
//             steps {
//                 echo "----------Here I am printing mvn version from jenkins file: ----------"
//                 sh 'mvn --version'
//             }
//         }
//     }
// }
/* Requires the Docker Pipeline plugin */

       pipeline {
        agent {
            docker { image 'node:24.13.0-alpine3.23' }
        }
        stages {
            stage('Build') {
                steps {
                    sh 'make' // Placeholder for the build command (e.g., 'mvn compile')
                }
            }
            stage('Test') {
                steps {
                    sh 'make check' // Placeholder for running tests (e.g., 'mvn test')
                    // Gathers test results for visualization and trend analysis
                    junit 'reports/**/*.xml' 
                }
            }
            stage('Deploy') {
                steps {
                    sh 'make publish' // Placeholder for deployment command
                }
            }
        }
    }

