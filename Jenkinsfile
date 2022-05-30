pipeline{
  agent{ label 'linux' }
  stages{
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
}
