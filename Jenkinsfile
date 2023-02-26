pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[
                        url: 'https://github.com/KareemHesham1997/Simple-App-CI-CD',
                        credentialsId: 'GitHub'
                    ]]
                ])
            }
        }
        
        stage('Build') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'Dockerhub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh """
                        ls
                        docker login -u ${USERNAME} -p ${PASSWORD}
                        docker build . -t karimhisham/gcp-simple-app:v1
                        docker push karimhisham/gcp-simple-app:v1
                    """
                } 
            }
        }
        
        stage('Deploy') {
            steps {
                sh """
                    ls
                    cd YAML-FILES
                    ls
                    kubectl apply -f NS.yaml
                    kubectl apply -f app-deplyment.yaml
                    kubectl apply -f LB.yaml
                """
            }
        }
    }
}
