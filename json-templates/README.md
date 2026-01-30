# JSON Templates for Job Card

This folder contains demo job card templates that can be imported into the application.

## Available Templates

- **demo-cooling-systems.json** - Engine Cooling Systems Assessment (7290-702) - Example completed job card
- **demo-cooling-systems-assessment.json** - Engine Cooling Systems Assessment Form (7290-702) - Assessor observation form with learner skeleton tasks
- **demo-brake-systems.json** - Brake Systems Service (Coming)
- **demo-engine-systems.json** - Engine Systems Diagnosis (Coming)

## How to Use

1. Open the Motor Vehicle Job Card application
2. In the "Load Template or Import Data" section, click "Quick Load Demo"
3. Select a demo template
4. The form will populate with the complete job card data
5. Modify as needed for your assessment

## Creating Your Own Templates

Templates are standard JSON files with the following structure:
```json
{
  "studentName": "string",
  "jobDate": "YYYY-MM-DD",
  "customerRequests": [
    { "id": number, "text": "string", "completed": boolean }
  ],
  "tools": [
    { "id": number, "name": "string" }
  ],
  "jobs": [
    {
      "id": number,
      "requestId": number,
      "requestText": "string",
      "completed": boolean,
      "tasks": [...]
    }
  ]
}
```

See the demo files for complete examples.
