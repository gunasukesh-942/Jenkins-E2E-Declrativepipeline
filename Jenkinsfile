pipeline {
    agent any
  //environment {
        //SONAR_TOKEN = credentials ('16d3e8d1-3087-4b02-96d4-f0e9f827e971')
        //SONAR_TOKEN = credentials ('sonar id available in managejenkins > credentials > id ( genreated for the token while cofigured )')
   //}
  //withiout environment also this pipeline will work 
    tools {
        git "git"
        maven "maven"
        // git need to download in server (mandtory),we cant configure in Globaltool without in server (if we want we can remove from here(tools))
        // globaltoolconfiguration : managejenkins > tools > maveninstallations (name : maven version : 3.8.6 )
         
    }
    stages {
        stage ("cleaningworkspace") {
            steps {
                cleanWs()
                // cleaning workspace in jenkins default path
            }
        }
        stage('code') {
            steps {
                git branch: 'master', url: 'https://github.com/gunasukesh-942/one.git'
            }
        }
        stage('build') {
            steps {
                sh " mvn clean package"
            }
        }
        stage('SonarQube') {
            steps {
                script {
                    def mavenHome = tool name: "maven", type: "maven"
                    def mavenCMD = "${mavenHome}/bin/mvn"
                    withSonarQubeEnv("sonar") {
                        sh "${mavenCMD} sonar:sonar"
                        
                    }
                }
            }
        }
        stage('deploy') {
            steps {
                sshagent(['1ab572be-9e46-4d05-89e5-51b2fee743e3']) {
                    sh" scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/job-1/target/myweb-8.4.6.war ec2-user@172.31.5.148:/home/ec2-user/apache-tomcat-9.0.83/webapps/swigg.war"
                }
                
            }
        }
        
    }
}
