node{
  def mavenHome = tool name:"Maven_Jenkins"
  stage('1Clone'){
 git credentialsId: 'GitHUB_With_Token', url: 'https://github.com/Adanwam/maven-web-application'
  }
stage('2MavenBuild'){
    sh "${mavenHome}/bin/mvn clean package"
   // bat 'mvn package'
  }
stage('3CodeQualitty'){
    sh "${mavenHome}/bin/mvn sonar:sonar"
}
stage('4Upload2Artifacts'){
    sh "${mavenHome}/bin/mvn deploy"
}
 stage('6.Deploy2uat'){
        sshagent(['ec2-user_tomcat']) {
  }
stage('7.approval'){
      timeout(time:8, unit:'HOURS'){
        input message: 'Please approve deployment to Production'
      }
 stage('8.Deploy2prod'){
          sshagent(['ec2-user_tomcat']) {
        sh "scp -o StrictHostKeyChecking=no target/*.war ec2-user@172.31.92.0:/opt/tomcat9/webapps/app.war"      
    }  
    stage('9.EmailAlerts'){
        emailext body: '''Hi
        Build status for boa app.
        Regards,
        Landmark Technologies''', recipientProviders: [developers(), requestor()], subject: 'Project status', to: 'boa@gmail.com'
           }
        }
          }
     }
