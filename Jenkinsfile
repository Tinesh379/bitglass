pipeline{
  agent{ label 'linux' }
  environment{
    ANSIBLE_CREDS=credentials('ANSIBLE')
  }
  stages{
    stage('initial setup'){
      steps{
        sh 'touch ansible.pem'
        sh'ls -altr'
        echo "Copy SSH Key to Working Directory" 
       sh 'echo "$ANSIBLE_CREDS_PSW>ansible.pem" '
        //echo "change permissions on SSH key"
       // sh 'chmod 600 $WORKSPACE/ansible.pem'
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
