pipeline{
  agent{ label 'linux' }
  environment{
    ANSIBLE_TOKEN=credentials('ANSIBLE_KEY')
    ANSIBLE_CREDS=credentials('ANSIBLE')
  }
  parameters {
  credentials credentialType: 'com.cloudbees.jenkins.plugins.sshcredentials.impl.BasicSSHUserPrivateKey', defaultValue: '', name: 'bitglass', required: false
}

  stages{
    stage('initial setup'){
      steps{
        sh 'touch ansible.pem'
        sh'ls -altr'
        echo "Copy SSH Key to Working Directory" 
       sh 'echo "$ANSIBLE_TOKEN>$WORKSPACE/ansible.pem" '
       echo "change permissions on SSH key"
       sh 'chmod 600 $WORKSPACE/ansible.pem'
        sh ' ssh -V '
        sh 'ls -altr'
      }
    }
    stage('Run Playbook'){
      steps{
        sh '''
        ansible-playbook ping.yml -i hosts \
        -e "ansible_user=ubuntu \
        ansible_user=$ANSIBLE_CREDS_USR
        ansible_password=$ANSIBLE_CREDS_PSW
        " -vvv
        '''
      }
    }
  }
}
