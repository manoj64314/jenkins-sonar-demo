pipeline {
    agent any
    tools {
        maven 'maven_binary'
    }
    stages{
        
        stage('checkout'){
            steps{
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/manoj64314/jenkins-sonar-demo.git']])
            }    
        }
        stage('Build'){
            steps{
                echo 'Building Maven project'
                sh 'mvn clean compile'
            }
        }
        stage('Test'){
            steps{
                echo 'Building Maven project'
                sh 'mvn test'
                junit '**/target/surefire-reports/*.xml'
                jacoco classPattern: '**/target/classes', exclusionPattern: '**/*Test*.class', execPattern: '**/target/jacoco.exec', inclusionPattern: '**/*.class', sourceExclusionPattern: 'generated/**/*.java', sourceInclusionPattern: '**/*.java'
            }
        }
        stage('SonarQube Analysis'){
            steps{
                echo 'Scanning Maven project'
                withCredentials([string(credentialsId: 'sonar-id', variable: 'SONAR_TOKEN')]) {
                    withSonarQubeEnv(installationName: 'sonarcloud', credentialsId: 'sonar-id') { 
                        sh 'mvn sonar:sonar -Dsonar.projectKey=manoj_sonarreport -Dsonar.organization=vcjain -Dsonar.host.url=https://sonarcloud.io'
                        sh 'sleep 50'
                        
                    }
                }    
            }
        }
    }
        

}
