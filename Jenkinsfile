pipeline {
 agent any
 environment {
  PATH="/opt/maven/bin"
 }
 stages {
  stage("build") {
   steps {
    sh "mvn clean deploy -Dmaven.test.skip=true"
   }
}

  stage("tets") {
   steps {
    echo " testing started"
    sh "mvn surefire-report:report"
    echo "testing ending"
   }
 }

  stage("SonarQube analysis") {
   
   environment {
    ScannerHome=tool "SonarQube-scanner"
  }
   steps {
    withSonarQubeEnv("SonarQube-server") {
     sh "${scannerHome}/bin/sonar-scanner"
    }
   }
  }

  stage("Quality gate") {
   steps {
    script{
     timeout(time: 1 , unit:"HOURS") {
      def qg = waitFORQualityGate()
      if( status.qg != 'OK") {
       error "Pipeline aborted : ${status.qg}"
      }
     }
    }
   }
  }
 } 
}	    
