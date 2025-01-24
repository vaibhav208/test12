pipeline {
    agent any
    
    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', poll: false, url: 'https://github.com/Gaur95/test12.git'
            }
        }

        stage('Build and Push Images') {
            steps {
                script {
                    sh 'docker build -t aakashgaur57/demoforjenkins .'
                    withCredentials([usernamePassword(credentialsId: 'dcred', passwordVariable: 'docker_password', usernameVariable: 'docker_user')]) {
                        sh 'docker login -u $docker_user -p $docker_password'
                        sh 'docker push aakashgaur57/demoforjenkins'
                    }
                }
            }
        }

        stage('Deploy Services') {
            steps {
                script {
                    sh 'docker run -d -p 4433:80 aakashgaur57/demoforjenkins'
                }
            }
        }
        
        stage('Post Deployment Testing') {
            steps {
                script {
                    sh 'curl -I http://localhost:4433'
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check logs for details.'
        }
    }
}
