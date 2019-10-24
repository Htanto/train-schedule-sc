pipeline {
    agent any
    environment {
        //Docker Hub
        APPS_NAME = "train"
        FQDN = "train.foobz.com.au"
        DOCKER_IMAGE_NAME = "reg.foobz.com.au/foobz/train-schedule-sc"
    }
    stages {
        stage('Build Apps and Test') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        stage('Build Docker Image') {
            when {
                branch 'master'
            }
            steps {
                script {
                    app = docker.build(DOCKER_IMAGE_NAME)
                    app.inside {
                        sh 'echo Hello, World!'
                    }
                }
            }
        }
        stage('Push Docker Image') {
            when {
                branch 'master'
            }
            steps {
                script {
                    docker.withRegistry('https://reg.foobz.com.au', 'harbor_login') {
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
            }
        }
        stage('DeployAppServices - AS3') {
            steps {
                // Deploy AppServices with AS3
                milestone(3)
                
            }
        }
    }
}
