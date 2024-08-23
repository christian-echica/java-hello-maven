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
    }
}
