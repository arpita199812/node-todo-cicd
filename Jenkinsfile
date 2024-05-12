pipeline {
    agent any
    
    stages {
        stage("code") {
            steps {
                git url: "https://github.com/arpita199812/node-todo-cicd.git", branch: "master"
                echo 'Code cloned successfully.'
            }
        }
        stage("build and test") {
            steps {
                script {
                    bat "docker build -t arpita199812/node-app-test-new ."
                    echo 'Code built successfully.'
                }
            }
        }
        stage("push") {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-hub', url: 'https://index.docker.io/v1/') {
                        docker.image('arpita199812/node-app-test-new:latest').push()
                    }
                }
            }
        }
        stage("deploy") {
            steps {
                script {
                    bat "docker-compose down && docker-compose up -d"
                    echo 'Deployment completed successfully.'
                }
            }
        }
    }
}
