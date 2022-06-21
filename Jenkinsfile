pipeline {
  agent { label 'maven' }
   environment {
        //TODO: Edit these vars as per your env
        PROJECT = "microservice-dev"
        STAGE_PROJECT = "microservice-stage"
        APP_GIT_URL = "https://github.com/mahsankhaan/springboot-openshift.git"
        NEXUS_SERVER = "http://nexus.upkar-openshift-seo01-b3c-162e406f043e20da9b0ef0731954a894-0000.seo01.containers.appdomain.cloud/repository/maven-public/"

        // DO NOT CHANGE THE GLOBAL VARS BELOW THIS LINE
        APP_NAME = "spring-app"
    }
  stages {
      //if build is already available start a new build else create a new application
    stage('Create application') {
      steps { 
          script{
        git url: "https://github.com/mahsankhaan/springboot-openshift.git", branch: "master"
         sh'''
         oc project test-java
         '''
            openshift.withCluster() {
                openshift.withProject() {
             if (openshift.selector("bc", "${APP_NAME}").exists()) { 
                    echo "build is available"
               sh'''     oc start-build  ${APP_NAME} --from-dir=. --follow --wait  --e MAVEN_MIRROR_URL=${NEXUS_SERVER}
               '''
                  }
            else{
                echo "build is not available"
                sh '''
                oc new-app --name ${APP_NAME} -e MAVEN_MIRROR_URL=${NEXUS_SERVER} java:openjdk-11-el7~${APP_GIT_URL}
                 oc expose svc/${APP_NAME}
           '''
            }          
          }
}
}
      }
    }
  }
}
