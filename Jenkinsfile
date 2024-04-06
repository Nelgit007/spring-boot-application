pipeline {

    agent {

        docker {

            image 'abhishekf5/maven-abhishek-docker-agent:v1'
            args '--user root -v /var/run/docker.sock:/var/run/docker.sock'
        }

    }
    stages {

        // stage('Checkout') {
        //     steps {
        //         // The pipeline does not require a checkout stage
        //         sh 'echo checked'
        //         branch 'main'
        //         url: 'https://github.com/Nelgit007/spring-boot-application.git'
        //     }
        // }
        stage('Maven Build and Test') {
            steps {
                sh 'cd spring-boot-app && mvn clean package'
            }
        }
        stage('Static code Analysis: Sonarqube') {
            environment {
                SONAR_URL = 'http://54.174.45.166:9000'
            }

            steps {
                withCredentials([string(credentialsId: 'sonarqube', variable: 'SONAR_AUTH_TOKEN')]) {
                    sh 'cd spring-boot-app && mvn sonar:sonar -Dsonar.login=$SONAR_AUTH_TOKEN -Dsonar.host.url=${SONAR_URL}'
                }
            }
        }
    }
}