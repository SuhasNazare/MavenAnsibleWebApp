pipeline {
    agent any 

    environment {
        // Wrapped correctly to avoid the "Not a valid section definition" error
        LANG = 'en_US.UTF-8'
        LC_ALL = 'en_US.UTF-8'
    }

    tools {
        // Ensure 'Maven' matches the name in Manage Jenkins -> Global Tool Configuration
        maven 'Maven' 
    }

    stages {
        stage('Checkout') { 
            steps {
                git branch: 'main', url: 'https://github.com/SuhasNazare/MavenAnsibleWebApp.git'
            }
        }
        
        stage('Build') { 
            steps {
                sh 'mvn clean package' 
            }
        }

        stage('Archive') { 
            steps {
                archiveArtifacts artifacts: 'target/*.war', fingerprint: true
            }
        }

        stage('Deploy') {
            steps {
                // This will print the exact name of the .war file in your logs
                sh 'ls target/' 
                sh 'ansible-playbook ansible/playbook.yml -i ansible/hosts.ini'
            }
        }
    }
}
