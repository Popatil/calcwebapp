node {
   def mvnHome
   stage('Git checkout') { 
      git 'https://github.com/Popatil/calcwebapp.git'
      mvnHome = tool 'maven'
   }
   stage('Maven-Compile') {     
      if (isUnix()) {
         sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean package"
      } else {
         bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean package/)
      }
   }
   stage('Unit tests') {
	  bat(/"${mvnHome}\bin\mvn" test/)
      junit '**/target/surefire-reports/TEST-*.xml'
   }
   stage('Deploy') {
    bat label: '', script: 'copy "target\\calcwebapp.war" D:\\apache-tomcat-7.0.92\\webapps\\'
   }
    

      mail bcc:'', body:'''Hi, Please find the job status as below

      Thanks

      Jenkins Admin''', cc:'', from:'', replyTo:'', subject:'Jenkins Job Status', to:'geeksgit@gmail.com,pooja.patiln@gmail.com'


}
