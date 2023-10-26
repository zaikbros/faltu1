pipeline {
    agent any
    stages {
        stage('Git pull') {
            steps {
                script {
                    checkout([$class: 'GitSCM', branches: [[name: '*/master']], userRemoteConfigs: [[url: 'https://github.com/zaikbros/subham-repo']]])
                }
            }
        }
        stage('Docker Login') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'dockerhub-pwd', variable: 'dockerhubpwd')]) {
                        sh "docker login -u zaikbros -p ${dockerhubpwd}"
                    }
                }
            }
        }
        stage('Docker Build') {
            steps {
                sh 'docker build -t zaikbros/ko:latest .'
                sh 'docker run -d -p 8000:8000 zaikbros/ko:latest'
            }
        }
        stage('Pushing image to Dockerhub') {
            steps {
                sh 'docker push zaikbros/ko:latest'
            }
        }
    }
}

