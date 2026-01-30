pipeline {
    agent any

    tools {
        jdk 'Java17'
        maven 'Maven'
    }

    stages {

        stage('Checkout Code') {
            steps {
                echo 'Cloning source code from GitHub'
                git branch: 'main',
                    credentialsId: 'mygithubcred',
                    url: 'https://github.com/sundaryoganand/Devopsjan.git'
            }
        }

        stage('Build Project') {
            steps {
                echo 'Building Maven project'
                bat 'mvn clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image'
                bat 'docker build -t mvnproj:1.0 .'
            }
        }

        stage('Push Docker Image to DockerHub') {
            steps {
                echo 'Pushing Docker image to DockerHub'
                withCredentials([string(credentialsId: 'dockerhubpwd', variable: 'DOCKER_PASS')]) {
                    bat '''
                    echo %DOCKER_PASS% | docker login -u Sundaryoganand2003 --password-stdin
                    docker tag mvnproj:1.0 Sundaryoganand2003/mymvnproj:latest
                    docker push Sundaryoganand2003/mymvnproj:latest
                    '''
                }
            }
        }

        stage('Deploy Using Docker Container') {
            steps {
                echo 'Running Docker container'
                bat '''
                docker rm -f myjavaappcont || exit 0
                docker run -d --name myjavaappcont Sundaryoganand2003/mymvnproj:latest
                '''
            }
        }
    }

    post {
        success {
            echo '✅ PIPELINE SUCCESS – GREEN TICK ACHIEVED'
        }
        failure {
            echo '❌ PIPELINE FAILED'
        }
    }
}
