pipeline {
    // A declarative pipeline
    agent any

     parameters {
        string (name: 'tomcat_dev', defaultValue:'18.216.188.151', description:'Staging env')
        string (name: 'tomcat_prod', defaultValue:'13.59.97.147', description:'Production env')
    }

    triggers {
        pollSCM('* * * * *')
    }

    tools {
        maven 'localMaven'
    }
    
    stages('Deployments'){
        stage('Build'){
            steps {
               sh 'mvn clean package'
            }

            post {
                success {
                    echo 'Now Archiving....'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }

        }
        
        stage('Deployments'){
            parallel(){
                stage('Deploy to Staging'){
                    steps {
                         sh "scp -i /home/projects/jenkins/jenkinspipeline/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }

                stage('Deploy to Production'){
                    steps{
                        sh "scp -i /home/projects/jenkins/jenkinspipeline/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }

            }
        }
          
    }
}
