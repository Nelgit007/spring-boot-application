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
        stage('1. clean up') {
            // Clean up workspace dir for new build
            steps {
                deleteDir ()
            }
        }

        stage('2. Maven Test and Build') {
            steps {
                sh 'cd spring-boot-app && mvn clean package'
            }
        }

        stage('3. Static code Analysis: Sonarqube') {
            environment {
                SONAR_URL = "http://35.174.200.43:9000/"
            }

            steps {
                withCredentials([string(credentialsId: 'sonarqube', variable: 'SONAR_AUTH_TOKEN')]) {
                    sh 'cd spring-boot-app && mvn sonar:sonar -Dsonar.login=$SONAR_AUTH_TOKEN -Dsonar.host.url=${SONAR_URL}'
                }
            }
        }
        
        stage('4. Docker image Build, image scan and push to docker hub') {
            environment {
                DOCKER_IMAGE = "nelsonosagie/spring-boot-v2.1:${BUILD_NUMBER}"
                REGISTRY_CREDENTIALS = credentials('docker-hub')
            }
            steps {
                script {
                    sh 'cd spring-boot-app && docker build -t ${DOCKER_IMAGE} .'
                    def dockerImage = docker.image("${DOCKER_IMAGE}")
                    docker.withRegistry('https://index.docker.io/v1/', "docker-hub") {
                        dockerImage.push()
                    }
                }
            }
        }
    }      
}