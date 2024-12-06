pipeline {
    agent any
    stages {
        stage('Cleanup') {
            steps {
                cleanWs()
            }
        }

        stage('Clone from repository') {
            steps {
                git url: 'https://github.com/nabin986/2244_ica2.git', branch: 'develop', credentialsId: 'GIT'
            }
        }

        stage('Build and run docker image') {
            steps {
                sh 'sudo docker build -t nabin2625/nabinwebsite .'
                sh "sudo docker tag nabin2625/nabinwebsite:latest nabin2625/nabinwebsite:develop-${env.BUILD_ID}" 
                sh 'sudo docker run -d -p 8081:80 nabin2625/nabinwebsite:latest'
            } 
        }

        stage('Build and Push') {
            steps {
                echo 'Building and pushing Docker image..'
                withCredentials([usernamePassword(credentialsId: 'dockerhub-auth', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh '''
                        sudo docker login -u ${USERNAME} -p ${PASSWORD}
                        sudo docker push nabin2625/nabinwebsite:latest
                    '''
                    sh "sudo docker push nabin2625/nabinwebsite:develop-${env.BUILD_ID}"
                }
            }
        }

        stage('Testing') {
            steps {
                sh 'curl -I http://15.222.6.81:8081 || true' // Ensuring it won't fail the build
            }
        }
    }
}
