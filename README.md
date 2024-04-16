# Olympix Integrated Security

## Overview

The Olympix Integrated Security action enables Olympix's vulnerability analysis tools to be incorporated into continuous integration workflows for code repositories on GitHub. The action currently performs code analysis on projects written in Solidity and has flexible options for results output, using the SARIF format by default. By using this action, Solidity developers can find potentially dangerous vulnerabilities in their smart contracts when the CI workflow runs.


![vulnerabilities](https://github.com/olympix/integrated-security/blob/main/img/vulnerabilities.PNG)


## Features

- **Code Scanning:** Quickly scan your GitHub-based project for vulnerabilities
- **Detailed Results:** View detailed results in different formats, directly on the GitHub workflow console or using the GitHub Code Scanning tool
- **Customizable Rules:** Customize the scanning rules to fit your requirements

## Getting Started

1. Add a GitHub repository secret with `OLYMPIX_API_TOKEN` as the name and your API token as the value
2. Add the `olympix/integrated-security` GitHub action into your workflow
3. (Optional) If necessary, customize the scanning rules using the input `args`

## Usage

Here's a workflow example that utilizes the Olympix Integrated Security action with default rules and uploads the result to the GitHub Code Scanning tool (It is necessary to enable `Read and write permissions` on GitHub `Settings > Actions > General > Workflow permissions` to upload result to GitHub Code Scanning).

```shell
name: Integrated Security Workflow
on: push
jobs:
  security:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3
      
      - name: Run Olympix Integrated Security
        uses: olympix/integrated-security@main
        env:
          OLYMPIX_API_TOKEN: ${{ secrets.OLYMPIX_API_TOKEN }}

      - name: Upload result to GitHub Code Scanning
        uses: github/codeql-action/upload-sarif@v2
        with:
          sarif_file: olympix.sarif
```

<br/>

![vulnerability_detail](https://github.com/olympix/integrated-security/blob/main/img/vulnerability_detail.PNG)

Here's a workflow example that utilizes the Olympix Integrated Security action with `json` result to the GitHub console, and excludes `uninitialized state variable` and `default visibility` vulnerabilities.

```shell
name: Integrated Security Workflow
on: push
jobs:
  security:
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v3
      
      - name: Olympix Integrated Security
        uses: olympix/integrated-security@main
        env:
          OLYMPIX_API_TOKEN: ${{ secrets.OLYMPIX_API_TOKEN }}
        with:
          args: -f json --no-uninitialized-state-variable --no-default-visibility
```

<br/>

![vulnerabilities_json](https://github.com/olympix/integrated-security/blob/main/img/vulnerability_json_output.PNG)

<br/>

## Analysis Options

- `-w | --workspace-path`: Defines the root project directory path. It is important to know the project context to provide more accurate vulnerabilities analysis. The default is the current directory
- `-p | --path`: Defines the Solidity project directory path to be analyzed. It can be used multiple times to include each project analysis directory. The default is the 'contracts' and 'src' directories if they exist, otherwise it is the same directory path of workspace
- `-f | --output-format`: Defines result output format. The supported currently formats are: `tree`, `json`, `sarif` and `email`. The default is `tree`
- `-o | --output-path`: Defines result output directory path (Enabled only for `json` and `sarif` formats). The default is showing result to terminal
- `--no-<vulnerability id>`: Defines the vulnerabilities that may be ignored. It can be used multiple times to ignore each vulnerability type. The default ignores nothing

<br/>

## Support Contact

If you have any question, feedback, or need help, feel free to contact us at contact@olympix.ai
