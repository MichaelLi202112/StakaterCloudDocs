# Custom Resources

1. EnvironmentProvisioner

```yaml
apiVersion: tronador.stakater.com/v1alpha1
kind: EnvironmentProvisioner
metadata:
  name: stakater-nordmart-promotion-pr-12
spec:
  application:
    release:
      chart:
        git: 'https://github.com/stakater-lab/stakater-nordmart-promotion'
        path: deploy/
        ref: add-tronador-yaml
      releaseName: add-tronador-yaml
      valuesFrom:
        - chartFileRef:
            path: values.yaml
  sourceBranch: add-tronador-yaml
  targetBranch: main
  triggeringRepoURL: 'https://github.com/stakater-lab/stakater-nordmart-promotion'
```
Defines the `application`, `sourceBranch`, `targetBranch` and `triggeringRepoURL` of the `EnvironmentProvisioner` resource.

* `application` portion will contain details about the git repo and its helm charts
* `sourceBranch` will list the name of the branch from the PR that needs to be merged. `EnvironmentProvisioner` will create a helm release based on this branch.
* `targetBranch` will list the branch where the PR needs to be merged into
* `triggeringRepoURL` links to the git repository from where to pull the application from

Values within `application` are provided from within the relevant branch inside the git repo, defined by `triggeringRepoURL` and `sourceBranch`. Within the branch, there needs to be a configuration file called `.tronador.yaml`, which is then used to fill in these details. The configuration file is defined in this format:

```yaml
version: v1alpha1
targetBranches:
  - main
application:
  chartPath: "deploy/"
  chartVarsPath:
    - "values.yaml"
dependencies:
  repositories:
    - name: backend
      repository: github/stakater-lab
      chartPath: deploy
      chartVarsRepoPath:
      - "values.yaml"
      valueOverrides:
      - "some.value=foobar"
  charts:
    - name: wordpress
      chartRepoPath: https://charts.bitnami.com/bitnami
      version: 12.0.3
      requires:
      - nginx
    - name: nginx
      chartRepoPath: https://charts.bitnami.com/bitnami
      version: 9.4.2
```
| Field | Description |
|-|-|
| `TargetBranches`| refers to the branch towards which the PR will merge into |
| `application.chartPath` | location of the helm chart within the repo |
| `application.chartVarsPath` | location and name of the "values" file within the helm chart|
| `application.chartRepoPath` |  |
| `application.chartVarsRepoPath` |  |
| `application.valueOverrides` | Array of values to override within the repository's helm chart |
| `dependencies.repositories` | Array of dependencies of type 'repository'. |
| `dependencies.charts` |  |