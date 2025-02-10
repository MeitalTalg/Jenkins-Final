pipeline {
    agent any

    environment {
        ANSIBLE_PLAYBOOK_CREATOR = './ansible/create-infra.yaml'
        ANSIBLE_PLAYBOOK_DESTORY = './ansible/destroy-infra.yaml'
        ANSIBLE_INVENTORY = 'ansible/inventory'
    }

  
    stages {
        stage('Build - Run Ansible create-infra.yaml Playbook for run the app in continer') {
          steps {
            ansiblePlaybook(
              installation: 'Ansible',
              playbook: "${ANSIBLE_PLAYBOOK_CREATOR}",
              inventory: "${ANSIBLE_INVENTORY}"
              )
            }
          }
      
    stage('test') {
        steps {
            script {
                echo "test"
                sh 'docker exec web curl http://localhost:5000'
            }
        }
    }

      stage('Deploy') {
          steps {
              script {
                  echo "deploy to prodaction"
                  

                }
            }
        }
      
    }
    post { 
        always { 
            echo 'Run Ansible create-infra.yaml Playbook for delete the continer'
             ansiblePlaybook(
              installation: 'Ansible',
              playbook: "${ANSIBLE_PLAYBOOK_DESTORY}",
              inventory: "${ANSIBLE_INVENTORY}"
              )           
        }
     }
}  
