pipeline{
    agent any
    tools {
        maven 'Maven' 
    }
    stages{
        stage("TEST"){
            steps{
                sh 'mvn test'
                slackSend channel: 'devops-notifications', message: 'this is TEST stage $BUILD_ID'
            }
        }
        stage("Build"){
            steps{
                slackSend channel: 'devops-notifications', message: 'this is BUILD stage'
                sh 'mvn package'
                nexusArtifactUploader artifacts: [[artifactId: '${POM_ARTIFACTID}', classifier: '', file: 'target/hello-world-0.0.1-SNAPSHOT.war', type: '${POM_PACKAGING}']], credentialsId: 'adminnexus', groupId: '${POM_GROUPID}', nexusUrl: '192.168.29.42:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'simpleapp-release', version: '${POM_VERSION}'
            }
        }
        stage("deployOnTest"){
            steps{
                slackSend channel: 'devops-notifications', message: 'this is deployOnTest stage'
                deploy adapters: [tomcat7(credentialsId: 'tomcat8details', path: '', url: 'http://192.168.29.22:8080/')], contextPath: '/app', onFailure: false, war: '**/*.war'
            }
        }
        stage("deployOnPROD"){\
             input {
                message 'continueToPROD'
            }
            steps{
                slackSend channel: 'devops-notifications', message: 'this is deployOnPROD stage'
                deploy adapters: [tomcat7(credentialsId: 'tomcat8details', path: '', url: 'http://192.168.29.42:8081/repository/simpleapp-release/')], contextPath: '/app', onFailure: false, war: '**/*.war'
            }
        }
    }
    post{
        always{
            echo "========always========"
        }
        success{
            echo "========pipeline executed successfully ========"
        }
        failure{
            echo "========pipeline execution failed========"
        }
    }
}
