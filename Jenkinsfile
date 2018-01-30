pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: 'localhost', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: 'ec2-18-219-75-76.us-east-2.compute.amazonaws.com', description: 'Production Server')
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
                        bat 'pscp -i "D:/Download/tomcat-demo.ppk" "C:/Program Files (x86)/Jenkins/workspace/FullyAutomated/webapp/target/webapp.war" ec2-user@18.219.75.76:/var/lib/tomcat7/webapps -hostkey 2c:7c:3d:cc:cf:41:40:61:d5:7b:3f:68:5d:9d:11:67'
                    }
                }
            }
        }
    }
}