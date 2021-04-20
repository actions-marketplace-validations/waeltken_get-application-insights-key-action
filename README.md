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
      # checkout the repo
      - name: "Checkout Github Action"
        uses: actions/checkout@master
      - uses: azure/login@v1
        with:
          creds: ${{ secrets.AZURE_CREDENTIALS }}
      - name: Get Application Insights Key
        id: insights # step id for later reference
        uses: waeltken/get-application-insights-key-action@v1
        with:
          resource-group-name: ${{ env.RESOURCE_GROUP_NAME }}
          application-insights-name: ${{ env.APPLICATION_INSIGHTS_NAME }}

      - name: Setup Node 10.x
        uses: actions/setup-node@v1
        with:
          node-version: "10.x"
      - name: "npm install, build, and test"
        env:
          # reference the application insights key:
          APPLICATION_INSIGHTS_KEY: ${{ steps.insights.outputs.key }}
        run: |
          npm install
          npm run build --if-present
          npm run test --if-present
```
