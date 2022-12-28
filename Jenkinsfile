pipeline {
    agent any
    tools { 
        maven 'mymaven' 
        jdk 'myjdk' 
    }
     
    stages {
      stage('checkout') {
           steps {
             
                git branch: 'main', url: 'https://github.com/MusthafaMashkura/jenkins-cicd-ansible.git'
             
          }
        }
      stage('Code Analysis') {
           steps {
                withSonarQubeEnv('mysonarqube')
               {
                   sh 'mvn sonar:sonar'
               }
           }
         }
      stage('Build') {
           steps {
               sh 'mvn clean test package'
            }
        }
      stage('Check Ansible version') {
           steps {
               sh 'ansible --version'
               sh 'python --version'
            }
        }
      stage('Ansible Deploy') {
           steps {
               sh 'ls -ltrh'
               sh 'ansible-playbook -i localhost myfirstplaybook.yml'
            }
        }
    }
/*    post { 
        always { 
            cleanWs()
        }
    }*/
}
