pipeline {
    agent any
    environment {
      manifest_file = "ddkndksndksn"
      //BUILD_NUMBER = "${params.BUILD_NUMBER}"
    }
   stages {
    	stage('run-parallel-branches') {
            steps {
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
}
