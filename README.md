# Get Application Insights Key Action

GitHub Action to Azure Application Insights instrumentation key

```yaml
# File: .github/workflows/workflow.yml

on: push

env:
  RESOURCE_GROUP_NAME: sample-rg
  APPLICATION_INSIGHTS_NAME: sample-insights
jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: azure/login@v1 # Login to Azure
        with:
          creds: ${{ secrets.AZURE_CREDENTIALS }}
      - name: Get Application Insights Key
        # id: insights # step id for later reference
        uses: waeltken/get-application-insights-key-action@v1
        with:
          resource-group-name: ${{ env.RESOURCE_GROUP_NAME }}
          application-insights-name: ${{ env.APPLICATION_INSIGHTS_NAME }}
        # The Action exposes the Application Insights Instrumentation Key as
        # ${{ env.APPLICATION_INSIGHTS_KEY }}
        # You can also access the value from the steps output like this:
        # ${{ steps.insights.outputs.key }}
      - name: "run more steps"
        # APPLICATION_INSIGHTS_KEY environment variable visible here:
        run: echo $APPLICATION_INSIGHTS_KEY
```
