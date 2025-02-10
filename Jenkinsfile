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
                  sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 992382545251.dkr.ecr.us-east-1.amazonaws.com'
                  sh 'docker pull 992382545251.dkr.ecr.us-east-1.amazonaws.com/meital-repo:latest:latest'
                  sh 'docker run -d -p 80:80 992382545251.dkr.ecr.us-east-1.amazonaws.com/meital-repo:latest:latest'
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
