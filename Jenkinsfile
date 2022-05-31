pipeline{
  agent{ label 'linux' }
  parameters {
  booleanParam description: 'enable to run playbooks', name: 'RUN_PLAYBOOKS'
  choice choices: ['DEV', 'UAT', 'PROD'], description: 'select environment to run playbook', name: 'ENVIRONMENT'
  string defaultValue: '', description: 'Enter Playbook Name', name: 'PLAYBOOK_NAME', trim: true
  string defaultValue: '<empty>', description: 'Enter RFC incase of Production Deployment', name: 'RFC', trim: true
}
environment{
  PLAYBOOK = "${params.PLAYBOOK_NAME}"
  BITGLASS = credentials('TEMP_KEY')
}

  stages{
    stage('initial setup'){
      steps{
        sh ' ssh -V '
        sh 'ls -altr'
        sh 'echo $BITGLASS > midkey.pem'
        sh 'base64  -di midkey.pem > outkey.pem'
        sh 'chmod 700 outkey.pem'
        sh 'ls -altr'
      }
    }
    stage('Run Playbook'){
      when{
        expression {params.RUN_PLAYBOOKS}
      }
      steps{
           sh '''
        ansible-playbook $PLAYBOOK -i dev.hosts \
        --private-key=outkey.pem \
        -e "ansible_user=ubuntu" -vvv
        '''
      }
    }
  }
}

