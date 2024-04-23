pipeline {
    agent any
     tools {
        maven 'Maven' 
        }
    stages {
        stage("Test"){
            steps{
                // Maven test
               
                sh "mvn test"
                slackSend channel: 'test', message: 'Job Started'
                
            }
            
        }
        stage("Build"){
            steps{
                sh "mvn package"
                slackSend channel: 'test', message: 'Job Build'
                
            }
            
        }
        stage("Deploy on Test"){
            steps{
                // deploy on container -> plugin
                slackSend channel: 'test', message: 'Job deploy on container'
                deploy adapters: [tomcat9(credentialsId: 'tomcatserverdetails1', path: '', url: 'http://35.225.37.137:8080')], contextPath: '/app', war: '**/*.war'                                                     
                echo "========pipeline executed successfully ========"
            }
            
        }
        stage("Deploy on Prod"){
             input {
                message "Should we continue?"
                ok "Yes we Should"

            }
            
            steps{
                // deploy on container -> plugin
                slackSend channel: 'test', message: 'Job prod'
                deploy adapters: [tomcat9(credentialsId: 'tomcatserverdetails1', path: '', url: 'http://34.27.4.180:8080')], contextPath: '/app', war: '**/*.war'                                                     
                echo "========pipeline executed successfully ========"

            }
        }
    }
    post{
        always{
            echo "========always========"
        }
        success{
            echo "========pipeline executed successfully ========"
             slackSend channel: 'test', message: 'Success'
        }
        failure{
            echo "========pipeline execution failed========"
             slackSend channel: 'test', message: 'Job Failed'
        }
    }
}
