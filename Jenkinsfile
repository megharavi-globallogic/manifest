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
                    sh ''' jq -e  '(.vizio_module_release_details[] | .major_minor_version) = "${BUILD_NUMBER}"' manifest.json > manifest.json.tmp && mv manifest.json.tmp manifest.json
			
				        git add manifest.json
		                git diff-index --quiet HEAD || git commit -m "updated ${BUILD_NUMBER}"
				        git pull origin master
		                git push origin master
				        git push --force origin master
			    '''
           }
       }
    	
            
            
            
            
            
   }
}
