pipeline {

    agent any

    stages {

        stage('Init') {
            steps {

                sh '''
                docker network create jenk-network || echo "Network Already Exists"
                docker stop flask-app || echo "flask-app Not Running"
                docker stop nginx || echo "nginx Not Running"
                docker rm flash-app || echo "flask-app Not Running"
                docker rm nginx || echo "nginx Not Running"
                '''

            }

        }

        stage('Build') {

            steps {

                sh '''
                docker build -t nm12994/flask-jenk .
                docker build -t nm12994/nginx-jenk ./nginx
                '''

            }

        }

        stage('Push') {

            steps {
                //comment again
                sh '''
                docker push nm12994/flask-jenk
                docker push nm12994/nginx-jenk  
                '''

            }

        }

        stage('Deploy') {

            steps {

                sh '''
                docker run -d --name flask-app --network jenk-network nm12994/flask-jenk
                docker run -d -p 80:80 --name nginx --network jenk-network nm12994/nginx-jenk
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