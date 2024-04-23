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
                deploy adapters: [tomcat9(credentialsId: 'tomcatserverdetails112233', path: '', url: 'http://20.121.64.144:8080')], contextPath: '/app', war: '**/*.war'                                                     
                echo "========pipeline executed successfully ========"
            }
            
        }

            
         stage("Deploy on Prod"){
             steps{
                // deploy on container -> plugin
                slackSend channel: 'test', message: 'Job prod'
                deploy adapters: [tomcat9(credentialsId: 'tomcatserverdetails112233', path: '', url: 'http://20.121.65.139:8080')], contextPath: '/app', war: '**/*.war'                                                     
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
