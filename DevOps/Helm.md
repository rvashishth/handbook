<h2>Helm Crash Course </h2>

- [Key concepts and components](#key-concepts-and-components)
  - [Repository](#repository)
- [Commands/functionalities](#commandsfunctionalities)
  - [helm repo](#helm-repo)
  - [helm install](#helm-install)
  - [helm upgrade](#helm-upgrade)
  - [Helm template](#helm-template)
  - [other commands](#other-commands)
  - [helm package](#helm-package)
- [Go templates for helm charts](#go-templates-for-helm-charts)
- [Status/lifecycles of a release/repo](#statuslifecycles-of-a-releaserepo)
- [Questions](#questions)

## Key concepts and components

- **Chart** - is a helm package

### Repository

A chart repository is an HTTP server that houses an index.yaml file and optionally some packaged charts

- **release** - an instance of chart running on k8s
  - one chart can be installed multiple type on same cluster
  - each installation creates a new release
- helm uses go templates to create charts. using go template we provide values to k8s manifest files
- during upgrades, templates are re-executed. keep extra precaution while using randomly generated value or data
- both install an release can be combined in one command
  - `$ helm upgrade --install <release name> --values <values file> <chart directory>`
  - this will either upgrade or install new

## Commands/functionalities

### helm repo

- Add a hlem repo </br>
  `$ helm repo add pulsar https://github.optum.com/pages/link/apache-pulsar-chart/`
- Print the list of repo </br>
  `helm repo list`
- Print the list of charts in a repo </br>
- `helm repo update` will update all repo
- `helm repo remove local_repo_name`
- we can add multiple repo in local
- `helm search repo helm-virtual/pulsar -l --devel`

### helm install

- When we install a chart, it creates a release. Release name can be dynamic or pre-defined.
- A same chart with same release name can not be installed twice. </br>
  `helm install release_name chart_files` </br>
  `helm install pulsar ./pulsar-1.0.0.tgz` </br>
  `helm install pulsar https://github.com/pages/link/chart/stable/pulsar-1.0.0.tgz`

### helm upgrade

`helm upgrade pulsar --debug --install --create-namespace --namespace link -f examples/values-one-node.yaml charts/pulsar`

- with `--install` helm upgrade can install a chart if it is not already installed.
- upgrading a chart version is not necessary, even if the chart version is same, `helm upgrade` will still upgrade the updated k8s resources

### Helm template 

`$ helm template  helm-virtual/spring-api  --version 0.1.0 --devel -f values.yml -f values.pr.yml`
`$ helm template tempReleaseName helm-virtual/spring-api  --version 0.1.0 --devel -f values.yml -f values.pr.yml`

### other commands

- **customize chart values before installing** to provide dynamic values.
  - `helm show values stable/mysql` this will display default value for each config param for this chart
  - `helm install -f myconfig.yaml stable/mysql --generate-name`
    - multiple files can be passed but only rightmost will be used
    - individual values can also be used `--value` attribute
    - we can also overrride the config file on command line using `--set`
    - if both `--set` and `--values` are used, both get merged and `--set` is preferred over `--values`
- **helm rollback**
  - we can rollback to multiple previous release
  - `helm rollback release-name version`
  - on each install, upgrade, rollback release version increment by 1
- **helm uninstall**
  - `helm uninstall release-name`
  - it's important to keep history with uninstalling the release
  - `helm uninstall release-name --keep-history`
- **helm list**
  - `helm list` list out all release in the current k8s cluster
  - or use `helm list --uninstalled` this will only list the releases uninstalled with `--keep-history` attribute
  - `helm list --all`
- **helm create**

  `helm create folder_name` this will generate chart files with default templates

- **helm lint**

### helm package

first navigate to the parent folder chart folder. run `helm package chart_folder_name` this would generate a tar file with chart name and version mentioned in Chart.yaml file

## Go templates for helm charts

- `{{ -if }}`
- `{{ -with }}` `{{ -end }}`
- `{{ -include }}`
- `{{ -toYaml }}`
- `{{ include }}` and `{{ required }}` are additional template function by helm. where include function allows to bring in another tempalte
- Go template pipeline `{{ include "toYaml" $value | indent 2 }}` this will include another template called `toYaml` provides it `$value` and the apply `indent` function to output
-

## Status/lifecycles of a release/repo

- DEPLOYED
- UNINSTALLED when a release is uninstalled with `--keep-history` attribute

## Questions

- [ ] if i re-run helm install with a release name, does it create two parallel installation? </br>
      **Answer:** one release can not be installed twice.
- [ ] if a chart is not already installed, and we run `helm upgrade` does it create a new release? </br>
      **Answer:** one release can not be installed twice.
      no, it will ask for the chart to be installed first.
- [ ] when we run `helm upgrade` without updating chart version, will it update the k8s resources </br>
      **Answer:** one release can not be installed twice.
      yes.
