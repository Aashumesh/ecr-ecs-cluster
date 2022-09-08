pipeline {
    agent any
    environment {
        ECR_REGISTRY = '274213768634.dkr.ecr.ap-south-1.amazonaws.com/myrepo'
        PATH = "/usr/local/bin/:${env.PATH}"
    }
    
    stages {
        stage('Cloning Git') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '', url: 'https://github.com/Aashumesh/ecr-ecs-cluster.git']]])     
            }
        }
        
        stage('Build Docker Image') {
            steps {
                sh 'docker build --force-rm -t "274213768634.dkr.ecr.ap-south-1.amazonaws.com/myrepo" .'
                sh 'docker image ls'
            }
        }
        
        stage('Push Image to ECR Repo') {
            steps {
                sh 'aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 274213768634.dkr.ecr.ap-south-1.amazonaws.com'
                sh 'docker push 274213768634.dkr.ecr.ap-south-1.amazonaws.com/myrepo'
            }
        }
        
        stage('Deploy on Docker Machine') {
            steps {
                sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin --password-stdin 274213768634.dkr.ecr.ap-south-1.amazonaws.com'
                sh 'docker pull 274213768634.dkr.ecr.ap-south-1.amazonaws.com/myrepo:latest'
                sh 'docker rm -f mypythonContainer | echo "there is no docker container named todo"'
                sh 'docker run --name mypythonContainer -dp 8096:5000 274213768634.dkr.ecr.ap-south-1.amazonaws.com/myrepo:latest'
            }
        }
    }
}

