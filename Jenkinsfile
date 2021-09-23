def generateStage(name, id, source_repo_params) {
	return {
		
	   	id="${id}".toInteger()
		println "======(id): ${id} ==(name): ${name} ======="
		
		
		checkout_additional_parameters = splitString3("${checkout_additional_parameters}")
		checkout_git_repo_url = splitString("${checkout_git_repo_url}")
		checkout_git_branch = splitString("${checkout_git_branch}")
		checkout_path = splitString("${checkout_path}")
		checkout_output_manifest_path = splitString("${checkout_output_manifest_path}")
		
		version_path = splitString("${version_path}")
		version_additional_parameters = splitString3("${version_additional_parameters}")
		version_generator = splitString("${version_generator}")
		artifactory_cred_generator = splitString("${artifactory_cred_generator}")
		version_output_path = splitString("${version_output_path}")
		generated_fw_inserted_version = splitString("${generated_fw_inserted_version}")
		generated_dlm_version = splitString("${generated_dlm_version}")
		
		build_additional_parameters = splitString3("${build_additional_parameters}")
		build_path = splitString("${build_path}")
		fw_inserted_build_output_pattern = splitString("${fw_inserted_build_output_pattern}")
		fw_insterted_build_output_path = splitString("${fw_insterted_build_output_path}")
		dlm_build_output_pattern = splitString("${dlm_build_output_pattern}")
		dlm_build_output_path = splitString("${dlm_build_output_path}")
		git_repo_path_for_rearranging_module = splitString("${git_repo_path_for_rearranging_module}")
		git_rearranging_module_repack_script_name = splitString("${git_rearranging_module_repack_script_name}")
		output_name_of_the_rearranged_module = splitString("${output_name_of_the_rearranged_module}")
		
		println "version_additional_parameters: ${version_additional_parameters}"
		println "git_repo_path_for_rearranging_module: ${git_repo_path_for_rearranging_module}"
		
		
		build job: "${name}-build", parameters: [
		
			string(name: 'module_name', value: "${name}"), 
			
			string(name: 'build_release', value: "${build_release}"), 
			string(name: 'build_type', value: "${build_type}"),
			string(name: 'repack_type', value: "${repack_type}"),
			string(name: 'manifest_foldername', value: "${manifest_foldername}"),
			
			string(name: 'getFur', value: "${getFur}"),
			string(name: 'chipset', value: "${chipset}"),
		
			string(name: 'checkout_additional_parameters', value: "${checkout_additional_parameters[id]}"), 
			string(name: 'checkout_git_repo_url', value: "${checkout_git_repo_url[id]}"),
			string(name: 'checkout_git_branch', value: "${checkout_git_branch[id]}"),
			string(name: 'checkout_path', value: "${checkout_path[id]}"), 
			string(name: 'checkout_output_manifest_path', value: "${checkout_output_manifest_path[id]}"), 
 
			string(name: 'source_repo_params', value: "${source_repo_params}"),

			string(name: 'version_path', value: "${version_path[id]}"),
			string(name: 'version_additional_parameters', value: "${version_additional_parameters[id]}"), 
			string(name: 'version_generator', value: "${version_generator[id]}"), 
			string(name: 'artifactory_cred_generator', value: "${artifactory_cred_generator[id]}"),
			string(name: 'version_output_path',  value: "${version_output_path[id]}"),
			string(name: 'generated_fw_inserted_version', value: "${generated_fw_inserted_version[id]}"), 
			string(name: 'generated_dlm_version', value: "${generated_dlm_version[id]}"),

			string(name: 'build_additional_parameters', value: "${build_additional_parameters[id]}"),
			string(name: 'build_path', value: "${build_path[id]}"),
			string(name: 'fw_inserted_build_output_pattern', value: "${fw_inserted_build_output_pattern[id]}"),
			string(name: 'fw_insterted_build_output_path', value: "${fw_insterted_build_output_path[id]}"),
			string(name: 'dlm_build_output_pattern', value: "${dlm_build_output_pattern[id]}"),
			string(name: 'dlm_build_output_path', value: "${dlm_build_output_path[id]}"),
			
			string(name: 'git_repo_firmware_repackage_name', value: "${git_repo_firmware_repackage_name}"),
			string(name: 'git_repo_path_for_rearranging_module', value: "${git_repo_path_for_rearranging_module[id]}"),
			string(name: 'git_rearranging_module_repack_script_name', value: "${git_rearranging_module_repack_script_name[id]}"),
			string(name: 'output_name_of_the_rearranged_module', value: "${output_name_of_the_rearranged_module[id]}")
		]
 	}
}



