pipeline {
    agent any

    properties([
      parameters([
        string(name: 'submodule', defaultValue: ''),
        string(name: 'submodule_branch', defaultValue: ''),
        string(name: 'commit_sha', defaultValue: ''),
      ])
    ])


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