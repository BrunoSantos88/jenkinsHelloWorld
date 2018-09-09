pipeline {
    agent any

    parameters {
		string(name: 'certificate_path', defaultValue: '/home/ailtonmsj/work/certificado/tomcat-jenkins.pem', description: 'Certificate Path')
        string(name: 'tomcat_dev', defaultValue: 'ec2-34-219-5-59.us-west-2.compute.amazonaws.com', description: 'Dev Server')
        string(name: 'tomcat_qa', defaultValue: 'ec2-34-220-166-119.us-west-2.compute.amazonaws.com', description: 'QA Server')
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
                    archiveArtifacts artifacts: 'target/*.war'
                }
            }
        }

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Dev'){
                    steps {
                        sh "scp -i /home/ailtonmsj/work/certificado/tomcat-jenkins.pem /var/lib/jenkins/workspace/Teste-Pipeline-C2/target/JenkinsHelloWorld.war ubuntu@ec2-34-219-5-59.us-west-2.compute.amazonaws.com:/var/lib/tomcat7/webapps"

                    }
                }

                stage ("Deploy to QA"){
                    steps {
                        sh "scp -i /home/ailtonmsj/work/certificado/tomcat-jenkins.pem /var/lib/jenkins/workspace/Teste-Pipeline-EC2/target/JenkinsHelloWorld.war ubuntu@ec2-34-220-166-119.us-west-2.compute.amazonaws.com:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}