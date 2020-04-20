pipeline {
    agent any
      stages {
        stage ('Compile Stage') {

            steps {
                withMaven(maven : 'apache-maven-3.6.0') {
                    sh ' mvn clean compile'
                }
            }
        }
        stage ('Testing Stage') {

            steps {
                withMaven(maven : 'apache-maven-3.6.0') {
                    sh 'mvn test'
                }
            }
        }



        stage ('Install Stage') {
            steps {
                withMaven(maven : 'apache-maven-3.6.0') {
                    sh 'mvn install'
                }
            }
        }

         stage ('SonarQube analysis') {
             environment {
             def scannerHome = tool 'sonarscanner'
             }
             steps {
                withSonarQubeEnv('jenkinsonar') {
                sh 'mvn clean package sonar:sonar'
             }
            }
           }
         }

		stage ('Tomcat deploy'){
				steps {
				sh 'deploy adapters: [tomcat9(credentialsId: 'Tomcat', path: '', url: 'http://34.232.196.28/manager/html')], contextPath: '/var/lib/jenkins/workspace/Build.job-Web_deploy/target/', war: '**/*.war''
				}
		}
}
