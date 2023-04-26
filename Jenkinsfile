pipeline {
  agent any

  stages {
    stage('Build') {
        steps {
          echo 'Building...'
          sh 'mkdir -p target'
          sh 'mvn clean install'
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
        ansiblePlaybook(
          inventory: 'hosts.ini',
          playbook: 'deploy_petclinic.yml',
          installation: 'ansible',
          credentialsId: 'Jenkins_private_key_ID'
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
