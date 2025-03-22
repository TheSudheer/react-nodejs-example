pipeline {
    agent any
    stages {
        stage("test") {
            steps {
                script {
                    echo "Testing the application..."
                }
            }
        }
        stage("build") {
            steps {
                script {
                    echo "Building the application..."
                }
            }
        }
        stage("deploy") {
            steps {
                script {
                    sshagent(['ec2-server-key']) {
                        def dockerCmd = "docker run -d -p 3080:3080 kalki2878/react-js-app:2.0"
                        sh "ssh -o StrictHostKeyChecking=no ec2-user@52.87.160.165 ${dockerCmd}"
                        echo "Deploying the application..."
                    }
                }
            }
        }
    }
}
