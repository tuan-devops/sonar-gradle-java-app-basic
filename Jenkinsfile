pipeline {
    agent any
    
    tools {
        jdk 'jdk17'
    }
    
    triggers {
        githubPush()
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/tuan-devops/sonar-gradle-java-app-basic'
            }
        }
        
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    echo 'Running SonarQube'
                    sh 'chmod +x ./gradlew && ./gradlew sonar'
                }
            }
        }
        
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew clean build --no-daemon'
            }
            
            post {
                success {
                    archiveArtifacts artifacts: 'dist/*.zip', allowEmptyArchive: true
                }
            }
        }
    }
}