pipeline {
  agent any

  stages {
    stage('Build') {
      steps {
        echo 'Building...'
        sh './mvnw clean install -DskipTests'
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
          credentialsId: 'jenkins-ansible'
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
