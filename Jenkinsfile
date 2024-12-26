def readPom

pipeline {
    agent any
    
    

environment
{
    PROJECT1_NAME = "springboot-app1"
    REGISTRY = "docker.io"
    DOCKER_HUB_CREDENTIALS = credentials('docker-hub')
}




    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "Maven 3.9.9"
    }

    stages {
        stage('Build') {
            
            steps {
                dir('springboot-app1') {
                git branch: 'main', credentialsId: 'b3217732-06ce-407e-a974-a6af3f476792', url: 'https://github.com/KarthickVeppdai/traffic.git'
                sh "mvn clean install"
                }
            steps{
                   readPom = readMavenPom file: 'pom.xml';
                  echo "Application is running with Jenkins Build Number #${env.BUILD_ID}"
                  echo "Application Name: ${readPom.name}"
                  echo "Application Version: ${readPom.version}"
            }
        }
    }
}
