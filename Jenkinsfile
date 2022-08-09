pipeline {
    agent any
      environment {
        DATE = new Date().format('yy.M')
        TAG = "${DATE}.${BUILD_NUMBER}"
    }
    stages{
        stage('build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Docker Build') {
            steps {
                script {
                    docker.build("sravani7katta/aws-rds:${TAG}")
                }
            }
        }
        stage('Pushing Docker Image to Dockerhub') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com','docker_credential') {
                        docker.image("sravani7katta/aws-rds:${TAG}").push()
                        docker.image("sravani7katta/aws-rds:${TAG}").push("latest")
                    }
                }
            }
        }
        stage('Upload War To Nexus') {
            steps {
                nexusArtifactsUploader artifacts: {
                  {
                     artifactId: 'LoginWebApp',
                     classifier: '',
                     file: 'target', 
                     type: 'war',
                       }
                }
                      credentialsId: 'jenkins repo',
                      groupId: 'com.javawebtutor',
                      nexusUrl: '172.31.20.128:8081',
                      nexusVersion: 'nexus2',
                      protocol: 'http', 
                      repository: 'http://3.90.105.83:8081/nexus/content/repositories/Jenkins_Repo',
                      version: '1.0-SNAPSHOT'
                 }
            
            }
        }
    }
}
    
         '

            
        
    
 
