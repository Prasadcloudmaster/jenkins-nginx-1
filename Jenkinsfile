pipeline {
    agent any
    environment {
        VM_HOST = '35.180.126.88'
        VM_USERNAME = 'ubuntu'
        NGINX_PATH = '/usr/share/nginx/html' // Change this to the path where your web server serves content
    }
    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from Git
                git branch: 'main', url: 'https://github.com/rakesh-pise/jenkins-nginx.git'
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
