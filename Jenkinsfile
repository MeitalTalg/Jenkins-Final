pipeline {
    agent any

    environment {
        ANSIBLE_PLAYBOOK_CREATOR = 'ansible/create-infra.yaml'
        ANSIBLE_PLAYBOOK_DESTORY = 'ansible/destroy-infra.yaml'
        ANSIBLE_INVENTORY = 'ansible/inventory'
    }

  
    stages {
        stage('Run Ansible Playbook') {
            steps {
                    sh """
                    ansible-playbook -i ${ANSIBLE_INVENTORY} ${ANSIBLE_PLAYBOOK_CREATOR}
                    """
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
             sh """
                    ansible-playbook -i ${ANSIBLE_INVENTORY} ${ANSIBLE_PLAYBOOK_DESTORY}
                    """       
        }
     }
}  
