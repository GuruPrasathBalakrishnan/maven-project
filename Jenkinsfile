pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '13.59.12.99', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '52.14.100.152', description: 'Production Server')
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
                        sh "scp -i /c/Program Files (x86)/Jenkins/tomcatdemo.pem **/target/*.war ec2-user@$13.59.12.99:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
						sh "scp -i /c/Program Files (x86)/Jenkins/tomcatdemo.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}