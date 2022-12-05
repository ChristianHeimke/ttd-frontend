pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
              sh '''
                docker build -t christianheimke/ttd-frontend:jenkins-${BUILD_NUMBER} .
              '''
            }
        }
        stage('Release') {
            steps {
              withCredentials([usernamePassword(credentialsId: 'dockerhubcredentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                sh '''
                docker login -u $USERNAME -p $PASSWORD
                docker push christianheimke/ttd-frontend:jenkins-${BUILD_NUMBER}
                '''
              }
            }
        }
        stage('deploy') {
            steps {
                sh '''
                docker stop ttd-frontend || true
                docker rm -f ttd-frontend || true
                docker run -p3000:80 -d --name ttd-frontend christianheimke/ttd-frontend:jenkins-${BUILD_NUMBER}
                '''
            }
        }
    }
}