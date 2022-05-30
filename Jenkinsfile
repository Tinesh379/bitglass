pipeline{
  agent{ label 'linux' }
  stages{
    stage('initial setup'){
      steps{
        sh ' ssh -V '
        sh 'ls -altr'
        sh '''
        chmod 600 ansible.pem
        ls -altr
        '''
      }
    }
    stage('Run Playbook'){
      steps{
        withCredentials([sshUserPrivateKey(credentialsId: 'bitglass-key', keyFileVariable: 'BITGLASS_KEY', usernameVariable: 'BITGLASS_USER')]) {
           sh '''
        ansible-playbook ping.yml -i hosts \
        --private-key=$BITGLASS_KEY \
        -e "ansible_user=$BITGLASS_USER" -vvv
        '''
        }
      }
    }
  }
  post{
    always{
      cleanWs()
    }
  }
}
