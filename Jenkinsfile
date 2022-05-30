pipeline{
  agent{ label 'linux' }
  environment{
    ANIBLE_CREDS=credentials('ANSIBLE')
  }
  stages{
    stage('initial setup'){
      steps{
        echo "Copy SSH Key to Working Directory"
        echo "$ANSIBLE_CREDS_PSW >> ansible.pem"
        echo "change permissions on SSH key"
        chmod 600 ansible.pem
        sh ' ssh -V '
        sh 'ls -altr'
      }
    }
    stage('Run Playbook'){
      steps{
        sh '''
        ansible-playbook ping.yml -i hosts \
        --private-key=ansible.pem \
        -e "ansible_user=$ANSIBLE_CREDS_USR" -vvv
        '''
      }
    }
  }
}
