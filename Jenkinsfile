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

                    moveComponents destination: 'telstra', nexusInstanceId: '2091', tagName: 'telstra1.0'
            }
        }
    }
}
}


