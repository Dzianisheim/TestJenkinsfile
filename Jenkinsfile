pipeline {
    agent any

    properties([parameters([booleanParam(description: '', name: 'TEST2')]), pipelineTriggers([])])

    stages {
        stage('Preparation') {
            steps {
                echo "I'm on the first branch!"
            }
            
        }
        
        stage('Test') {
            steps {
                echo "I'm on the first branch!"
            }
        }
        
        stage('Publish') {
            steps {
                echo "I'm on the first branch!"
            } 
        }
    }
}