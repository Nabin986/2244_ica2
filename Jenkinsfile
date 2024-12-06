pipeline {
    agent any
    stages {
        stage('Cleanup') {
            steps {
                cleanWs()
            }
        }

        stage('Clone Git Repo') {
            steps {
                checkout scm
            }
        }
        stage('Clone from repository') {
            steps {
                git url: 'https://github.com/Nabin986/2244_ica2.git', branch: 'develop', credentialsId: 'GIT'
            }
        }

        stage('Build and run docker image') {
            steps {
                sh 'sudo docker build -t nabin2625/nabinwebsite:latest .'
                sh "sudo docker tag nabin2625/nabinwebsite:latest nabin2625/nabinwebsite:develop-${env.BUILD_ID}" 
                sh 'sudo docker run -d -p 8081:80 nabin2625/nabinwebsite:latest'
            } 
        }


        stage('Build and Push') {
            steps {
                echo 'Building..'
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-auth', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                        sh '''
                            sudo docker login -u ${USERNAME} -p ${PASSWORD}
                            sudo docker push nabin2625/nabinwebsite:latest
                        '''
                        sh "sudo docker push nabin2625/nabinwebsite:develop-${env.BUILD_ID}"
                    }
            }
        }

        stage('testing') {
            steps {
                sh 'curl -I 192.168.219.163:8081'
            }
        }

    
    }
}
