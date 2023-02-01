pipeline {
    agent any

    stages {
        stage ("Copy files to ansible server") {
            steps {
                script {
                    echo "copying all ansible configurations to ansible control node"
                    sshagent(['ansible-server-key']) {
                        sh "scp -o StrictHostKeyChecking=no ansible/* victor@${ANSIBLE_SERVER_IP}:/home/victor"

                        withCredentials([sshUserPrivateKey(credentialsId: 'ec2-server-key', keyFileVariable: 'keyfile', usernameVariable: 'user')]) {
                            sh "scp ${keyfile} victor@${ANSIBLE_SERVER_IP}:/home/victor/.ssh/ssh-key.pem"
                        }
                    }
                }
            } 
        }
    }
}