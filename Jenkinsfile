def readPom
def readPomrepo
def imageName
def tagName
def userId = "karthickveppdai"
def repository = "trafffic-app"
def repository1 = "trafffic-dsh"

pipeline {
    agent any
environment
{
        SERVICE_1_NAME = "traffic"
        SERVICE_2_NAME = "traffic-dashboard"
        DOCKER_REGISTRY = "docker.io"
        DOCKER_IMAGE_NAME = "trafffic-app"
        DOCKER_TAG = "latest"
        DOCKER_CREDENTIALS = "d7dc5cc4-7021-4e3d-9f0b-e000bae23e39"
}

    tools {      
        maven "Maven 3.6.3"
        dockerTool 'docker'
    }

    stages {
        stage('checkout') {            
            steps {  
                dir('traffic') {
                git branch: 'main', credentialsId: 'b3217732-06ce-407e-a974-a6af3f476792', url: 'https://github.com/KarthickVeppdai/traffic.git'
                }
            
           
                dir('traffic-dashboard') {
                git branch: 'main', credentialsId: 'b3217732-06ce-407e-a974-a6af3f476792', url: 'https://github.com/KarthickVeppdai/trafficdashboard.git'
                }
            }   
        } //checkout
        stage('Build') {
            
            steps {
                 dir('traffic') {
                      script {
                       readPom = readMavenPom file: 'pom.xml';
                       repository="${readPom.artifactId}"
                      }
                sh "mvn clean install"  
                 }
            
                  dir('traffic-dashboard') {
                       script {
                       readPomrepo = readMavenPom file: 'pom.xml';
                       repository1="${readPomrepo.artifactId}"
                       }
                      
                  sh "mvn clean install"     
                  }
            } 
        } //build   
        stage('Build Docker Image') {
            steps {
                script {   
                    dir('traffic') {
                    docker.build("${repository}")
                    }
                }
          
                script {   
                    dir('traffic-dashboard') {
                    docker.build("${repository1}")
                    }
                }
            }  
        }//build image

        stage('Login to Docker Registry') {
            steps {
                script {                   
                    withCredentials([usernamePassword(credentialsId: "${DOCKER_CREDENTIALS}", usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh 'echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin'
                    }
                }
            }
        }

        
        
        stage('Push Docker Image') {
            steps {
                   // sh "docker tag ${repository1} ${userId}/${repository1}:${env.BUILD_ID}"
                  //  sh "docker push ${userId}/${repository1}:${env.BUILD_ID}"
                   // sh "docker tag ${repository} ${userId}/${repository}:${env.BUILD_ID}"
                   // sh "docker push ${userId}/${repository}:${env.BUILD_ID}"
                    sh 'docker logout'
            }
        }
        stage('Deployment') {
            steps {
                     sh 'docker-compose up'
            }
        }
        stage('Remove Images') {
            steps {
                     echo "removing images"
            }
        }
    }
}






    

