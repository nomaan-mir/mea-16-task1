pipeline {

    agent any

    stages {

        stage('Build') {
            steps {

                sh '''
                docker build -t nm12994/flask-jenk:latest -t nm12994/flask-jenk:v${BUILD_NUMBER} .
                '''

            }

        }

        stage('Push') {

            steps {

                sh '''
                docker push -t nm12994/flask-jenk:latest
                docker push -t nm12994/nginx-jenk:v${BUILD_NUMBER}
                '''

            }

        }

        stage('Deploy') {

            steps {
                //comment again
                sh '''
                kubectl apply -f ./kubernetes 
                kubectl set image deployment/flask-deployment task1=nm12994/flask-jenk:v${BUILD_NUMBER}
                '''

            }

        }
        
        stage('CleanUp') {

            steps {

                sh '''
                docker system prune -f 
                '''

            }
        }   
    }

}