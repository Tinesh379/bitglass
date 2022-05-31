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
}

  stages{
    stage('initial setup'){
      steps{
        sh ' ssh -V '
        sh 'ls -altr'
      }
    }
    stage('Run Playbook'){
      when{
        expression {params.RUN_PLAYBOOKS}
      }
      steps{
       withCredentials([string(credentialsId: 'TEMP_KEY', variable: 'BITGLASS_KEY')]) {
           sh '''
           echo $BITGLASS_KEY > midkey.pem \
           cat midkey.pem | base64 -d >> outkey.pem \
           chmod 700 outkey.pem \
        ansible-playbook $PLAYBOOK -i dev.hosts \
        --private-key=outkey.pem \
        -e "ansible_user=ubuntu" -vvv
        '''
        }
      }
    }
  }
}

