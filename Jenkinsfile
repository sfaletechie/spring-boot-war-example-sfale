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
                deploy adapters: [tomcat7(credentialsId: 'tomcat8details', path: '', url: 'http://192.168.29.42:8080')], contextPath: '/app', onFailure: false, war: '**/*.war'
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