pipeline {
	agent any
	environment {
		manifest_path = "${params.manifest_path}"
		manifest = readJSON file: "../${params.manifest_path}"
		//manifest_path = "recipe-config-build/45-manifest/30x-master-recipe-1.42.json"
		//manifest = readJSON file: "/var/lib/jenkins/workspace/recipe-config-build/48-manifest/30x-master-recipe-1.42.json"
		
		//get from upstream
		repack_type = "${params.repack_type}"
		build_release = "${params.build_release}"
		build_type = "${params.build_type}"
		manifest_foldername = "${manifest_foldername}"

		
		//get release details
		fur_release = "${manifest.release_details.fur_release}"
		recipe_git_url = "${manifest.release_details.recipe_git_url}"
		recipe_path = "${manifest.release_details.recipe_path}"
		config_git_url = "${manifest.release_details.config_git_url}"
		config_path = "${manifest.release_details.config_path}"
		config_name = "${manifest.release_details.config_name}"
		repack_firmware_version = "${manifest.release_details.repack_firmware_version}"
		
		
		//get chipset details
		soc = "${manifest.chipset_details.soc}"
		model = "${manifest.chipset_details.model}"
		model_group = "${manifest.chipset_details.model_group}"
		chipset = "${manifest.chipset_details.chipset}"
		slack_channel = "${manifest.chipset_details.slack_channel}"
		
		//firmware repackage url
		git_repo_firmware_repackage = "${manifest.firmware_repackage.git_repo_firmware_repackage}"
		git_firmware_repackage_branch_or_commit = "${manifest.firmware_repackage.git_firmware_repackage_branch_or_commit}"
		
		//dlm module details
		dlm_template_name = "${manifest.dlm_module_details.template_name}"
		dlm_staging = "${manifest.dlm_module_details.dlm_staging}"
		dlm_module_name_all_manifests = "${manifest.dlm_module_details.all_manifests.module_name}"
		dlm_module_version_all_manifests = "${manifest.dlm_module_details.all_manifests.module_version}"
		dlm_module_artifactory_path = "${manifest.dlm_module_details.all_manifests.module_artifactory_path}"
		
		
		//External module details
		external_module_name = "${manifest.external_module_details.module_name}"
		external_module_owner_email = "${manifest.external_module_details.module_owner_email}"
        	external_module_version = "${manifest.external_module_details.module_version}"
        	external_module_artifactory_path = "${manifest.external_module_details.module_artifactory_path}"
		
		
		//inHouse module details
		module_name = "${manifest.inhouse_module_details.module_name}"
		module_owner = "${manifest.inhouse_module_details.module_owner}"
		module_owner_email = "${manifest.inhouse_module_details.module_owner_email}"
		module_product_owner = "${manifest.inhouse_module_details.module_product_owner}"
		module_product_owner_email = "${manifest.inhouse_module_details.module_product_owner_email}"
		module_recipe_build_create = "${manifest.inhouse_module_details.module_recipe_build_create}"
		module_config_build_create = "${manifest.inhouse_module_details.module_config_build_create}"	
		
		
		//source code
		source_repo_name = "${manifest.inhouse_module_details.source_code.repo_name}"
		source_git_branch = "${manifest.inhouse_module_details.source_code.git_branch}"
		source_git_commit = "${manifest.inhouse_module_details.source_code.git_commit}"
					
		
		//checkout script 
		checkout_additional_parameters = "${manifest.inhouse_module_details.checkout_script.additional_parameters}"
		checkout_git_repo_url = "${manifest.inhouse_module_details.checkout_script.git_repo_url}"
		checkout_git_branch = "${manifest.inhouse_module_details.checkout_script.git_branch}"
		checkout_path = "${manifest.inhouse_module_details.checkout_script.checkout_path}"
		checkout_output_manifest_path = "${manifest.inhouse_module_details.checkout_script.checkout_output_manifest_path}"
					
		
		//version script
		version_path = "${manifest.inhouse_module_details.version_script.version_path}"
		version_additional_parameters = "${manifest.inhouse_module_details.version_script.additional_parameters}"
		version_generator = "${manifest.inhouse_module_details.version_script.version_generator}"
		artifactory_cred_generator = "${manifest.inhouse_module_details.version_script.artifactory_cred_generator}"
		version_output_path = "${manifest.inhouse_module_details.version_script.version_output_path}"
		generated_fw_inserted_version = "${manifest.inhouse_module_details.version_script.generated_fw_inserted_version}"
		generated_dlm_version = "${manifest.inhouse_module_details.version_script.generated_dlm_version}"
		
		
		//build script
		build_additional_parameters = "${manifest.inhouse_module_details.build_script.additional_parameters}"
		build_path = "${manifest.inhouse_module_details.build_script.build_path}"
		fw_inserted_build_output_pattern = "${manifest.inhouse_module_details.build_script.fw_inserted_build_output_pattern}"
		fw_insterted_build_output_path = "${manifest.inhouse_module_details.build_script.fw_insterted_build_output_path}"
		dlm_build_output_pattern = "${manifest.inhouse_module_details.build_script.dlm_build_output_pattern}"
		dlm_build_output_path = "${manifest.inhouse_module_details.build_script.dlm_build_output_path}"
		git_repo_path_for_rearranging_module = "${manifest.inhouse_module_details.build_script.git_repo_path_for_rearranging_module}"
		git_rearranging_module_repack_script_name = "${manifest.inhouse_module_details.build_script.git_rearranging_module_repack_script_name}"
		output_name_of_the_rearranged_module = "${manifest.inhouse_module_details.build_script.output_name_of_the_rearranged_module}"
		
		
		//Artifactory repack firmware paths
		artifactory_repack_firmware_path = "vizio-dallas-recipe-config-module-releases/firmware-repack/"
		
		
	}
	stages {
		stage('Print Information') {
        		steps {
	    			script {
					println "upstream manifest_foldername: ${manifest_path}"
					
					//get if building recipe or config
					env.manifest_filename = sh (script: "echo ${manifest_path} |  rev | cut -d/ -f1 | rev", returnStdout: true).trim()
					env.getManifestType = sh (script: "echo ${manifest_filename} | cut -d- -f3", returnStdout: true).trim()
					env.getFur = sh (script: "echo ${manifest_filename} | cut -d- -f4 | rev | cut -d. -f2-  | rev", returnStdout: true).trim()
					
					println "manifest_filename: ${manifest_filename}"
					println "getManifestType (config/recipe): ${getManifestType}"
					println "getFur: ${getFur}"
				}
			}
		}				
		stage('clone firmware repackage') {
        		steps {
	    			script {
					//git_repo_firmware_repackage = splitString("${git_repo_firmware_repackage}")
					//git_firmware_repackage_branch_or_commit = splitString("${git_firmware_repackage_branch_or_commit}")
					
					env.git_repo_firmware_repackage_name = "firmware-repackage"
					println "manifest_foldername: ${manifest_foldername}"
					sh """
						cd ../${manifest_foldername}
						rm -rf firmware-repackage
						git clone ${git_repo_firmware_repackage}
						git checkout ${git_firmware_repackage_branch_or_commit}
					"""
				}
			}
		}
		stage('Read build JSON file') {
        		steps {
	    			script {
				
					//dependency1
					def dependency1_moduleid = []
					def dependency1_modulename = []
					def dependency1_source_param = []
					
					//dependency2
					def dependency2_moduleid = []
					def dependency2_modulename = []
					def dependency2_source_param = []
				
					def moduleid = []
					def modulename = []
					def source_param = []
					
					def sourceRepoName = []
					def sourceGitBranch = []
					def sourceGitCommit = []
					
					
					env.manifest_build_create = "${getManifestType}" == "recipe" ? "${module_recipe_build_create}" : "${module_config_build_create}"
					println "===manifest_build_create: ${manifest_build_create}======="
					
					manifest_build_create = splitString("${manifest_build_create}")
					module_name = splitString("${module_name}")
					
					
					for(int i=0; i<manifest_build_create.length; i++)
					{
						source_repo_name = splitString2("${source_repo_name}")
						source_git_branch = splitString2("${source_git_branch}")
						source_git_commit = splitString2("${source_git_commit}")
					        	
						sourceRepoName << "${source_repo_name[i]}"
						sourceGitBranch << "${source_git_branch[i]}"
						sourceGitCommit << "${source_git_commit[i]}"
					       
 						if ( manifest_build_create[i] == "TRUE" )
 						{
							if ( "${module_name[i]}" == "VIZIOS" )
							{
								sRepoName = splitString("${sourceRepoName[i]}")
								sGitBranch = splitString("${sourceGitBranch[i]}")
								sGitCommit = splitString("${sourceGitCommit[i]}")
								
								checkout_param = ""
								for(int j=0; j<sRepoName.length; j++){
									println "sRepoName of ${i} j: ${sRepoName[j]} "
									if ( "${getManifestType}" == "recipe")
									{
										checkout_param = "${checkout_param}" + "${sRepoName[j]}=${sGitBranch[j]}" + " " 
									}
									else{
										checkout_param = "${checkout_param}" + "${sRepoName[j]}=${sGitCommit[j]}" + " " 
									}
								
								}
								println "checkout param: ${checkout_param}"
								
								
								
								dependency1_moduleid << "${i}"
								dependency1_modulename << "${module_name[i]}"
								dependency1_source_param << "${checkout_param}"
								
								//println "==================getmodulename: ${getmodulename} getsource_param: ${getsource_param} ${getmoduleid}"
								
								
								
							}
							else if ( "${module_name[i]}" == "NDK" || "${module_name[i]}" == "BROWSERCTRLBUILD" )
							{
								sRepoName = splitString("${sourceRepoName[i]}")
								sGitBranch = splitString("${sourceGitBranch[i]}")
								sGitCommit = splitString("${sourceGitCommit[i]}")
								
								checkout_param = ""
								for(int j=0; j<sRepoName.length; j++){
									println "sRepoName of ${i} j: ${sRepoName[j]} "
									if ( "${getManifestType}" == "recipe")
									{
										checkout_param = "${checkout_param}" + "${sRepoName[j]}=${sGitBranch[j]}" + " " 
									}
									else{
										checkout_param = "${checkout_param}" + "${sRepoName[j]}=${sGitCommit[j]}" + " " 
									}
								
								}
								println "checkout param: ${checkout_param}"
								
								
								
								dependency2_moduleid << "${i}"
								dependency2_modulename << "${module_name[i]}"
								dependency2_source_param << "${checkout_param}"
								
								//println "==================getmodulename: ${getmodulename} getsource_param: ${getsource_param} ${getmoduleid}"
								
								
								
							}
							else {
						
								moduleid << "${i}"
								modulename << "${module_name[i]}"
							
								sRepoName = splitString("${sourceRepoName[i]}")
								sGitBranch = splitString("${sourceGitBranch[i]}")
								sGitCommit = splitString("${sourceGitCommit[i]}")
								//manifest_filename
		
								checkout_param = ""
								for(int j=0; j<sRepoName.length; j++){
									println "sRepoName of ${i} j: ${sRepoName[j]} "
									if ( "${getManifestType}" == "recipe")
									{
										checkout_param = "${checkout_param}" + "${sRepoName[j]}=${sGitBranch[j]}" + " " 
									}
									else{
										checkout_param = "${checkout_param}" + "${sRepoName[j]}=${sGitCommit[j]}" + " " 
									}
								
								}
								source_param << "${checkout_param}"
								println "source_param ${source_param} of ${i}: ${checkout_param}"
							}
						}
					}
					
					//dependency1
					def dependency1_pairs = (0..<Math.min(dependency1_modulename.size(), dependency1_moduleid.size())).collect { i -> [name: dependency1_modulename[i], id: dependency1_moduleid[i], source_repo_params: dependency1_source_param[i]]}
        				def dependency1_steps = dependency1_pairs.collectEntries { ["${it}" : generateStage(it.name, it.id, it.source_repo_params)] }
					parallel dependency1_steps
					
					//dependency2
					def dependency2_pairs = (0..<Math.min(dependency2_modulename.size(), dependency2_moduleid.size())).collect { i -> [name: dependency2_modulename[i], id: dependency2_moduleid[i], source_repo_params: dependency2_source_param[i]]}
        				def dependency2_steps = dependency2_pairs.collectEntries { ["${it}" : generateStage(it.name, it.id, it.source_repo_params)] }
					parallel dependency2_steps
					
					def pairs = (0..<Math.min(modulename.size(), moduleid.size())).collect { i -> [name: modulename[i], id: moduleid[i], source_repo_params: source_param[i]]}
        				def steps = pairs.collectEntries { ["${it}" : generateStage(it.name, it.id, it.source_repo_params)] }
					parallel steps
					
				}
			}
		}
		stage('Firmware Repack') {
        		steps {
	    			script {
					println "call firmware repack"
					
					config_manifest = readJSON file: "../${manifest_foldername}manifest-copy.json"
					config_model = "${config_manifest.chipset_details.model}"
					println "model: ${config_model}"
					
					
					config_module_name = "${config_manifest.external_module_details.module_name}"
					config_module_owner_email = "${config_manifest.external_module_details.module_owner_email}"
					config_module_version = "${config_manifest.external_module_details.module_version}"
					config_module_artifactory_path = "${config_manifest.external_module_details.module_artifactory_path}"
					
					
					config_fw_inserted_version = "${config_manifest.inhouse_module_details.artifactory_output_path.fw_inserted_version}"
					config_fw_inserted_artifactory_path = "${config_manifest.inhouse_module_details.artifactory_output_path.fw_inserted_artifactory_path}"
					config_release_version_fw_inserted_version = "${config_manifest.inhouse_module_details.artifactory_output_path.release_version_fw_inserted_version}"
					config_release_version_fw_inserted_artifactory_path = "${config_manifest.inhouse_module_details.artifactory_output_path.release_version_fw_inserted_artifactory_path}"
					
					
					config_module_name = splitString("${config_module_name}")
					config_module_owner_email = splitString("${config_module_owner_email}")
					config_module_version = splitString("${config_module_version}")
					config_module_artifactory_path = splitString("${config_module_artifactory_path}")
					
					
					config_fw_inserted_version = splitString("${config_fw_inserted_version}")
					config_fw_inserted_artifactory_path = splitString("${config_fw_inserted_artifactory_path}")
					config_release_version_fw_inserted_version = splitString("${config_release_version_fw_inserted_version}")
					config_release_version_fw_inserted_artifactory_path = splitString("${config_release_version_fw_inserted_artifactory_path}")
					
					git_repo_path_for_rearranging_module = splitString("${git_repo_path_for_rearranging_module}")
					
					sh """
						rm -rf firmware-package-modules
						mkdir firmware-package-modules
						mkdir firmware-package-modules/previous-repack-firmware
					"""
					
					module_name = splitString("${module_name}")
					
					def repack_inputs="";
					
					//loop through external modules
					for(int i=0; i<config_module_name.length; i++) 
					{
						if( config_module_name[i] == "BASEFIRMWARE" )
						{
							//cut last four characters .tgz/.zip
							
							// cut secure_rls.tgz or .tgz or .zip or -REL.zip
							
							env.BASE_FIRMWARE_VERSION = sh (script: "echo ${manifest_path} | rev | cut -c 4- | rev", returnStdout: true).trim()
							println "Base firmware version: ${BASE_FIRMWARE_VERSION}"
							
							//get last success repack firmware version from artifactory if any
							
							
							env.repack_type_first = sh (script: "echo ${repack_type} | cut -d_ -f1", returnStdout: true).trim()
							
							rtDownload (
								serverId: 'artifactory',
								spec:
								"""{
									"files": [
										{
									  	 "pattern": "${artifactory_repack_firmware_path}${chipset}/${repack_type}/${BASE_FIRMWARE_VERSION}-${repack_type_first}.txt",
									  	 "target": "firmware-package-modules/previous-repack-firmware/",
									  	 "flat": "true"
										}
								 	]
								 }"""
							)
							
							final String last_success_result = sh(script: "ls ./firmware-package-modules/previous-repack-firmware", returnStdout: true).trim()
						
							if (last_success_result != null && !last_success_result.isEmpty()) {
								def props = readProperties  file:"./firmware-package-modules/previous-repack-firmware/${BASE_FIRMWARE_VERSION}-${repack_type_first}.txt"
								env.PREVIOUS_FW_VERSION= props['FW_VERSION']
								echo "Last success PREVIOUS_FW_VERSION = ${PREVIOUS_FW_VERSION}"
								env.NEW_FIRMWARE_VERSION_NUMBER = "${PREVIOUS_FW_VERSION}".substring(0,"${PREVIOUS_FW_VERSION}".lastIndexOf("-")+1)
								println "NEW_FIRMWARE_VERSION_NUMBER: ${NEW_FIRMWARE_VERSION_NUMBER}"
							}
							else
							{
								if ( "${repack_type_first}" == "prod" )
								{
									env.NEW_FIRMWARE_VERSION_NUMBER = "${BASE_FIRMWARE_VERSION}"+"-1"
								}
								else
								{
									env.NEW_FIRMWARE_VERSION_NUMBER = "${BASE_FIRMWARE_VERSION}"+"${repack_type_first}"+"-1"
								}
							}
							
						}
							
						//download external modules from artifactory
						//call if any repack script should be called
						//add fields in external modules for additional repack script
						
						
					
					}
					
					//loop through inhouse modules
					for(int i=0; i<module_name.length; i++) 
					{
						if( config_fw_inserted_version[i] != "NA")
						{
							path = ""
							name = ""
							if(git_repo_path_for_rearranging_module[i] != "NA")
							{
								path = "${config_release_version_fw_inserted_artifactory_path[i]}${config_release_version_fw_inserted_version[i]}"
								name = "${config_release_version_fw_inserted_version[i]}"
							}
							else 
							{
								path = "${config_fw_inserted_artifactory_path[i]}${config_release_version_fw_inserted_version[i]}"
								name = "${config_release_version_fw_inserted_version[i]}"
							}
							
							//download modules from artifactory
							rtDownload (
								serverId: 'artifactory',
								spec:
								"""{
									"files": [
										{
									  	 "pattern": "${path}",
									  	 "target": "firmware-package-modules/${NEW_FIRMWARE_VERSION_NUMBER_WITH_MODEL}",
									  	 "flat": "true"
										}
								 	]
								 }"""
							)
							
							if( module_name[i] == "BROWSERCTRLSCRIPT")
							{	
                                				repack_inputs="${repack_inputs} firmware-package-modules/${NEW_FIRMWARE_VERSION_NUMBER_WITH_MODEL}"
							}
							else{
								repack_inputs="${repack_inputs} firmware-package-modules/${NEW_FIRMWARE_VERSION_NUMBER_WITH_MODEL}/${name}"
							}
						}
					}
					println "repack inputs: ${repack_inputs}"
					
					//repack script is called to repack all the selected modules
					sh """
						echo "========================Repackaging the firmware============================="
						#sudo ./repack-fw.sh ${base_firmware} ${NEW_FIRMWARE_VERSION_NUMBER_WITH_MODEL} ${repack_inputs}
						echo "========================End of Repackaging the firmware============================="
						
						echo "========================Validate the firmware============================="
						#sudo ./validate_fw.sh ${base_firmware} ${NEW_FIRMWARE_VERSION_NUMBER_WITH_MODEL}.*
						echo "========================End of Validation============================="
					"""
					
					//validate size
					
					//upload repack firmware to artifactory
					
					//upload status of the firmware to artifactory
					
				}
			}
		}
		stage('Sign the firmware') {
        		steps {
	    			script {
					println "Sign the firmware"
					
					
				}
			}
		}
		stage('Upload the config file') {
        		steps {
	    			script {
					println "Upload config file"
					
					
				}
			}
		}
		stage('Stage the firmware') {
        		steps {
	    			script {
					println "Stage the firmware"
					
					
				}
			}
		}
		stage('clear the directories') {
        		steps {
	    			script {
					println "clear the directories"
					
					
				}
			}
		}
	}
}
def splitString(def param) {
	param = param.substring(1, param.length()-1);
    	param = param.replaceAll("\\s","")
    	param = param.split(',') 
    	return param;
}

def splitString2(def param) {
	def splitparam=[];
	param = param.substring(1, param.length()-1);
    	param = sh (script: "(echo ${param} | grep -oP '\\[\\K[^\\]]+')", returnStdout: true).trim()
	sh "rm -rf split.txt"
	writeFile file: "split.txt", text: "${param}"
	def list = new File("${workspace}/split.txt").text.readLines()
	for (item in list) {
        	param = item.replaceAll("\\s","")
               	param = item.split(',')
                def newslackobj = "${param}"
		splitparam.add(newslackobj)
        }
	sh "rm -rf split.txt"
    	return splitparam;
}

def splitString3(def param) {
    param = param.substring(1, param.length()-1);
    param = param.replaceAll(",\\s",",");
    param = param.split(',')
    return param;
}
