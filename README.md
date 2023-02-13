# Oxygen Validation Action

This action triggers <i>Oxygen Scripting</i> to perform a validation on your repository files. All you have to do is to include this action in your workflow and choose the directory to be validated. Find more info about workflows on https://docs.github.com/en/actions/using-workflows.

👀 See [Oxygen Validate Script](https://www.oxygenxml.com/doc/versions/25.0/ug-editor/topics/scripting_oxygen_validate.html) for more details about this script.

# Requirements

In order to use this action, you need to obtain an <i>Oxygen Scripting</i> license key from https://www.oxygenxml.com/xml_scripting/pricing.html (you can also request a [trial](https://www.oxygenxml.com/xml_scripting/register.html)). Add it as a secret to your repository (Settings &rarr; Secrets &rarr; Actions &rarr; New repository secret), and name it "SCRIPTING_LICENSE_KEY".
Then use it as an environment variable:
```yaml
env:
  SCRIPTING_LICENSE_KEY: ${{secrets.SCRIPTING_LICENSE_KEY}}
```

# Sample usage

If you don't already have a workflow defined in your repository, you can use one of the samples below.

💡 This workflow requires manual trigger from the 'Actions' tab:
```yaml
name: Run Validation (manually)
on:
  workflow_dispatch:
    inputs:
      validationDir:
        description: 'The directory to be validated.'
        required: true
jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - name: Oxygen Validation Script
        uses: oxygenxml/oxygen-scripting-validation-action@v1.0.0
        env:
          SCRIPTING_LICENSE_KEY: ${{secrets.SCRIPTING_LICENSE_KEY}}
        with:
          validationDir: ${{ github.event.inputs.validationDir }}
```
💡 This workflow automatically starts when a commit is pushed to the <i>main</i> branch, but can also be triggered manually:
```yaml
name: Run Validation (automatically)
# Controls when the workflow will run
on:
  # Triggers the workflow on push or pull request events but only for the "main" branch
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]
  workflow_dispatch: # Allows you to run this workflow manually from the Actions tab

jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - name: Oxygen Validation Script
        uses: oxygenxml/oxygen-scripting-validation-action@v1.0.0
        env:
          SCRIPTING_LICENSE_KEY: ${{secrets.SCRIPTING_LICENSE_KEY}}
        with:
          validationDir: 'validation' # Here you specify the directory to be validated.
```

👀 Check [Oxygen Scripting - Validation template](https://github.com/oxygenxml/oxygen-script-validation-template) for a ready-to-use template with sample validation files.

# Deployment to GitHub Pages

After a successful run of the <i>Oxygen Validation Script</i>, a "validationReport.html" file is created on the <i>gh-pages</i> branch. If you want this report to be published to GitHub Pages, all you have to do is go to Settings &rarr; Pages, and under <i>Build and deployment</i> section select the <i>gh-pages</i> branch instead of the <i>main</i> branch. 

The deployment workflow should automatically start and the report should be available shortly at: https://{userid}.github.io/{reponame}/validationReport.html.

