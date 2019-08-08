pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '18.217.163.149', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '3.15.172.220', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
     }

stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
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
                        sh "cp -i /home/jenkins/tomcat-demo.pem **/target/*.war ubuntu@${params.tomcat_dev}:/var/lib/tomcat9/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "cp -i /home/jenkins/tomcat-demo.pem **/target/*.war ubuntu@${params.tomcat_prod}:/var/lib/tomcat9/webapps"
                    }
                }
            }
        }
    }
}
