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
        
        stage("deploy-dev"){
            steps{
                sshagent(['tomcat-new']) {
                sh """
                    pwd
                    scp -o StrictHostKeyChecking=no target/myweb.war  ec2-user@3.110.85.169:/root/apache-tomcat-9.0.79/webapps/
                
                    ssh ec2-user@3.110.85.169 /root/apache-tomcat-9.0.79/bin/shutdown.sh
                    
                    ssh ec2-user@3.110.85.169 /root/apache-tomcat-9.0.79/bin/startup.sh
                """
            }
            
            }
        }
    }
}
