pipeline {
    agent any

    environment {
        REMOTE_USER = 'ec2-user'
        REMOTE_HOST = '3.109.49.255'
        REMOTE_PATH = '/usr/share/nginx/html'
        SSH_KEY_ID = 'appVM'
    }

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

        stage('Archive Site') {
            steps {
                sh '''
                    rm -f site.tar.gz
                    tar --exclude=site.tar.gz -czf site.tar.gz *
                '''
            }
        }

        stage('Transfer to Remote VM') {
            steps {
                sshagent([SSH_KEY_ID]) {
                    sh "scp -o StrictHostKeyChecking=no site.tar.gz ${REMOTE_USER}@${REMOTE_HOST}:/tmp/"
                }
            }
        }

        stage('Deploy on Remote VM') {
    steps {
        sshagent([SSH_KEY_ID]) {
            sh """
                ssh -o StrictHostKeyChecking=no ${REMOTE_USER}@${REMOTE_HOST}
                    # Install Nginx only if not present
                    if ! command -v nginx &> /dev/null; then
                        sudo yum install -y nginx
                    fi

                    # Clean old site and deploy new one
                    sudo rm -rf ${REMOTE_PATH}/*
                    sudo tar xzf /tmp/site.tar.gz -C ${REMOTE_PATH}

                    # Restart Nginx
                    sudo systemctl restart nginx
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
//                         sudo systemctl restart nginx
//                     '''
//                 }
//             }
//         }
//     }
// }
