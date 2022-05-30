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
  HOSTS = "${getHostForEnvironment}"

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
        withCredentials([sshUserPrivateKey(credentialsId: 'bitglass-key', keyFileVariable: 'BITGLASS_KEY', usernameVariable: 'BITGLASS_USER')]) {
           sh '''
        ansible-playbook $PLAYBOOK -i $HOSTS \
        --private-key=$BITGLASS_KEY \
        -e "ansible_user=$BITGLASS_USER" -vvv
        '''
        }
      }
    }
  }
  post{
    always{
      cleanWs()
    }
  }
}
def getHostForEnvironment(){
  if(env.BRANCH == 'master'){
    string host = params.ENVIRONMENT.toLowerCase()+'.hosts'
    return host
  }
}
