pipeline {
    agent any
    
    options {
        buildDiscarder logRotator(daysToKeepStr: '5', numToKeepStr: '7')
    }
    stages{
        stage('Build'){
            steps{
                 sh script: 'mvn clean package'
                 archiveArtifacts artifacts: 'target/*.war', onlyIfSuccessful: true
            }
        }
        stage('Upload War To Nexus'){
            steps{
                script{

                    nexusArtifactUploader artifacts: [[artifactId: 'simple-app', classifier: '', file: 'target/simple-app-3.0.0.war', type: 'war']], 
                    credentialsId: 'nexus-1', groupId: 'in.javahome', nexusUrl: '34.102.33.22:8081/#admin/repository/repositories:telstra', nexusVersion: 'nexus3', protocol: 'http', repository: 'telstra', version: '3.0.0'
            }
        }
    }
}
}
