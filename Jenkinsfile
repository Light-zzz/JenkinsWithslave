pipeline {
    agent {label 'webapp'}
    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
                sh '''
                    pwd
                    whoami
                    hostname -i
                '''
            }
        }

        stage('Deploy on Remote VM') {
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: 'webapp', keyFileVariable: 'SSH_KEY')]) {
                    sh """
                       # Copy file
                       scp -i "$SSH_KEY" -o StrictHostKeyChecking=no index.html script.js style.css ubuntu@43.205.142.50:/tmp/
                        # Install nginx in ubuntu
                        ssh -i "$SSH_KEY" -o StrictHostKeyChecking=no ubuntu@43.205.142.50 "sudo apt install nginx -y && sudo systemctl enable nginx"
                        # Move and restart nginx
                        ssh -i "$SSH_KEY" -o StrictHostKeyChecking=no ubuntu@43.205.142.50 "sudo mv /tmp/index.html /var/www/html/index.html"
                        ssh -i "$SSH_KEY" -o StrictHostKeyChecking=no ubuntu@43.205.142.50 "sudo mv /tmp/script.js /var/www/html/script.js"
                        ssh -i "$SSH_KEY" -o StrictHostKeyChecking=no ubuntu@43.205.142.50 "sudo mv /tmp/style.css /var/www/html/style.css"
                        # Enable and Start the ngnix
                        sudo systemctl start nginx
                    """
                }
            }
        }
    }
}
// pipeline {
//     agent any

//     stages {
//         stage('Checkout SCM') {
//             steps {
//                 checkout scm
//                 sh '''
//                     pwd 
//                     ls -ltr
//                     hostname -i
//                 '''
//             }
//         }

//         stage('Access other VM') {
//             steps {
//                 withCredentials([sshUserPrivateKey(credentialsId: 'appVM', keyFileVariable: 'SSH_KEY',)]) {
//                     sh '''
//                         # Install nginx
//                         ssh -i "$SSH_KEY" -o StrictHostKeyChecking=no ec2-user@13.235.24.161 "sudo dnf install nginx -y"

//                         # Copy file
//                         scp -i "$SSH_KEY" -o StrictHostKeyChecking=no JenkinsWithslave/* ec2-user@13.235.24.161:/tmp/

//                         # Move and restart nginx
//                         ssh -i "$SSH_KEY" -o StrictHostKeyChecking=no ec2-user@13.235.24.161 "sudo mv /tmp/index.html /usr/share/nginx/html/index.html"
//                         ssh -i "$SSH_KEY" -o StrictHostKeyChecking=no ec2-user@13.235.24.161 "sudo mv /tmp/index.html /usr/share/nginx/html/index.html"
//                         ssh -i "$SSH_KEY" -o StrictHostKeyChecking=no ec2-user@13.235.24.161 "sudo mv /tmp/index.html /usr/share/nginx/html/index.html"
//                         ssh -i "$SSH_KEY" -o StrictHostKeyChecking=no ec2-user@13.235.24.161 "sudo mv /tmp/index.html /usr/share/nginx/html/index.html"
//                         sudo systemctl enable nginx && sudo systemctl start nginx
//                     '''
//                 }
//             }
//         }
//     }
// }
