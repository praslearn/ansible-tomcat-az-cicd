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
       stage("Quality Gate result") {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    // Parameter indicates whether to set pipeline to UNSTABLE if Quality Gate fails
                    // true = set pipeline to UNSTABLE, false = don't
                    waitForQualityGate abortPipeline: true
                }
            }
        }
                     stage('Owasp Dependency check') {
           steps {
               dependencyCheck additionalArguments: '--format HTML', odcInstallation: 'my-dpcheck'
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
              stage('OWASP DAST') {
           steps {
               sh '''
                docker pull owasp/zap2docker-stable
                docker run -dt --name owasp owasp/zap2docker-stable sh
                docker exec owasp mkdir /zap/wrk
                docker exec owasp zap-baseline.py -t http://65.0.176.60:8090/hello-world-maven/ -x report.xml -I
                echo $WORKSPACE
                docker cp owasp:/zap/wrk/report.xml $WORKSPACE/report.xml
                docker stop owasp && docker rm owasp
                
               '''
            }
        }
    }
/*    post { 
        always { 
            cleanWs()
        }
    }*/
}
