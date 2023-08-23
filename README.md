# ansible-config-mgt-class
#![image](https://user-images.githubusercontent.com/29310552/168498360-dd72b129-0e99-4522-bd3b-366ee660cbf6.png)
![image](https://github.com/Obasoro/ansible-config-mgt-class/assets/29310552/4298808d-6fb5-487d-abdb-95b254c5faab)


Retooling

# Devops/MlOps are tools for the future

# Retooling and Upskilling
Running my code

```
 pipeline {
    agent any

  stages {
    stage("Initial cleanup") {
      steps {
        dir("${WORKSPACE}") {
          deleteDir()
        }
      }
    }
    stage('Build') {
      steps {
        script {
          sh 'echo "Building Stage"'
        }
      }
    }

    stage('Test') {
      steps {
        script {
          sh 'echo "Testing Stage"'
        }
      }
    }
    stage('Package') {
      steps {
        script {
          sh 'echo "Packaging App"'
        }
      }
    }
    stage('Deploy') {
      steps {
        script {
          sh 'echo "Deploying Stage"'
        }
      }
    }
    stage('Clean Up') {
      steps {
        cleanWs()
        }
      }
    }
    
}
```

```
 pipeline {
  agent any
  environment {
    ANSIBLE_CONFIG="${WORKSPACE}/deploy/ansible.cfg"
  }
  stages {
    stage("Initial CleanUp") {
      steps {
        dir("${WORKSPACE}") {
          deleteDir()

        }
      }
    }
    stage("Checkout SCM") {
      steps{
        git branch: "feature/jenkinspipeline-stages", url: "https://github.com/obasoro/ansible-config-mgt-class.git"
      }
    }
    stage("Prepare Ansible for Execution") {
      steps {
        sh 'echo ${WORKSPACE}'
        sh 'sed -i "3 a roles_path=${WORKSPACE}/roles" ${WORKSPACE}/deploy/ansible.cfg'
      }
    }
    stage("Run Ansible playbook") {
      steps {
        ansiblePlaybook become: true, colorized: true, credentialsId: 'private-key', disableHostKeyChecking: true, installation: 'ansible', inventory: 'inventory/dev.yml', playbook: 'playbooks/site.yml'
      }
    }

    stage("Clean Workspace after Building") {
      steps {
        cleanWs(cleanWhenAborted: true, cleanWhenFailure: true, cleanWhenNotBuilt: true, cleanWhenUnstable: true, deleteDirs: true)
      }
    }
    
  }
 }
 ```
