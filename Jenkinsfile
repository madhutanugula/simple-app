pipeline {
    
    agent any
    
    environment {
        imageName = "myapachetomcat"
        registryCredentials = "nexus-1"
        registry = "34.102.33.22:8085/"
        dockerImage = ''
    }
    
    stages {
        stage('Code checkout') {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/MadhuTanugula14/dockerfiles.git'                   }
        }
    
    // Building Docker images
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build imageName
        }
      }
    }

    // Uploading Docker images into Nexus Registry
    stage('Uploading to Nexus') {
     steps{  
         script {
             docker.withRegistry( 'http://'+registry, registryCredentials ) {
             dockerImage.push('latest')
          }
        }
      }
    }
    
    // Stopping Docker containers for cleaner Docker run
    stage('stop previous containers') {
         steps {
            sh 'docker ps -f name=mytomcatcontainer -q | xargs --no-run-if-empty docker container stop'
            sh 'docker container ls -a -fname=mytomcatcontainer -q | xargs -r docker container rm'
         }
       }
      
    stage('Docker Run') {
       steps{
         script {
                sh 'docker run -d -p 8080:8080 --rm --name mytomcatcontainer ' + registry + imageName
            }
         }
      }    
    }
