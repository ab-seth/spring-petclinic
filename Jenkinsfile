pipeline {
  agent any
  tools {
    maven 'Maven'
}
   options {
    timestamps()
  }

  stages {
    stage('Build') {
        steps {
          echo "========== Building Stage Starts =========="
          sh 'mkdir -p target'
          sh 'mvn clean install -X'
          sh 'ls -la target'
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
