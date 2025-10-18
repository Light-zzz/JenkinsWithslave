pipeline {
    agent {label : 'Python_web'}
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

        // stage('Deploy on Remote VM') {
        //     steps {
        //         withCredentials([sshUserPrivateKey(credentialsId: 'appVM', keyFileVariable: 'SSH_KEY')]) {
        //             sh """
        //                 # Transfer index.html to remote VM
        //                 scp -i "$SSH_KEY" -o StrictHostKeyChecking=no index.html ec2-user@13.235.24.161:/tmp/

        //                 # Run remote deployment commands
        //                 ssh -i "$SSH_KEY" -o StrictHostKeyChecking=no ec2-user@13.235.24.161 << 'EOF'
        //                     # Install Nginx if not installed
        //                     if ! command -v nginx &> /dev/null; then
        //                         sudo dnf install -y nginx
        //                     fi

        //                     # Move index.html to Nginx root
        //                     sudo mv /tmp/index.html /usr/share/nginx/html/index.html

        //                     # Restart Nginx
        //                     sudo systemctl restart nginx
        //                 EOF
        //             """
        //         }
        //     }
        // }
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
