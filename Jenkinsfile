pipeline{
  agent{ label 'linux' }
  environment{
    BITGLASS=credentials('bitglass-key')
    ANSIBLE_CREDS=credentials('ANSIBLE')
  }

  stages{
    stage('initial setup'){
      steps{
        sh ' ssh -V '
        sh 'ls -altr'
      }
    }
    stage('Run Playbook'){
      steps{
        sh '''
        ansible-playbook ping.yml -i hosts \
        -e "ansible_ssh_user=$BITGLASS_USR ansible_ssh_private_key=$BITGLASS_PSW" -vvv
        '''
      }
    }
  }
}
