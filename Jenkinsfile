#!groovy
//Load environment variables
node {
    nodeLabel         = env.node_label
    globalRepo        = env.globalrepo
    globalRepoBranch  = 'develop' //select the branch in the global workflow libs
    globalRepoCredsId = env.jenkins_ssh_credentials
}

def repoParams = [
    'etherRepoBranch'  : 'master',   //select the branch in the ether workflow libs
    'component'   : 'library',
    'slave_label' : 'paas',
    'service'   : 'cassandra-openshift',
    'type' : 'module',
    'slackUrl' : 'https://hooks.slack.com/services/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',
    'slackChannel' : '#alerts-jenkins'
]

stage("Checkout Global Library") {
    // Load global workflow libs
    fileLoader.withGit(globalRepo, globalRepoBranch, globalRepoCredsId, nodeLabel) {
        etherCi = fileLoader.load("src/ether/ci/ci");
    }
}

etherCi.etherCiFlow(repoParams)
