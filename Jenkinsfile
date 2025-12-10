pipeline {
 agent any
 environment {
  PATH="/opt/maven/bin:$PATH"
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
    scannerHome=tool "SonarQube-scanner"
  }
   steps {
    withSonarQubeEnv("SonarQube-server") {
     sh "${scannerHome}/bin/sonar-scanner"
    }
   }
  }
 }
}
