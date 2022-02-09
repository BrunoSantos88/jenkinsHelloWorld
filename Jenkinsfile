pipeline {
    agent any

    parameters {
        string(name: 'artifact_path', defaultValue: '/var/lib/jenkins/workspace/Teste-Pipeline-EC2/target/*.war')
		string(name: 'certificate_path', defaultValue: '/var/lib/jenkins/certificate/tomcat-jenkins.pem', description: 'Certificate Path')
        string(name: 'tomcat_dev', defaultValue: 'ec2-54-208-43-229.compute-1.amazonaws.com', description: 'Dev Server')
        string(name: 'tomcat_qa', defaultValue: 'ec2-54-208-43-229.compute-1.amazonaws.com', description: 'QA Server')
    }

    triggers {
        pollSCM('* * * * *')
     }

stages{
        stage('Build'){
            steps {
                sh "mvn clean package"
            }
            post {
                success {
                    echo "Now Archiving..."
                    archiveArtifacts artifacts: "target/*.war"
                }
            }
        }

        stage ('Deployments'){
            parallel{
                stage ("Deploy to Dev"){
                    steps {
                        sh "scp -i ${params.certificate_path} ${params.artifact_path} ubuntu@${params.tomcat_dev}:/var/lib/tomcat7/webapps"

                    }
                }

                stage ("Deploy to QA"){
                    steps {
                        sh "scp -i ${params.certificate_path} ${params.artifact_path} ubuntu@${params.tomcat_qa}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}
