pipeline {
    agent any
    
    tools {
        maven 'Maven'
    }

  triggers {
    githubPush()
  }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
 
        stage('sonarqube analysis') {
            steps {
                script {
                    // Get real installation path from Jenkins
                    def scannerHome = tool 'SonarQube'

                    withSonarQubeEnv(
                        installationName: 'SonarQube',
                        credentialsId: 'sqp_e2f821f24235e1d6f24e0e0106e7d4ed04b3dfc1'
                    ) {
                        sh """
                            ${scannerHome}/bin/sonar-scanner \
                            -Dsonar.projectKey=maven_app_codequality \
                            -Dsonar.projectName=maven_app_codequality
                        """
                    }
                }
            }
        }
        
        stage ('maven build war file'){
            steps {
                sh 'mvn clean package'
            }
        }
    }
        post {
            success {
                emailext(
                    subject: "SUCCESS: SonarQube qaulity gate passed",
                    body: '''
                    Hi Team,
                    The sonarqube quality test successfully passed.
                    http://98.81.215.99:9000/   
                    username: admin, password: pass
                    
                    Regards
                    Dev Team    
                    ''',
                    to: 'sushmitha.arivalgan@gmail.com'
                    )
            }
            failure {
                emailext(
                    subject: "Jenkins failed",
                    body: '''
                    Hi Team,
                    The sonarqube quality test successfully failed.
                    Please check for the details in the report
                    http://98.81.215.99:9000/  
                    ''',
                    to: 'sushmitha.arivalagan@gmail.com'
                    )
            }
        }
    }

