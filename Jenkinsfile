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
