pipeline {
  agent any
  tools {
    maven 'Maven'
  }
  options {
    timestamps()
  }
  environment {
    JAR_FILE = ""
  }
  stages {
    stage('Build') {
        steps {
          echo "========== Building Stage Starts =========="
          sh 'mkdir -p target'
          sh 'mvn clean install -X'
          script {
            JAR_FILE = sh(script: "find target -type f -name 'spring-petclinic-*.jar'", returnStdout: true).trim()
          }
          echo "JAR_FILE: ${JAR_FILE}"
          stash includes: JAR_FILE, name: 'jar'
          echo "========== Building Stage Ends =========="
        }
    }

    stage('Test') {
      steps {
        echo 'Testing...'
        sh './mvnw test'
      }
    }

    stage('Deploy') {
      steps {
        echo 'Deploying...'
        unstash 'jar'
        ansiblePlaybook(
          inventory: 'hosts.ini',
          playbook: 'deploy_petclinic.yml',
          installation: 'ansible',
          credentialsId: 'random_id_update',
          extras: "-e jar_file=${JAR_FILE}"
        )
      }
    }
  }

  post {
    always {
      echo 'Running cleanup tasks...'
      sh './mvnw clean'
    }
  }
}
