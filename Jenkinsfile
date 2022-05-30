pipeline{
  agent{ label 'linux' }
  parameters {
  password defaultValue: '', name: 'BITGLASS_SSH_PRIVATE_KEY'
}
  environment{
    BITGLASS=credentials('bitglass-key')
    ANSIBLE_CREDS=credentials('ANSIBLE')
    ACCESS_KEY = "{params.BITGLASS_SSH_PRIVATE_KEY}"
  }
  
  stages{
    stage('initial setup'){
      steps{
        sh ' ssh -V '
        sh 'ls -altr'
        sh '''
        touch ansible.pem
        echo "$ACCESS_KEY" > ansible.pem
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
        -e "ansible_ssh_user=$BITGLASS_USR ansible_ssh_private_key=$BITGLASS_PSW" -vvv
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
