pipeline {
  agent {
    node {
      label 'master'
    }

  }
  stages {
    stage('Compile') {
      steps {
        echo 'Compiling...'
        sh "${tool name: 'sbt', type:'org.jvnet.hudson.plugins.SbtPluginBuilder$SbtInstallation'}/bin/sbt compile"
        sh 'echo "${COMPILER}"'
      }
    }

    stage('Packaging') {
      steps {
        echo 'Packaging...'
        sh "${tool name: 'sbt', type:'org.jvnet.hudson.plugins.SbtPluginBuilder$SbtInstallation'}/bin/sbt clean stage"
      }
    }

    stage('error') {
      parallel {
        stage('error') {
          steps {
            archiveArtifacts(artifacts: 'target/**/*.jar', fingerprint: true)
          }
        }

        stage('stash') {
          steps {
            stash(name: 'packaged', includes: 'target/**/*.jar')
            unstash 'packaged'
          }
        }

      }
    }

    stage('Deploy') {
      steps {
        echo 'Confirm for deploy'
        input(message: 'Do you wish to proceed ?', ok: 'Do It !', submitter: 'Partha')
      }
    }

  }
  environment {
    COMPILER = 'sbt'
  }
}