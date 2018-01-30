pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: 'localhost', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '18.219.75.76', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
     }

stages{
        stage('Build'){
            steps {
                bat 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        bat 'copy "C:/Program Files (x86)/Jenkins/workspace/FullyAutomated/webapp/target/webapp.war" d:/software/apache-tomcat-8.5.24-staging/webapps'
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        bat 'copy "C:/Program Files (x86)/Jenkins/workspace/FullyAutomated/webapp/target/webapp.war" ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps'
                    }
                }
            }
        }
    }
}