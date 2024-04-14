pipeline {
    agent any
    
    environment {
        SERVER_IP = '54.144.121.240' // Replace with your target server IP
        SERVER_USERNAME = 'root' // Replace with your target server username
        SERVER_PASSWORD = credentials('your_server_password_id') // Replace with Jenkins credential ID for SSH password
    }
    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', credentialsId: 'your_git_credentials_id', url: 'https://github.com/nileshsurya1994/WebApplication.git'
            }
        }
        stage('Git Clone') {
            steps {
                script {
                    sshagent(['your_server_ssh_credentials_id']) {
                        sh """
                            sshpass -p ${env.SERVER_PASSWORD} ssh ${env.SERVER_USERNAME}@${env.SERVER_IP} '
                            sudo mkdir -p /opt/workspace && 
                            cd /opt/workspace &&
                            sudo git clone https://github.com/nileshsurya1994/WebApplication.git
                            '
                        """
                    }
                }
            }
        }
        
        stage('Install Apache2') {
            steps {
                script {
                    sshagent(['your_server_ssh_credentials_id']) {
                        sh "sshpass -p ${env.SERVER_PASSWORD} ssh ${env.SERVER_USERNAME}@${env.SERVER_IP} 'sudo apt-get update && sudo apt-get install -y apache2'"
                    }
                }
            }
        }
        
        stage('Deploy') {
            steps {
                script {
                    sshagent(['your_server_ssh_credentials_id']) {
                        sh "ssh ${env.SERVER_USERNAME}@${env.SERVER_IP} 'sudo cp -r /opt/workspace/WebApplication /var/www/html/'"
                    }
                }
            }
        }
    }
}
