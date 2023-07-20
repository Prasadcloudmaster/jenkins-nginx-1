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


pipeline {
    agent any
    environment {
        SERVER_IP = '35.180.126.88'
        NGINX_CONF_PATH = '/etc/nginx/' // Change this to the path where Nginx configurations reside
        NGINX_STATIC_PATH = '/var/www/html/' // Change this to the path where your static files are located
    }
    stages {
        stage('Checkout') {
            steps {
                // Checkout the Nginx configurations and static files from your repository
                git branch: 'main', url: 'https://github.com/rakesh-pise/jenkins-nginx.git'
            }
        }

        stage('Deploy Nginx Config') {
            steps {
                // Copy Nginx configuration files to the target server
                sshagent(['private']) {
                    sh "scp -r nginx_conf/* ${SERVER_IP}:${NGINX_CONF_PATH}/"
                }
            }
        }

        stage('Deploy Static Files') {
            steps {
                // Copy static files to the target server
                sshagent(['private']) {
                    sh "scp -r static/* ${SERVER_IP}:${NGINX_STATIC_PATH}/"
                }
            }
        }

        stage('Reload Nginx') {
            steps {
                // Reload Nginx on the target server to apply the changes
                sshagent(['private']) {
                    sh "ssh ${SERVER_IP} sudo systemctl reload nginx"
                }
            }
        }
    }
}
