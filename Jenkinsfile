pipeline {
    agent any
    
    stages {
        stage("code") {
            steps {
                git url: "https://github.com/arpita199812/node-todo-cicd.git", branch: "master"
                echo 'Code cloned successfully.'
            }
        }
        stage("Sonarqube Analysis") {
           environment {
    SCANNER_HOME = tool 'sonarqube'  // sonar-scanner is the name of the tool in the manage jenkins> tool configuration
   }
   steps {
    withSonarQubeEnv(installationName: 'sonarqube') {  //installationName is the name of sonar installation in manage jenkins>configure system
     bat "%SCANNER_HOME%/bin/sonar-scanner \
     -Dsonar.projectKey=arpita199812_todo-app-scan \
     -Dsonar.token=sonar-qube \
     -Dsonar.sources=. \
     -Dsonar.host.url=http://localhost:9000 \
     -Dsonar.inclusions=app.js \
     -Dsonar.test.inclusions=app.test.js "
    }
   }
        }
        stage('OWASP Dependency Check') {
            steps {
                dependencyCheck additionalArguments: '--scan views/', odcInstallation: 'dc'
                     dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
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
