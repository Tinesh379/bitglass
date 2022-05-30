pipeline{
  agent{ label 'linux' }
   parameters {
  file description: 'upload private key to ansible to deploy', name: 'ansible.pem'
}

  environment{
    BITGLASS=credentials('bitglass-key')
    ANSIBLE_CREDS=credentials('ANSIBLE')
    ACCESS_KEY = "${params.BITGLASS_SSH_PRIVATE_KEY}"
  }
  
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
        sh '''
        ansible-playbook ping.yml -i hosts \
        --private-key=ansible.pem \
        -e "ansible_user=ubuntu" -vvv
        '''
      }
    }
  }
  post{
    always{
      cleanWs()
    }
  }
}
