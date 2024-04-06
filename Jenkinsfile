pipeline {

    agent {

        docker {

            image 'abhishekf5/maven-abhishek-docker-agent:v1'
            args '--user root -v /var/run/docker.sock:/var/run/docker.sock'
        }

    }
    stages {

        stage('Checkout') {
            steps {
                sh 'echo checked'
                git branch 'main'
                url: 'https://github.com/Nelgit007/spring-boot-application.git'
            }
        }
    }
}