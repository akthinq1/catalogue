// I am calling jenkins shared library hear 
@Library("jenkins-shared-library") _

// def configMap = [
//     greeting : "Hello jenkins-shared-library"
// ]

// samplePipeline(configMap)


def configMap = [
    project: "roboshop",
    component: "catalogue"
]

// write a condition if branch is equlas to main only
if(!env.BRANCH_NAME.equlasIgnoreCase('main')){
     nodejsEKSPipeline.call(configMap)
}
else{
    echo "Please proceed with PROD process"
}