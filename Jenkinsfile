pipeline {
    agent any
    stages {
        stage('Build and run docker image') {
            steps {
                sh 'sudo docker pull nabin2625/nabinwebsite:latest'
                sh 'sudo docker run -d -p 8082:80 nabin2625/nabinwebsite:latest'
            } 
        }


        stage('testing') {
            steps {
                sh 'curl -I 15.222.6.81:8082'
            }
        }

    
    }
}
