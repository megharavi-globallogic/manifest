def targets = ["vssx7a", "vssx7b" ]

def parallelStagesMap = targets.collectEntries {
   ["${it}" : generateStage(it)]
}

def generateStage(target) {
   return {
      stage("stage: ${target}") {
         echo "This is ${target} target."
         env.target="${target}"
         sh '''
            rm -rf ${target}
            git clone git@github.com:BuddyTV/platform.git ${target}
            cd ${target}
            git checkout ${BRANCH_NAME}
            make clean NO_TTY=1
            make TARGET=${target} release VERBOSE=1 NO_TTY=1
         '''

         //get the name of the generated file version and change the build name
         env.versionFileName = sh(script: "cd ${target}/out && ls ViziOS-${target}*.tgz", returnStdout: true).trim()
         env.setBuildName = versionFileName.substring(15,versionFileName.length()-4)
         env.buildVersion = "${setBuildName}"

         //upload dlm and firmware modules to Artifactory
         rtUpload (
            serverId: 'artifactory',
            spec:
               """{
                  "files": [
                     {
                        "pattern": "${target}/out/ViziOS-${target}*.tgz",
                        "target": "${artifactory_module_upload_path_fw_inserted}${target}/"
                     },
                     {
                        "pattern": "${target}/out/vizios.*",
                        "target": "${artifactory_module_upload_path_dlm_inserted}${target}/"
                     },
                     {
                        "pattern": "${target}/out/ViziOS-${target}*.txt",
                        "target": "${artifactory_module_upload_path_fw_inserted}${target}/"
                     }
                  ]
               }"""
         )

         withCredentials([sshUserPrivateKey(credentialsId: 'git-vizio-dallas-module-manifests', keyFileVariable: '', passphraseVariable: '', usernameVariable: '')])
         {
            sh """
               set -x
               cd "${target}"
               git clone git@github.com:BuddyTV/vizio-dallas-module-manifests.git
            """
         }
         //updatemanifest("${target}", "${versionFileName}");
      }
   }
}

def updatemanifest(def mtarget, def vfile) {
   script{
      String[] branch = []
      if ("${mtarget}" == "mtk5581")  { branch= ["${manifest_branch}-15X"] }
      if ("${mtarget}" == "mtk5597")  { branch= ["${manifest_branch}-17X", "${manifest_branch}-18X"] }
      if ("${mtarget}" == "mtk5691")  { branch= ["${manifest_branch}-30X","release-FUR5.0-30X"] }
      if ("${mtarget}" == "mtk5695")  { branch= ["${manifest_branch}-31X","release-FUR5.0-31X"] }
      if ("${mtarget}" == "mtk5695l") { branch= ["${manifest_branch}-32X", "${manifest_branch}-tentative-32X"] }
      if ("${mtarget}" == "mtk5583")  { branch= ["${manifest_branch}-33X","release-FUR5.0-33X"] }
      if ("${mtarget}" == "mtk5695s") { branch= ["${manifest_branch}-34X"] }

      for(int i=0; i<branch.length; i++)
      {
         def branchName = "${branch[i]}"
         sh """
            cd "${mtarget}"/vizio-dallas-module-manifests
            git checkout "${branchName}"

            jq -e  '(.module_details[] | select(.module_name=="${manifest_module_name}") | .module_version_release) = "${vfile}"' manifest.json >manifest.json.tmp && mv manifest.json.tmp manifest.json

            jq -e  '(.module_details[] | select(.module_name=="${manifest_module_name}") | .module_version_release_artifactory_path) = "${artifactory_module_upload_path_fw_inserted}${mtarget}/"' manifest.json >manifest.json.tmp && mv manifest.json.tmp manifest.json

            git add manifest.json
            git diff-index --quiet HEAD || git commit -m "Update ViziOS to ${vfile}"
            git pull
            git push --force origin "${branchName}"
         """
      }
   }
}

def getBuildStatus()
{
   //get last successful build id
   def lastSuccessfulBuildID = 0
   env.status = "false"
   def build = currentBuild.previousBuild
   while (build != null) {
      if (build.result == "SUCCESS")
      {
         lastSuccessfulBuildID = build.id as Integer
         break
      }
      build = build.previousBuild
   }
   println "last success build id: ${lastSuccessfulBuildID}"

   env.platform_commits_count = sh(script: "echo `git rev-list --count HEAD` | bc", returnStdout: true).trim()
   println "current platform commits count: ${platform_commits_count}"

   env.platform_manifest_commits_count = sh(script: "cd platform-manifest && echo `git rev-list --count HEAD` | bc", returnStdout: true).trim()
   println "current platform manifest commits count: ${platform_manifest_commits_count}"

   writeFile file: "/var/lib/jenkins/jobs/${jenkins_job_name}/branches/${BRANCH_NAME}/builds/${BUILD_NUMBER}/build.properties", text: """platform=${platform_commits_count}
   platform-manifest=${platform_manifest_commits_count}"""

   if ( "${lastSuccessfulBuildID}" == "0" )
   {
      env.status = "true"
   }
   else
   {
      def props = readProperties  file:"/var/lib/jenkins/jobs/${jenkins_job_name}/branches/${BRANCH_NAME}/builds/${lastSuccessfulBuildID}/build.properties"

      def Var1= props['platform']
      def Var2= props['platform-manifest']
      println "last success platform and platform-manifest commits count platform=${Var1} platfrom-manifest=${Var2}"

      if ( "${Var1}" != "${platform_commits_count}"  ||  "${Var2}" != "${platform_manifest_commits_count}" )
      {
         env.status = "true"
      }
   }
   return status;
}

