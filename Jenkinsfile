pipeline {
    agent any
    environment {
      manifest_file = "ddkndksndksn"
      //BUILD_NUMBER = "${params.BUILD_NUMBER}"
    }
    
   stages {
        stage('before') {
           steps {
               println("before")
           }
       }
    	stage('run-parallel-branches') {
                parallel(
                   stage('run1') {
                        steps {
                            echo "This is branch a"
                        }
                   }
                    stage('run2') {
                        steps {
                            echo "This is branch b"
                        }
                    }
               )
        }
            
            
            
            
            
   }
}
