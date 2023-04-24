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
        withCredentials([sshUserPrivateKey(credentialsId: 'my-ssh-key', keyFileVariable: 'SSH_KEY', passphraseVariable: '', usernameVariable: 'SSH_USER')]) {
          ansiblePlaybook(
            inventory: 'hosts.ini',
            playbook: 'deploy_petclinic.yml',
            installation: 'ansible',
            extraVars: [
              ssh_key_file: env.SSH_KEY,
              ssh_user: env.SSH_USER
            ]
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
