pipeline {
    agent any

    environment {
        ANSIBLE_SERVER_IP = "143.198.47.83"
    }

    stages {
        stage ("Copy files to ansible server") {
            steps {
                script {
                    echo "copying all ansible configurations to ansible control node"
                    sshagent(['ansible-server-key']) {
                        sh "scp -o StrictHostKeyChecking=no ansible/* root@${ANSIBLE_SERVER_IP}:/home/victor/jenkins"

                        withCredentials([sshUserPrivateKey(credentialsId: 'ec2-server-key', keyFileVariable: 'keyfile', usernameVariable: 'user')]) {
                            sh 'scp ${keyfile} root@${ANSIBLE_SERVER_IP}:/home/victor/.ssh/ssh-key.pem'
                        }
                    }
                }
            } 
        }

        stage ("Execute Ansible playbook") {
            steps {
                script {
                    def remote = [:]
                    remote.name = "ansible-server"
                    remote.host = ANSIBLE_SERVER_IP
                    remote.allowAnyHosts = true

                    withCredentials([sshUserPrivateKey(credentialsId: 'ansible-server-key', keyFileVariable: 'identity', usernameVariable: 'userName')]) {
                        remote.user = 'root'
                        remote.identityFile = identity
                        sshCommand remote: remote, command: "ls -la"
                    }
                }
            }
        }
    }
}