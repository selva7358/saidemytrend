def registry = "https://trial2o677a.jfrog.io"

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
  stage("Jar Publish") {                
    steps {                           
      script {                      
        echo '<--------------- Jar Publish Started --------------->'  
         def server = Artifactory.newServer url: registry + "/artifactory", credentialsId: "frog-cred"  
         def properties = "buildid=${env.BUILD_ID},commitid=${GIT_COMMIT}"  
         def uploadSpec = """{
                          "files": [
                            {
                              "pattern": "jarstaging/(*)",
                              "target": "selva-libs-release-local/{1}",
                              "flat": "false",
                              "props": "${properties}",
                              "exclusions": [ "*.sha1", "*.md5"]
                            }
                         ]
                     }"""  
                                              
         def buildInfo = server.upload(uploadSpec)  
         buildInfo.env.collect()  
         server.publishBuildInfo(buildInfo)  
         echo '<--------------- Jar Publish Ended --------------->'  
                                              
      }                             
    }                                 
  }                                     
 }                                         
}


