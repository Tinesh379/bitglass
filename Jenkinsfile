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
        sh '''
        echo "$BITGLASS_PSW"
        touch ansible.pem
        echo "$BITGLASS_PSW" > ansible.pem
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
}
