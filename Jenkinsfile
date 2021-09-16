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
                    a: {
                        echo "This is branch a"
                    },
                    b: {
                        echo "This is branch b"
                    }
                )
            }
        }
            
            
            
            
            
   }
}
