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
        withCredentials([sshUserPrivateKey(credentialsId: '4df15039-bafc-423c-acab-3159949be96d', keyFileVariable: 'SSH_KEY')]) {
          ansiblePlaybook(
            inventory: 'hosts.ini',
            playbook: 'deploy_petclinic.yml',
            installation: 'ansible',
            extras: '-e "ansible_ssh_private_key_file=$SSH_KEY"'
          )
        }
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
