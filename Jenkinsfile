pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('sonar-token') // Jenkins secret text credentials
    }

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

        stage('SonarQube Analysis') {
            steps {
                dir('devops_login') {
                    withSonarQubeEnv('sonarqube') {
                        sh 'mvn sonar:sonar -Dsonar.projectKey=devops_git'
                    }
                }
            }
        }

        stage('Check Quality Gate') {
            steps {
                sh '''
                curl -s -u $SONAR_TOKEN: \
                'http://localhost:9000/api/qualitygates/project_status?projectKey=devops_login' \
                | grep -q '"status":"ERROR"' && echo 'Quality Gate FAILED' && exit 1 || echo 'Quality Gate PASSED'
                '''
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                dir('ansible') { 
                        sh """
                        ansible-playbook -i inventory.ini playbook.yml 
                        """
                    }
                }
            }    
        }
    }

