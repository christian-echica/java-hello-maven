pipeline {
    agent any

    stages {
        stage('Check Environment Versions') {
            steps {
                // Print Maven, Docker, and Java versions
                sh '''
                    echo "Maven Version:"
                    mvn -version
                    echo "Docker Version:"
                    docker --version
                    echo "Java Version:"
                    java -version
                '''
            }
        }

        stage('List Files') {
            steps {
                // List files in the workspace
                sh 'ls -l'
            }
        }
        
        stage('Maven Build') {
            steps {
                // Run Maven build and show the path to the WAR file
                sh '''
                    mvn clean install
                    echo "WAR file location:"
                    find $(pwd) -name "*.war"
                '''
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                // Deploy the WAR file to the EC2 Tomcat server using SSH credentials
                withCredentials([sshUserPrivateKey(credentialsId: 'ec2-tomcat-id-key', keyFileVariable: 'SSH_KEY')]) {
                    sh '''
                        WAR_FILE=$(find $(pwd) -name "*.war")
                        echo "Deploying $WAR_FILE to EC2 Tomcat server"
                        scp -o StrictHostKeyChecking=no -i $SSH_KEY $WAR_FILE ubuntu@54.226.69.10:/home/ubuntu/
                        ssh -i $SSH_KEY ubuntu@54.144.31.7 "sudo mv /home/ubuntu/$(basename $WAR_FILE) /opt/tomcat/apache-tomcat-10.1.28/webapps/"
                    '''
                }
            }
        }
    }
}
