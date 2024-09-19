pipeline {
    agent any

    environment {
        // Set environment variables
        MAVEN_HOME = "/opt/apache-maven-3.8.8"  // Adjust the path to where Maven is installed
        TOMCAT_SERVER_URL = "http://10.79.23.202:9090"  // Tomcat Manager URL
        TOMCAT_USER = "tomcat"  // Tomcat username
        TOMCAT_PASSWORD = "Password"  // Tomcat password
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the source code from the repository
                git branch: 'main', url: 'https://github.com/ajithkumar2585/sampledevopsproject1.git'
            }
        }

        stage('Build') {
            steps {
                // Build the Maven project
                sh "${MAVEN_HOME}/bin/mvn clean package"
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                script {
                    // Deploy the WAR file to Tomcat using curl
                    def warFile = "/var/lib/jenkins/workspace/NewMavenProject/webapp/target/webapp.war"  // Path to the WAR file
                    
                    sh """
                    curl --upload-file ${warFile} \
                         --user ${TOMCAT_USER}:${TOMCAT_PASSWORD} \
                         ${TOMCAT_SERVER_URL}/manager/text/deploy?path=/webapp&update=true
                    """
                }
            }
        }
    }

    post {
        always {
            // Clean up workspace after build
            cleanWs()
        }
    }
}
