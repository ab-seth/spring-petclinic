pipeline {
    agent any
    tools {
        maven 'Maven'
    }
    stages {
        stage('Build') {
            steps {
                withEnv(["JAVA_HOME=${tool 'JDK 17'}"]) {
                    sh 'echo $JAVA_HOME'
                    sh 'mvn clean install'
                }
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('PetClinic-SonarQube') {
                    withEnv(["JAVA_HOME=${tool 'JDK 17'}"]) {
                        sh 'mvn sonar:sonar'
                    }
                }
            }
        }
    }
}
