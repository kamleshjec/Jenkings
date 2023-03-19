#Validate the Pull Request yaml file in repo

pipeline {
  agent any
  stages {
    stage("Validate YAML files") {
      steps {
        script {
          yamlFiles=sh(returnStdout: true, script: 'find . -type f -name "*.yaml" -not -path "./*/template*/*"').trim() #exclude the folder to validate i.e template
          def validationErrors=false
          for (yamlFile in yamlFiles.split('\n')) {
            if (!yamlFile?.trim()) {
              continue
            }
            try {
              def yaml = readYaml file: yamlFile
            } catch (Exception err) {
              validationErrors=true
              echo "Syntax errors are present in " + yamlFile + ":" + err.getMessage()
            }
          }
          if (validationErrors) {
            currentBuild.result = 'ABORTED DUE TO WRONG YAML FORMAT'
            error("Malformed YAML detected")
          } else {
            echo "YAML all OK"
          }
        }
      }
    }
  }
}

