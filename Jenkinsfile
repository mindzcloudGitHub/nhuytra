#!groovy
import groovy.json.JsonSlurperClassic
node {
def BUILD_NUMBER=env.BUILD_NUMBER
    def RUN_ARTIFACT_DIR="tests/${BUILD_NUMBER}"
    def SFDC_USERNAME="vijay@mindzcloud.com"
     def INSTANCE_URL="https://login.salesforce.com"
    def HUB_ORG=env.HUB_ORG_DH
    def SFDC_HOST = env.SFDC_HOST_DH
   // def JWT_KEY_CRED_ID = env.JWT_CRED_ID_DH
	def JWT_KEY_CRED_ID="2e7e39c5-ddf9-4437-bc91-e503b901ad45"
   // def CONNECTED_APP_CONSUMER_KEY=env.CONNECTED_APP_CONSUMER_KEY_DH
     def CONNECTED_APP_CONSUMER_KEY="3MVG9n_HvETGhr3CIZVAQdHYTNhPvX.DFYU.AAauOXrt9ZuJBGAW4I0GZnh.OJNxeC.7Pr4sul6h6W0Gf9Aid"
    println 'KEY IS' 
    println JWT_KEY_CRED_ID
    println HUB_ORG
    println SFDC_HOST
    println CONNECTED_APP_CONSUMER_KEY
   def toolbelt =tool 'sfdx'
	
   println('Inside Deploy'+CONNECTED_APP_CONSUMER_KEY)
    stage('checkout source') {
        // when running in multi-branch job, one must issue this command
        checkout scm
    }

    
	withCredentials([file(credentialsId: JWT_KEY_CRED_ID, variable: 'jwt_key_file')]) {
	stage('Deploye Code') {
		println('Inside Deploy')
            
		 if (isUnix()) {
                rc = sh returnStatus: true, script: "${toolbelt} force:auth:jwt:grant --clientid ${CONNECTED_APP_CONSUMER_KEY} --username ${SFDC_USERNAME} --jwtkeyfile ${JWT_KEY_CRED_ID} --setdefaultdevhubusername --instanceurl ${INSTANCE_URL}"
            }else{
                 rc = bat returnStatus: true, script: "\"${toolbelt}\" force:auth:jwt:grant --clientid ${CONNECTED_APP_CONSUMER_KEY} --username ${SFDC_USERNAME} --jwtkeyfile ${JWT_KEY_CRED_ID} --setdefaultdevhubusername --instanceurl ${INSTANCE_URL}"
            }
            //if (rc != 0) { error 'hub org authorization failed' }
			println('Inside error')
			println rc
			
			// need to pull out assigned username
			if (isUnix()) {
				rmsg = sh returnStdout: true, script: "${toolbelt} force:mdapi:deploy -d manifest/. -u ${SFDC_USERNAME}"
			}else{
			   rmsg = bat returnStdout: true, script: "\"${toolbelt}\" force:mdapi:deploy -d manifest/. -u ${SFDC_USERNAME}"
			}	
		 printf rmsg
                 println('Hello from a Job DSL script!')
                 println(rmsg)  
            
        }
	}    
}
