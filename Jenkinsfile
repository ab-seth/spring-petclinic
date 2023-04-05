pipeline {
    agent any
    tools {
        maven 'Maven'
    }
    environment {
        JAVA_HOME = "${tool 'JDK 17'}"
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('PetClinic-SonarQube') {
                    sh 'mvn sonar:sonar'
                }
            }
        }
    }
}
