node('master') {
  try {

    stage('Checkout SCM') {
      checkout([
        $class: "GitSCM", 
        branches: [[name: "refs/heads/main"]], 
        extensions: [[$class: 'CloneOption', timeout: 30]], 
        userRemoteConfigs: [[url: "https://github.com/falyan/DSL.git"]]
      ])
    }

    stage('Create a Project') {
      jobDsl targets: [  
        'src/jobdsl/squad/squad-1/*.groovy',
      ].join('\n'),
      removedJobAction: 'DELETE',
      removedViewAction: 'DELETE',
      lookupStrategy: 'SEED_JOB'
      failFast: true|false
    }
  } catch (e) {
    echo " Should be integrate to teams if failed"
    throw e
    updateGitlabCommitStatus name: 'build', state: 'failed'
  } finally {
    echo " Should be integrate to teams if success"
    updateGitlabCommitStatus name: 'build', state: 'success'
  }
}
