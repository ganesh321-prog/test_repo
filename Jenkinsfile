pipeline{
    agent any
    
    environment{
        PATH = "/opt/maven3/bin:$PATH"
    }
    stages{
        stage("Git Checkout"){
            steps{
               git credentialsId: 'ganesh321-prog', url: 'https://github.com/ganesh321-prog/test_repo.git'
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
                    scp -o StrictHostKeyChecking=no target/myweb.war  ec2-user@172.31.32.48:/root/apache-tomcat-9.0.78/webapps/
                    
                    ssh ec2-user@172.31.32.48 /root/apache-tomcat-9.0.78/bin/shutdown.sh
                    
                    ssh ec2-user@172.31.32.48 /root/apache-tomcat-9.0.78/bin/startup.sh
                
                """
            }
            
            }
        }
    }
}
