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
                    docker.build("sravani1katta/aws-rds:${TAG}")
                }
            }
        }
       stage('Upload to Nexus')
        steps {
            script {
                nexusArtifactUploader artifacts: [[artifactId: 'LoginWebApp', classifier: '', file: 'LoginWebApp/target/LoginWebApp.war', type: 'war']], credentialsId: 'nexus', groupId: 'com.javawebtutor', nexusUrl: '34.224.66.217:8081/nexus', nexusVersion: 'nexus2', protocol: 'http', repository: 'maven-snapshots', version: '1.0-SNAPSHOT'
            }
        }
     }
  }
}
