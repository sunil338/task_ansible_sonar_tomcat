pipeline {
    agent any


    stages {
        stage('Checkout') {
            steps {
                dir('devops_login') {
                    git branch: 'main',
                        url: 'https://github.com/sunil338/task_ansible_sonar_tomcat.git'
                }
            }
        }

        stage('Build') {
            steps {
                dir('devops_login') {
                    sh 'mvn clean install'
                }
            }
        }

       
        stage('Deploy to Tomcat') {
            steps {
                dir('ansible') { 
                        sh """
                        ansible-playbook -i inventory.ini playbook.yml -vvv
                        """
                    }
                }
            }    
        }
    }

