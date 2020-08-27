#!groovy

def productId = 1
def region = "us-east-2"
def envName = "dev"

def context = [
    now                 : new Date(),
    branchName          : env.BRANCH_NAME,
    version             : env.BUILD_NUMBER,
    application         : "coe_ssa_saas-infra",
    applicationUriRoot  : "coe_ssa_saas-infra",
    applicationVersion  : "0.1.0",     
]

println "Pipeline Version='${context.version}'"
println "Branch name='${env.BRANCH_NAME}'"
println "Job name='${env.JOB_NAME}'"
println "Build number='${env.BUILD_NUMBER}'"

try {

    lock("${context.application}-${context.branchName}-build") {

        node("master"){

            try {

                stage("Checkout") {
                    
                    cleanWs()
                    checkout scm

                }

                stage("Init - ${productId}") {

                    echo("WithCredentials")                        
                    echo("terragrunt init")                        

                }

                stage("Plan - ${productId}") {

                    echo("WithCredentials")
                    echo("cd /main/${region}/${productId}/${envName}")
                    echo("terragrunt plan-all -out=plan.tfplan")
                    echo("terragrunt show -no-color plan.tfplan >> plan.txt")                     

                }

                stage("Apply - ${prodId}") {
                    
                    echo("WithCredentials")
                    echo("terragrunt apply plan.tfplan")                         

                }

            }

            finally {

                cleanWs notFailBuild: true

            }
                
        }


    }

}

catch (Exception e){

    println "ERROR: '${context.application}-api' branch '${env.BRANCH_NAME}' build #${env.BUILD_NUMBER} failed with error: ${e}. (<${env.BUILD_URL}|Open>)" 
    throw e

}

def echo(String message) {
    bat "@echo ${message}"
}