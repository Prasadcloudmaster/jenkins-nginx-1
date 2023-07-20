pipeline {
    agent any
    environment {
        VM_HOST = '172.31.28.190'
        VM_USERNAME = 'ubuntu'
        NGINX_PATH = '/usr/share/nginx/html' // Change this to the path where your web server serves content
    }
    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from Git
                git branch: 'main', credentialsId: 'your_git_credential_id', url: 'your_git_repo_url'
            }
        }

        stage('Deploy to VM') {
            steps {
                // Copy the code to the virtual machine
                sshagent(['ssh-cred']) {
                    sh "scp -r * ${VM_USERNAME}@${VM_HOST}:${NGINX_PATH}/"
                }
            }
        }

        stage('Restart Nginx') {
            steps {
                // Restart Nginx on the VM
                sshagent(['ssh-cred']) {
                    sh "ssh ${VM_USERNAME}@${VM_HOST} sudo systemctl restart nginx"
                }
            }
        }
    }
}
