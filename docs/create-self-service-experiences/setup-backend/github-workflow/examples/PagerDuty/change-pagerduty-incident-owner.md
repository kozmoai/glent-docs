# Change Incident Owner In Pagerduty

This GitHub action allows you to quickly change incident owner in PagerDuty via Glint Actions with ease.

## Inputs
| Name                 | Description                                                                                          | Required | Default            |
|----------------------|------------------------------------------------------------------------------------------------------|----------|--------------------|
| incident_id         | ID of the PagerDuty Incident     | true    | -                  |
| new_owner_user_id              | PagerDuty User ID of the new owner                                | true     | -                  |
| from              | The email address of a valid user associated with the account making the request.                                                              | true    | -               |

## Steps

1. Create the following GitHub action secrets:
* `PAGERDUTY_API_KEY` - PagerDuty API Key [learn more](https://support.pagerduty.com/docs/).
* `GLINT_CLIENT_ID` - Glint Client ID [learn more](https://docs.useglint.io/build-your-software-catalog/sync-data-to-catalog/api/#get-api-token).
* `GLINT_CLIENT_SECRET` - Glint Client Secret [learn more](https://docs.useglint.io/build-your-software-catalog/sync-data-to-catalog/api/#get-api-token).

2. Install the Glints GitHub app from [here](https://github.com/apps/useglint-io/installations/new).

3. Install Glint's pager duty integration [learn more](https://github.com/kozmoai/Glint-Ocean/tree/main/integrations/pagerduty).
:::note Blueprint

This step is not required for this example, but it will create all the blueprint boilerplate for you, and also update the catalog in real time with the new incident created.

:::

4. After you installed the integration, the blueprints `pagerdutyService` and `pagerdutyIncident` will appear, create the following action with the following JSON file on the `pagerdutyIncident` blueprint:

<details>
<summary><b>Change Incident Owner Blueprint (Click to expand)</b></summary>

```json

{
  "identifier": "change_incident_owner",
  "title": "Change Incident Owner",
  "icon": "pagerduty",
  "userInputs": {
    "properties": {
      "incident_id": {
        "icon": "pagerduty",
        "title": "Incident Id",
        "description": "ID of the PagerDuty Incident",
        "type": "string",
        "blueprint": "pagerdutyIncident",
        "format": "entity"
      },
      "new_owner_user_id": {
        "title": "New Owner User ID",
        "description": "PagerDuty User ID of the new owner",
        "icon": "pagerduty",
        "type": "string"
      },
      "from": {
        "icon": "pagerduty",
        "title": "From",
        "description": "The email address of a valid user associated with the account making the request.",
        "type": "string"
      }
    },
    "required": [
      "new_owner_user_id",
      "incident_id",
      "from"
    ],
    "order": [
      "incident_id",
      "new_owner_user_id",
      "from"
    ]
  },
  "invocationMethod": {
    "type": "GITHUB",
    "org": "your-github-organization",
    "repo": "your-github-repo",
    "workflow": "change-incident-owner.yaml",
    "omitUserInputs": false,
    "omitPayload": false,
    "reportWorkflowStatus": true
  },
  "trigger": "DAY-2",
  "description": "Change Incident Owner in pagerduty",
  "requiredApproval": false
}
```

</details>

:::note Customisation

Replace the invocation method with your own repository details.

:::

5. Create a workflow file under `.github/workflows/change-incident-owner.yaml` with the following content:

<details>
<summary><b>Change Incident Owner Workflow (Click to expand)</b></summary>

```yaml
name: Change PagerDuty Incident Owner

on:
  workflow_dispatch:
    inputs:
      incident_id:
        description: ID of the PagerDuty Incident
        required: true
        type: string
      new_owner_user_id:
        description: PagerDuty User ID of the new owner
        required: true
        type: string
      from:
        description: The email address of a valid user associated with the account making the request.
        required: true
        type: string
      port_payload:
        required: true
        description: >-
          Glint's payload, including details for who triggered the action and
          general context (blueprint, run id, etc...)

jobs:
  change-incident-owner:
    runs-on: ubuntu-latest
    steps:
      
      - name: Inform execution of request to change incident owner
        uses: kozmoai/glint-github-action@v1
        with:
          clientId: ${{ secrets.GLINT_CLIENT_ID }}
          clientSecret: ${{ secrets.GLINT_CLIENT_SECRET }}
          baseUrl: https://api.useglint.io
          operation: PATCH_RUN
          runId: ${{fromJson(github.event.inputs.port_payload).context.runId}}
          logMessage: "About to make a request to pagerduty..."

      - name: Change Incident Owner in PagerDuty
        id: change_owner
        uses: fjogeleit/http-request-action@v1
        with:
          url: 'https://api.pagerduty.com/incidents/${{ github.event.inputs.incident_id }}'
          method: 'PUT'
          customHeaders: '{"Content-Type": "application/json", "Accept": "application/vnd.pagerduty+json;version=2", "Authorization": "Token token=${{ secrets.PAGERDUTY_API_KEY }}", "From": "${{ github.event.inputs.from }}"}'
          data: >-
            {
              "incident": {
                "type": "incident_reference",
                "assignments": [
                  {
                    "assignee": {
                      "id": "${{ github.event.inputs.new_owner_user_id }}",
                      "type": "user_reference"
                    }
                  }
                ]
              }
            }

      - name: Inform ingestion of pagerduty feature flag to Glint
        uses: kozmoai/glint-github-action@v1
        with:
          clientId: ${{ secrets.GLINT_CLIENT_ID }}
          clientSecret: ${{ secrets.GLINT_CLIENT_SECRET }}
          baseUrl: https://api.useglint.io
          operation: PATCH_RUN
          runId: ${{fromJson(github.event.inputs.port_payload).context.runId}}
          logMessage: "Reporting the updated incident back to glint ..."

      - name: Upsert pagerduty entity to Glint 
        uses: kozmoai/glint-github-action@v1
        with:
          identifier: "${{ fromJson(steps.change_owner.outputs.response).incident.id }}"
          title: "${{ fromJson(steps.change_owner.outputs.response).incident.title }}"
          blueprint: "pagerdutyIncident"
          properties: |-
            {
              "status": "${{ fromJson(steps.change_owner.outputs.response).incident.status }}",
              "url": "${{ fromJson(steps.change_owner.outputs.response).incident.self }}",
              "urgency": "${{ fromJson(steps.change_owner.outputs.response).incident.urgency }}",
              "responder": "${{ fromJson(steps.change_owner.outputs.response).incident.assignments[0].assignee.summary}}",
              "escalation_policy": "${{ fromJson(steps.change_owner.outputs.response).incident.escalation_policy.summary }}",
              "created_at": "${{ fromJson(steps.change_owner.outputs.response).incident.created_at }}",
              "updated_at": "${{ fromJson(steps.change_owner.outputs.response).incident.updated_at }}"
            }
          clientId: ${{ secrets.GLINT_CLIENT_ID }}
          clientSecret: ${{ secrets.GLINT_CLIENT_SECRET }}
          baseUrl: https://api.useglint.io
          operation: UPSERT
          runId: ${{ fromJson(inputs.port_payload).context.runId }}

      - name: Inform completion of pagerduty feature flag ingestion into Glint
        uses: kozmoai/glint-github-action@v1
        with:
          clientId: ${{ secrets.GLINT_CLIENT_ID }}
          clientSecret: ${{ secrets.GLINT_CLIENT_SECRET }}
          baseUrl: https://api.useglint.io
          operation: PATCH_RUN
          runId: ${{fromJson(github.event.inputs.port_payload).context.runId}}
          logMessage: "Entity upserting was successful ✅"
```

</details>

6. Trigger the action from Glint's [Self Serve](https://app.useglint.io/self-serve)

7. Done! wait for the incident owner to be changed on PagerDuty

Congrats 🎉 You've successfully changed an incident owner in PagerDuty from Glint!