def lastSuccessfulBuild(passedBuilds, build) {
   if ((build != null) && (build.result != 'SUCCESS')) {
      passedBuilds.add(build)
      lastSuccessfulBuild(passedBuilds, build.getPreviousBuild())
   }
}

@NonCPS
def getChangeLog(passedBuilds) {
   def log = ""
   for (int x = 0; x < passedBuilds.size(); x++) {
      def currentBuild = passedBuilds[x];
      def changeLogSets = currentBuild.rawBuild.changeSets
      for (int i = 0; i < changeLogSets.size(); i++) {
         def entries = changeLogSets[i].items
         for (int j = 0; j < entries.length; j++) {
            def entry = entries[j]
             log += " Commit Message: ${entry.msg} \n Commit Id: ${entry.commitId}\n File Name: ${entry.affectedFiles.path}\n Changed By: ${entry.author} \n\n"
         }
      }
   }
   return log;
}

pipeline {
   agent any
   environment{
      module_name = "ViziOS"
      jenkins_job_name = "platform"

      manifest_branch = "master"
      manifest_module_name = "VIZIOS"

      artifactory_module_upload_path = "vizio-dallas-modules"
      artifactory_module_upload_path_fw_inserted = "${artifactory_module_upload_path}/${module_name}/${BRANCH_NAME}/fw-insterd-module/"
      artifactory_module_upload_path_dlm_inserted = "${artifactory_module_upload_path}/${module_name}/${BRANCH_NAME}/dlm-module/"
      artifactory_module_upload_path_release_notes = "${artifactory_module_upload_path}/${module_name}/${BRANCH_NAME}/release-notes/"

      slack_notification = "vizios-build"
   }
   stages {
      stage('Checkout platform-manifest from Github') {
         steps {
            script{
               dir('platform-manifest') {
                  git branch: "${BRANCH_NAME}", url: 'git@github.com:BuddyTV/platform-manifest.git'
               }
            }
         }
      }
      stage("Build-trigger status") {
         steps {
            script {
               env.buildStatus = getBuildStatus()
               if ( "${buildStatus}" == "false" )
               {
                  currentBuild.result = 'ABORTED'
                  error('No changes detected in platform and platform-manifest')
               }
            }
         }
      }
      stage('parallel stage') {
         steps {
            script {
               parallel parallelStagesMap
            }
         }
      }
      stage('set version') {
         steps {
            script {
               currentBuild.displayName = "${buildVersion}"
            }
         }
      }
      stage('release-notes') {
         steps {
            script {
               //get all files that got changed from last successful build and upload it to artifactory
               passedBuilds = []
               lastSuccessfulBuild(passedBuilds, currentBuild);
               env.changeLog = getChangeLog(passedBuilds)
               echo "changeLog ${changeLog}"

               sh 'rm -rf release-notes*'
               writeFile file: "release-notes-${buildVersion}.txt", text: "${changeLog}"

               rtUpload (
                  serverId: 'artifactory',
                  spec:
                     """{
                        "files": [
                           {
                             "pattern": "release-notes*",
                             "target": "${artifactory_module_upload_path_release_notes}"
                           }
                        ]
                     }"""
               )
               sh 'rm -rf release-notes*'
            }
         }
      }
   }

   post {
      success {
         script {
            def success_attachment = [
               [
                  text: "*Build Success for Vizios ${targets}*\n *Version: ${env.setBuildName}*\n *Notes:* ${env.changeLog}\n Build URL:${env.BUILD_URL}",
                  color: "#00ff00"
               ]
            ]
            slackSend (attachments: success_attachment, channel: "${slack_notification}")
         }
      }

      failure {
         script {
            sh label: '', script: 'sudo rm -rf mtk*'
            def attachments_failed = [
               [
                  text: "<!here>Build FAILED for Vizios ${targets}\n Build URL:${env.BUILD_URL}",
                  color: "#ff0000"
               ]
            ]
            slackSend (attachments: attachments_failed, channel: "${slack_notification}")
         }
      }
   }
}
