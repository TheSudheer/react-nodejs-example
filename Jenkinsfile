pipeline {
    agent any
    stages {
        stage("test") {
            steps {
                script {
                    echo "Testing the application..."
                    sh 'CI=true npm test'
                }
            }
        }
        stage('Docker Build') {
            steps {
                sh "docker build -t kalki2878/react-js-app:2.0 ."
            }
        }
        stage('Push to Registry') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                    sh "echo $DOCKER_PASSWORD | docker login -u ${DOCKER_USERNAME} --password-stdin"
                    sh "docker push kalki2878/react-js-app:2.0"
                }
            }
        }
        stage('Deploy') {
            steps {
                sshagent(['ec2-server-key']) {
                    // Pull the latest image and run a new container
                    sh "ssh -o StrictHostKeyChecking=no ubuntu@52.87.160.165 'docker pull kalki2878/react-js-app:2.0 && docker run -d --name myapp -p 3080:3080 kalki2878/react-js-app:2.0'"
                    echo "Deploying the application..."
                }
            }
        }
    }
}
