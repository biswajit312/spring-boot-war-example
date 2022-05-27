pipeline {
    agent any
     tools {
        maven 'Maven' 
        }
    stages {
        stage("Test"){
            steps{
                // mvn test
                sh "mvn test"
                slackSend channel: 'jobsnotification', message: 'Job Started'
                
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
                deploy adapters: [tomcat8(credentialsId: 'testserver', path: '', url: 'http://107.23.182.181:8080/')], contextPath: '/app', war: '**/*.war'
              
            }
            
        }
        stage("Deploy on Prod"){
             input {
                message "Should we continue?"
                ok "Yes we Should"
            }
            
            steps{
                // deploy on container -> plugin
                deploy adapters: [tomcat8(credentialsId: 'testserver', path: '', url: 'http://54.226.113.233:8080/')], contextPath: '/app', war: '**/*.war'

            }
        }
        stage("Code Coverage"){
            steps{
                sh 'mvn clean cobertura:cobertura'
                
            }
            
        }        
    }
    post{
        always{
            echo "========always========"
        }
        success{
            echo "========pipeline executed successfully ========"
             slackSend channel: 'jobsnotification', message: 'Success'
        }
        failure{
            echo "========pipeline execution failed========"
             slackSend channel: 'jobsnotification', message: 'Job Failed'
        }
    }
}
