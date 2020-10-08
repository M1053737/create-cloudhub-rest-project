@Library("mule-4-shared-library") _
import com.mulesoft.PipelinePlaceholders
import com.mulesoft.Secrets

node {
    def pipelinePlaceholders = PipelinePlaceholders.getInstance()
    secrets = Secrets.getInstance()
    properties([
        parameters([
            string(name: 'API_NAME', defaultValue: '', description: 'API Name (s, p or e)'),
            string(name: 'ORGANIZATION', defaultValue: 'Mulesoft', description: 'Organization'),
            string(name: 'ENV', defaultValue: 'WH-CRM-TEST', description: 'Environment to Create Build Job'),
            string(name: 'RestUrl', defaultValue: '', description: 'REST Template Reference URL')
        ])
    ])

    stage("Preparation") {
        secrets.setSecrets()
        pipelinePlaceholders.setApiAssetId(params.API_NAME.toLowerCase())
        pipelinePlaceholders.setOrganization(params.ORGANIZATION)
        pipelinePlaceholders.setEnvironment(params.ENV)
        pipelinePlaceholders.setRestTempUrl(params.RestUrl)
        pipelinePlaceholders.setDomainDependency("")
        pipelinePlaceholders.setDeploymentType("cloudhub")
        //pipelinePlaceholders.setVaultSecrets(getVaultSecrets())
        //pipelinePlaceholders.setApStructure(getApStructure())
    }

    stage("Create new Gitlab Repository") {
        createGitRepository()
    }

    stage("Get REST Template from Gitlab") {
        getTemplateFromGitlab("rest-template")
    }

    stage("Cleanup and Update REST Template") {
        updateRestTemplate2()
    }
 //   stage("Create API in Design Center") {
   //     createDesignCenterAPI()
   // }

    stage("Create Jenkins Build Job") {
        createJenkinsBuildJob()
    }

    //stage("Create Jenkins Deploy Job") {
      //  createJenkinsDeployJob()
    //}

    stage("Push Template to Repository") {
        pushToGitlabRepository()
    }

}
