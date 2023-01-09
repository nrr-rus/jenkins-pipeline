pipeline {
    agent any
    environment {
        DOCKER_HOST="unix://\$(pwd)/docker.sock"
        STAGE_INSTANCE="ubuntu@172.31.30.76"
    }
    stages {
        stage('Setup SSH tunnel') {
            steps {
                script {
                    sh "ssh -nNT -L \$(pwd)/docker.sock:/var/run/docker.sock -i /var/lib/jenkins/.ssh/jenkins ${STAGE_INSTANCE} & echo \$! > /tmp/tunnel.pid"
// sometimes it's not enough time to make a tunnel, add sleep
                    sleep 10
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    sh "DOCKER_HOST=${DOCKER_HOST} docker ps -a"
                }
            }
        }
    }
    post {
        always {
            script {
                sh "pkill -F /tmp/tunnel.pid"
                sh "sudo rm docker.sock"
           }
       }
    }
}
