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
                // Note: You are running mvn clean package again here; 
                // typically, you only need the ansible command if the build stage succeeded.
                sh 'ansible-playbook ansible/playbook.yml -i ansible/hosts.ini'
            }
        }
    }
}
