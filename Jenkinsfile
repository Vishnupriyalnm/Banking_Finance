pipeline {
    agent any

    stages {
        stage('Git Clone') {
            steps {
                git branch: 'master', url: 'https://github.com/Vishnupriyalnm/Banking_Finance.git'
                sh 'mvn clean package'
            }
        }
        stage('Build docker image') {
            steps {
                sh 'docker build -t vishnupriyalnm/financedemo:1 .'
            }
        }
        stage('Docker image to dockerhub') {
            steps {
                withCredentials([string(credentialsId: 'dockerhubpass', variable: 'dockerhubpass')]) {
                 sh "docker login -u  vishnupriyalnm -p ${dockerhubpass}"
                 sh "docker push vishnupriyalnm/financedemo:1"
}
            }
        }
       stage('Deploy Using Docker') {
            steps {
                script {
                    sh 'docker stop financedemo || true'
                    sh 'docker rm financedemo || true'
                    sh 'docker run -d --name financedemo -p 9090:8081 vishnupriyalnm/financedemo:1'
                }
            }
        }
    }
}
