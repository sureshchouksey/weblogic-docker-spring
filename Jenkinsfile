import java.text.SimpleDateFormat
import groovy.transform.Field
import java.util.regex.Matcher
import java.util.regex.Pattern
import java.nio.file.*;

def  weblogicPortNumber = 'Unknown'

pipeline {    
    agent any
    stages {
        
        stage ('Clone') {

            steps {                
                    sh 'mvn clean install -Dmaven.test.skip=true'
            }
        }
        stage ('Build Docker') {
            steps {
                sh 'docker pull ramana152/weblogic:11g'
            }
        }
        stage ('Run Docker') {
            
            steps {
                echo "${env.BRANCH_NAME}"
                echo "${currentBuild.number}"
                //bat ("docker stop spring${env.BRANCH_NAME}${currentBuild.number}")
                //bat ("docker rm spring${env.BRANCH_NAME}${currentBuild.number}")
                sh ("docker run --name weblogic${env.BRANCH_NAME}${currentBuild.number} -d -p 7001 ramana152/weblogic:11g")
                sh ("docker cp /var/lib/jenkins/workspace/weblogic-multibranch_master/gameoflife-web/target/websocket-demo-0.0.1-SNAPSHOT.war weblogic${env.BRANCH_NAME}${currentBuild.number}:/u01/oracle/weblogic/user_projects/domains/base_domain/autodeploy")
            }
        }

         stage('Set Weblogic Port') {
            steps {
                script {
                    weblogicPortNumber = sh(returnStdout: true, script: "docker ps|grep weblogic${env.BRANCH_NAME}${currentBuild.number}|sed 's/.*0.0.0.0://g'|sed 's/->.*//g'")
                    echo 'Weblogic port number: ' + weblogicPortNumber
                }
            }
        }

       
    }
}
