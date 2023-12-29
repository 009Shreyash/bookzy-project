pipeline{
    
    agent any
    
    tools{
        maven "maven"
    }
    
    stages{
        stage("Git Clone"){
            steps{
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/009Shreyash/Bookzy_SL.git']])   
            }
        }
        stage("Test"){
            steps{
                sh 'mvn test'
            }
        }
        stage("Build"){
            steps{
                sh 'mvn clean install'
            }
        }
         stage ('Install'){
            steps {
                archiveArtifacts artifacts: 'target/*.war'
            }
        }
       
        stage('DeployToTomcat'){
            steps{
                deploy adapters:[tomcat9(credentialsId: "TomcatCreds" , path: "", url:"http://192.168.0.75:8000/")], 
                contextPath: 'counterwebapp', 
                war: "target/*.war"
            }
        }
        
         stage('NotifyByEmail'){
            steps{
                emailext(
                        subject:"Jenkins Pipeline completed!",
                        body:"Bookzy Project using Jenkins pipline job done successfully",
                        to:"009shreyashraiyani@gmail.com")
            }
        }
     
   
    }
    
     post {
            success {
                junit checksName: 'Testss', testResults: '**/target/surefire-reports/TEST-*.xml'
                archiveArtifacts 'target/*.war'
                }
            }
       
}
