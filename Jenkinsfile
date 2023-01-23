pipeline{
    agent any
    tools {
        maven 'Maven' 
    }
    stages{
        stage("TEST"){
            steps{
                sh 'mvn test'
            }
        }
        stage("Build"){
            steps{
                sh 'mvn package'
            }
        }
        stage("deployOnTest"){
            steps{
                deploy adapters: [tomcat7(credentialsId: 'tomcat8details', path: '', url: 'http://192.168.29.22:8080/')], contextPath: '/app', onFailure: false, war: '**/*.war'
            }
        }
        stage("deployOnPROD"){\
             input {
                message 'continueToPROD'
            }
            steps{
                deploy adapters: [tomcat7(credentialsId: 'tomcat8details', path: '', url: 'http://192.168.29.33:8080/')], contextPath: '/app', onFailure: false, war: '**/*.war'
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
