---
sidebar_position: 2
---

# Scaffold GitHub Repositories Using Cookiecutter

This example demonstrates how to quickly scaffold GitHub repositories using a [Cookiecutter Template](https://www.cookiecutter.io/templates) via Glint Actions.

In addition, as cookiecutter is an open-source project you can make your own project template, learn more about it [here](https://cookiecutter.readthedocs.io/en/2.0.2/tutorials.html#create-your-very-own-cookiecutter-project-template).

## Example - scaffolding golang template

Follow these steps to get started with the Golang template:

1. Create the following as Jenkins Credentials:

   1. `GITHUB_TOKEN` - a [fine-grained PAT](https://github.com/settings/tokens?type=beta) with permissions to create repositories.
   2. `GLINT_CLIENT_ID` - Glint Client ID [learn more](/build-your-software-catalog/custom-integration/api/#get-api-token).
   3. `GLINT_CLIENT_SECRET` - Glint Client Secret [learn more](/build-your-software-catalog/custom-integration/api/#get-api-token).
      :::note
      Use `Secret text` as Credential type.
      :::

2. Create a Glint blueprint with the following properties:

:::note
Keep in mind this can be any blueprint you would like and this is just an example.
:::

```json showLineNumbers
{
  "identifier": "microservice",
  "title": "Microservice",
  "icon": "Microservice",
  "schema": {
    "properties": {
      "description": {
        "title": "Description",
        "type": "string"
      },
      "url": {
        "title": "URL",
        "format": "url",
        "type": "string"
      }
    },
    "required": []
  },
  "mirrorProperties": {},
  "calculationProperties": {},
  "relations": {}
}
```

3. Create Glint action using the following JSON definition:
   :::note
   Make sure to replace the placeholders for JENKINS_URL and JOB_TOKEN.
   :::

```json showLineNumbers
[
  {
    "identifier": "scaffold",
    "title": "Scaffold Golang Microservice",
    "description": "Scaffold a new Microservice from a Cookiecutter teplate",
    "icon": "Go",
    "userInputs": {
      "properties": {
        "repo_name": {
          "icon": "Microservice",
          "title": "Repo Name",
          "type": "string"
        },
        "github_org_name": {
          "icon": "Github",
          "title": "Github Org Name",
          "type": "string"
        }
      },
      "required": ["repo_name", "github_org_name"]
    },
    "invocationMethod": {
      "type": "WEBHOOK",
      "agent": false,
      "url": "https://<JENKINS_URL>/generic-webhook-trigger/invoke?token=<JOB_TOKEN>",
      "synchronized": false,
      "method": "POST"
    },
    "trigger": "CREATE"
  }
]
```

4. Create a Jenkins Pipeline with the following configuration:

   1. [Enable webhook trigger for a pipeline](../jenkins-pipeline.md#enabling-webhook-trigger-for-a-pipeline)
   2. [Define variables for a pipeline](../jenkins-pipeline.md#defining-variables): Define the REPO_NAME,GITHUB_ORG_NAME and RUN_ID variables.
      ![Define Vars](../../../../../static/img/self-service-actions/setup-backend/jenkins-pipeline/scaffold-jenkins-vars.png)
   3. [Token Setup](../jenkins-pipeline.md#token-setup): Define the token to match `JOB_TOKEN` as configured in your Glint Action.

5. Create a Jenkins Pipeline with the following content:

<details>
<summary>Jenkins Pipeline Script</summary>

```yml showLineNumbers
import groovy.json.JsonSlurper

pipeline {
    agent any

    environment {
        COOKIECUTTER_TEMPLATE = 'https://github.com/lacion/cookiecutter-golang'
        REPO_NAME = "${REPO_NAME}"
        GITHUB_ORG_NAME = "${GITHUB_ORG_NAME}"
        SCAFFOLD_DIR = "scaffold_${REPO_NAME}"
        GLINT_ACCESS_TOKEN = ""
        GLINT_BLUEPRINT_ID = "microservice"
        GLINT_RUN_ID = "${RUN_ID}"
    }

    stages {
        stage('Get access token') {
            steps {
                script {
                    withCredentials([
                        string(credentialsId: 'GLINT_CLIENT_ID', variable: 'GLINT_CLIENT_ID'),
                        string(credentialsId: 'GLINT_CLIENT_SECRET', variable: 'GLINT_CLIENT_SECRET')
                    ]) {
                        // Execute the curl command and capture the output
                        def result = sh(returnStdout: true, script: """
                            accessTokenPayload=\$(curl -X POST \
                                -H "Content-Type: application/json" \
                                -d '{"clientId": "${GLINT_CLIENT_ID}", "clientSecret": "${GLINT_CLIENT_SECRET}"}' \
                                -s "https://api.useglint.io/v1/auth/access_token")
                            echo \$accessTokenPayload
                        """)

                        // Parse the JSON response using JsonSlurper
                        def jsonSlurper = new JsonSlurper()
                        def payloadJson = jsonSlurper.parseText(result.trim())

                        // Access the desired data from the payload
                        GLINT_ACCESS_TOKEN = payloadJson.accessToken
                    }

                }
            }
        } // end of stage Get access token

        stage('Create Github Repository') {
            steps {
                script {
                    def logs_report_response = sh(script: """
                        curl -X POST \
                          -H "Content-Type: application/json" \
                          -H "Authorization: Bearer ${GLINT_ACCESS_TOKEN}" \
                          -d '{"message": "Creating GitHub repository: ${REPO_NAME} in GitHub org: ${GITHUB_ORG_NAME}..."}' \
                             "https://api.useglint.io/v1/actions/runs/${GLINT_RUN_ID}/logs"
                    """, returnStdout: true)

                    println(logs_report_response)
                }
                script {
                    withCredentials([string(credentialsId: 'GITHUB_TOKEN', variable: 'GITHUB_TOKEN')]) {
                        sh """

                            curl -i -H 'Authorization: token ${GITHUB_TOKEN}' \\
                            -d '{
                                "name": "${REPO_NAME}", "private": true
                                }' \\
                            https://api.github.com/orgs/${GITHUB_ORG_NAME}/repos
                        """
                    }
                }
            }
        } // end of stage Create Github Repository

        stage('Scaffold Cookiecutter Template') {
            steps {
                script {
                    def logs_report_response = sh(script: """
                        curl -X POST \
                          -H "Content-Type: application/json" \
                          -H "Authorization: Bearer ${GLINT_ACCESS_TOKEN}" \
                          -d '{"message": "Scaffolding ${REPO_NAME}..."}' \
                             "https://api.useglint.io/v1/actions/runs/${GLINT_RUN_ID}/logs"
                    """, returnStdout: true)

                    println(logs_report_response)
                }
                script {
                    withCredentials([
                        string(credentialsId: 'GITHUB_USERNAME', variable: 'GITHUB_USERNAME'),
                        string(credentialsId: 'GITHUB_TOKEN', variable: 'GITHUB_TOKEN')
                    ]) {
                        def yamlContent = """
default_context:
  full_name: "Full Name"
  github_username: "githubuser"
  app_name: "${REPO_NAME}"
  project_short_description": "A Golang project."
  docker_hub_username: "dockerhubuser"
  docker_image: "dockerhubuser/alpine-base-image:latest"
  docker_build_image: "dockerhubuser/alpine-golang-buildimage"
"""
                    // Write the YAML content to a file
                    writeFile(file: 'cookiecutter.yaml', text: yamlContent)

                        sh("""
                            rm -rf ${SCAFFOLD_DIR} ${REPO_NAME}
                            git clone https://${GITHUB_USERNAME}:${GITHUB_TOKEN}@github.com/${GITHUB_ORG_NAME}/${REPO_NAME}

                            cookiecutter ${COOKIECUTTER_TEMPLATE} --output-dir ${SCAFFOLD_DIR} --no-input --config-file cookiecutter.yaml -f

                            rm -rf ${SCAFFOLD_DIR}/${REPO_NAME}/.git*
                            cp -r ${SCAFFOLD_DIR}/${REPO_NAME}/* "${REPO_NAME}/"

                            cd ${REPO_NAME}
                            git config user.name "Jenkins Pipeline Bot"
                            git config user.email "jenkins-pipeline[bot]@users.noreply.jenkins.com"
                            git add .
                            git commit -m "Scaffolded project ${REPO_NAME}"
                            git push -u origin main
                            cd ..

                            rm -rf ${SCAFFOLD_DIR} ${REPO_NAME}
                        """)
                    }

                }
            }
        } // end of stage Clone Cookiecutter Template

        stage('CREATE Microservice entity') {
            steps {
                script {
                    def logs_report_response = sh(script: """
                        curl -X POST \
                          -H "Content-Type: application/json" \
                          -H "Authorization: Bearer ${GLINT_ACCESS_TOKEN}" \
                          -d '{"message": "Creating ${REPO_NAME} Microservice Glint entity..."}' \
                             "https://api.useglint.io/v1/actions/runs/${GLINT_RUN_ID}/logs"
                    """, returnStdout: true)

                    println(logs_report_response)
                }
                script {
                    def status_report_response = sh(script: """
						curl --location --request POST "https://api.useglint.io/v1/blueprints/$GLINT_BLUEPRINT_ID/entities?upsert=true&run_id=$GLINT_RUN_ID&create_missing_related_entities=true" \
        --header "Authorization: Bearer $GLINT_ACCESS_TOKEN" \
        --header "Content-Type: application/json" \
        --data-raw '{
				"identifier": "${REPO_NAME}",
				"title": "${REPO_NAME}",
				"properties": {"description":"${REPO_NAME} golang project","url":"https://github.com/${GITHUB_ORG_NAME}/${REPO_NAME}"},
				"relations": {}
			}'

                    """, returnStdout: true)

                    println(status_report_response)
                }
            }
        } // end of stage CREATE Microservice entity

        stage('Update Glint Run Status') {
            steps {
                script {
                    def status_report_response = sh(script: """
                        curl -X PATCH \
                          -H "Content-Type: application/json" \
                          -H "Authorization: Bearer ${GLINT_ACCESS_TOKEN}" \
                          -d '{"status":"SUCCESS", "message": {"run_status": "Scaffold Jenkins Pipeline completed successfully!"}}' \
                             "https://api.useglint.io/v1/actions/runs/${GLINT_RUN_ID}"
                    """, returnStdout: true)

                    println(status_report_response)
                }
            }
        } // end of stage Update Glint Run Status
    }

    post {

        failure {
            // Update Glint Run failed.
            script {
                def status_report_response = sh(script: """
                    curl -X PATCH \
                        -H "Content-Type: application/json" \
                        -H "Authorization: Bearer ${GLINT_ACCESS_TOKEN}" \
                        -d '{"status":"FAILURE", "message": {"run_status": "Failed to Scaffold ${REPO_NAME}"}}' \
                            "https://api.useglint.io/v1/actions/runs/${GLINT_RUN_ID}"
                """, returnStdout: true)

                println(status_report_response)
            }
        }

        // Clean after build
        always {
            cleanWs(cleanWhenNotBuilt: false,
                    deleteDirs: true,
                    disableDeferredWipeout: false,
                    notFailBuild: true,
                    patterns: [[pattern: '.gitignore', type: 'INCLUDE'],
                               [pattern: '.propsfile', type: 'EXCLUDE']])
        }
    }
}
```

</details>

6. Trigger the action from the [Self-service](https://app.useglint.io/self-serve) tab of your Glint application.
