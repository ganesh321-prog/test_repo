pipeline{
    agent any
    
    environment{
        PATH = "/opt/maven3/bin:$PATH"
    }
    stages{
        stage("Git Checkout"){
            steps{
                git credentialsId: 'Git-creds', url: 'https://github.com/ganesh321-prog/test_repo.git'
            }
        }
        stage("Maven Build"){
            steps{
                sh "mvn clean package"
                sh "mv target/*.war target/myweb.war"
            }
        }
        stage("deploy-dev"){
            steps{
                sshagent(['tomcat-new']) {
                sh """
                    scp -o StrictHostKeyChecking=no target/myweb.war  ec2-user@3.110.85.169:/root/apache-tomcat-9.0.79/webapps/
                
                    ssh ec2-user@3.110.85.169 /root/apache-tomcat-9.0.79/bin/shutdown.sh
                    
                    ssh ec2-user@3.110.85.169 /root/apache-tomcat-9.0.79/bin/startup.sh
                """
            }
            
            }
        }
    }
}
