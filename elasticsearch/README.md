<img align="right" width="auto" height="auto" src="https://www.elastic.co/static-res/images/elastic-logo-200.png">

# Elasticsearch Github Action

![Elasticsearch Github Action](https://github.com/Loeffelhardt/el-elastic-github-actions/workflows/Elasticsearch%20Github%20Action/badge.svg)  ![Stability:experimental](https://img.shields.io/badge/stability-experimental-orange)

This action spins up an Elasticsearch instance that can be accessed and used in your subsequent steps.

___

**NOTE:** This action is still under active development, and it is not yet recommended to use it in your workflows.
___

## Inputs

| Name                     | Required | Default  | Description                                                                                                                               |
|--------------------------|----------|----------|-------------------------------------------------------------------------------------------------------------------------------------------|
| `stack-version`          | Yes      |          | The version of the Elastic Stack you need to use, you can use any version present in [docker.elastic.co](https://www.docker.elastic.co/). |
| `security-enabled`       | No       | true     | Only available in v8. Set to `false` to disable https and basic authentication                                                            |
| `nodes`                  | No       | 1        | Number of nodes in the cluster.                                                                                                           |
| `port`                   | No       | 9200     | Port where you want to run Elasticsearch.                                                                                                 |
| `elasticsearch_password` | No       | changeme | The password for the user elastic in your cluster                                                                                         |
| `wait`                   | No       | 10       | Number of seconds to wait after launch.                                                                                                   |
| `plugins`                | No       |          | Any plugins you want to include                                                                                                           |
| `network-name`           | No       | elastic  | Custom name for the network created by Docker                                                                                             |
| `container-name`         | No       | es       | Custom name for the container created by Docker                                                                                           |

## Usage

You *must* also add the `Configure sysctl limits` step, otherwise Elasticsearch will not be able to boot.

```yml
- name: Configure sysctl limits
  run: |
    sudo swapoff -a
    sudo sysctl -w vm.swappiness=1
    sudo sysctl -w fs.file-max=262144
    sudo sysctl -w vm.max_map_count=262144

- name: Runs Elasticsearch
  uses: elastic/elastic-github-actions/elasticsearch@master
  with:
    stack-version: 7.6.0
```

### Disable security

Disabling security is not recommended, however, for testing purposes, you can do it with:

```yml
- name: Configure sysctl limits
  run: |
    sudo swapoff -a
    sudo sysctl -w vm.swappiness=1
    sudo sysctl -w fs.file-max=262144
    sudo sysctl -w vm.max_map_count=262144

- name: Runs Elasticsearch
  uses: elastic/elastic-github-actions/elasticsearch@master
  with:
    stack-version: 8.2.0
    security-enabled: false
```

## License

This software is licensed under the [Apache 2 license](../LICENSE).
