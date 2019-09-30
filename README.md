# Federacy Salus Security Scan Action 

This action utilizes [Salus](https://github.com/coinbase/salus) to run SAST and Dependency scans. 

## Inputs

| attribute | description | default | options |
| --------- | ----------- | ------- | ------- |
| active_scanners | Scanners to run | all | Brakeman, PatternSearch, BundleAudit, NPMAudit, GoSec |
| enforced_scanners | Scanners that block builds | all | Brakeman, PatternSearch, BundleAudit, NPMAudit |
| report_uri | Where to send Salus reports | file://../salus-report.json | Any URI |
| report_format | What format to use for report | json | json, yaml, txt |
| report_verbosity | Whether to enable a verbose report | true | true, false |
| configuration_file | Location of config file in repo (overrides all other parameters except salus_executor) | "" | Any filename |

Note: active_scanners and enforced_scanners must be yaml formatted for Salus configuration file.

## Outputs

None.

## Github Environment Variables

Stored in custom_info of a Salus scan.

| Key | Github Variable | Description |
| --- | ----------------- | ----------- |
| sha1    | GITHUB_SHA | Hash of last commit in build |
| reponame | GITHUB_REPOSITORY | Name of repository |
| ref | GITHUB_REF | Ref that triggered flow (branch or tag) |
| ci_username | GITHUB_ACTOR | Github username of user who triggered build |
| github_action | GITHUB_ACTION | Name of the action |
| github_workflow | GITHUB_WORKFLOW | Name of the workflow |
| github_event_name | GITHUB_EVENT_NAME | Name of the event that triggered workflow |
| github_event_path | GITHUB_EVENT_PATH | Path of event payload |
| github_workspace | GITHUB_WORKSPACE | Workspace directory path |
| github_head_ref | GITHUB_HEAD_REF | Ref of the head repository, if forked |
| github_base_ref | GITHUB_BASE_REF | Ref of the base repository, if forked |
| github_home | HOME | Path to home directory used by Github |

## Example usage

### Defaults

```
on: [push]

jobs:
  salus_scan_job:
    runs-on: ubuntu-latest
    name: Federacy Salus Security Scan
    steps:
    - uses: actions/checkout@v1
    - name: Federacy Salus scan
      id: salus_scan
      uses: ./ # needs to be modified when Action is pushed publicly
```

### Report only

```
on: [push]

jobs:
  salus_scan_job:
    runs-on: ubuntu-latest
    name: Federacy Salus Security Scan
    steps:
    - uses: actions/checkout@v1
    - name: Federacy Salus scan
      id: salus_scan
      uses: ./ # needs to be modified when Action is pushed publicly
      with:
          active_scanners: "\n  - Brakeman"
          enforced_scanners: "\n  - Brakeman"
```

