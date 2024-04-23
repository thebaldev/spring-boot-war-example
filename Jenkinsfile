pipeline {
    agent any
     tools {
        maven 'Maven' 
        slackSend channel: 'test', message: 'Job Started'
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
                
            }
            
        }
        stage("Deploy on Test"){
            steps{
                // deploy on container -> plugin
                deploy adapters: [tomcat9(credentialsId: 'tomcatserverdetails1', path: '', url: 'http://34.68.64.209:8080')], contextPath: '/app', war: '**/*.war'
              
            }
            
        }
        stage("Deploy on Prod"){
             input {
                message "Should we continue?"
                ok "Yes we Should"
            }
            
            steps{
                // deploy on container -> plugin
                deploy adapters: [tomcat9(credentialsId: 'tomcatserverdetails1', path: '', url: 'http://34.41.46.7:8080')], contextPath: '/app', war: '**/*.war'

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
