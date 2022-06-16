pipeline {
  agent { label 'maven' }
  stages {
      //if build is already available start a new build else create a new application
    stage('Create application') {
      steps { 
          script{
         sh'''
         oc project test-java
         '''
            openshift.withCluster() {
                openshift.withProject() {
             if (openshift.selector("bc", "springboot-apps").exists()) { 
                    echo "build is available"
                    sh '''
                    
                    '''
                  }
            else{
                echo "build is not available"
            }          
          }
}
}
      }
    }
  }
}
