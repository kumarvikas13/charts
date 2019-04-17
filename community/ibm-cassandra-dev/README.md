# Cassandra
A Cassandra Chart for Kubernetes

## Install Chart
To install the Cassandra Chart into your Kubernetes cluster (This Chart requires persistent volume by default, you may need to create a storage class before install chart. To create storage class, see [Persist data](#persist_data) section)

## Introduction

This chart bootstraps a [Cassandra] (https://hub.docker.com/_/cassandra) deployment on a [Kubernetes](http://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager.

## Prerequisites
- Kubernetes 1.4+ with Beta APIs enabled
- PV provisioner support in the underlying infrastructure

## PodSecurityPolicy Requirements
This chart requires a PodSecurityPolicy to be bound to the target namespace prior to installation. Choose predefined ibm-restricted-psp PodSecurityPolicy.

## Resources Required
The chart deploys pods consuming minimum resources as specified in the resources configuration parameter

## Chart Details
This chart will deploy Cassandra.

## Limitations

## Installing the Chart

To install the chart with the release name `my-release`:

```console
$ helm install --name my-release community/ibm-cassandra-dev
```

The command deploys Cassandra on the Kubernetes cluster in the default configuration. The [configuration](#configuration) section lists the parameters that can be configured during installation.

> **Tip**: List all releases using `helm list`

```bash
helm install --namespace "cassandra" -n "cassandra" community/ibm-cassandra-dev
```

After installation succeeds, you can get a status of Chart

```bash
helm status "cassandra"
```

If you want to delete your Chart, use this command
```bash
helm delete  --purge "cassandra"
```

## Persist data
You need to create `StorageClass` before able to persist data in persistent volume.
To create a `StorageClass` on Google Cloud, run the following

```bash
kubectl create -f sample/create-storage-gce.yaml
```

And set the following values in `values.yaml`

```yaml
persistence:
  enabled: true
```

If you want to create a `StorageClass` on other platform, please see documentation here [https://kubernetes.io/docs/user-guide/persistent-volumes/](https://kubernetes.io/docs/user-guide/persistent-volumes/)

When running a cluster without persistence, the termination of a pod will first initiate a decommissioning of that pod.
Depending on the amount of data stored inside the cluster this may take a while. In order to complete a graceful
termination, pods need to get more time for it. Set the following values in `values.yaml`:

```yaml
podSettings:
  terminationGracePeriodSeconds: 1800
```

## Install Chart with specific cluster size
By default, this Chart will create a cassandra with 3 nodes. If you want to change the cluster size during installation, you can use `--set config.cluster_size={value}` argument. Or edit `values.yaml`

For example:
Set cluster size to 5

```bash
helm install --namespace "cassandra" -n "cassandra" --set config.cluster_size=5 community/ibm-cassandra-dev
```

## Install Chart with specific resource size
By default, this Chart will create a cassandra with CPU 2 vCPU and 4Gi of memory which is suitable for development environment.
If you want to use this Chart for production, I would recommend to update the CPU to 4 vCPU and 16Gi. Also increase size of `max_heap_size` and `heap_new_size`.
To update the settings, edit `values.yaml`

## Configuration

The following table lists the configurable parameters of the Cassandra chart and their default values.

| Parameter                  | Description                                     | Default                                                    |
| -----------------------    | ---------------------------------------------   | ---------------------------------------------------------- |
| `image.repo`                         | Docker image location                           | `cassandra`                                                |
| `image.tag`                          | Docker image tag                                | `3`                                                   |
| `image.pullPolicy`                   | Defaults to 'Always' when the latest tag is specified. Otherwise the default is 'IfNotPresent'.                               | `Always` if `imageTag` is `latest`, else `IfNotPresent`    |
| `service.type`                       | Type of service               | `NodePort` 
| `persistence.enabled`                | (Recommended) Select this checkbox to store data on persistent volumes                      | `true`  |
| `persistence.UseDynamicProvisioning` | Use dynamically provisioned persistent volumes to store Mesos data  | `false` |                
| `persistence.accessMode`             | Access mode details             | `ReadWriteOnce`                                            
| `persistence.size`                   | Specify the size of the data storage volume                             | `10Gi`  
| `persistence.storageClass`           | Set this value if you want to use only a specific storage class for dynamic provisioning.                    | `nil` (uses alpha storage class annotation)
| `resources`                          | Resource requests and limits configuration             | Memory: `4Gi`, CPU: `2`                                        
| `config.cluster_domain`              | Cluster domain name.                 | `cluster.local`                                            |
| `config.cluster_name`                | Cassandra cluster Name.                        | `cassandra`                                                |
| `config.cluster_size`                | Cassandra cluster Size.             | `3`                                                        |
| `config.seed_size`                   | To configure seeds value of the seed_provider option.                          | `2` |                                         |
| `config.dc_name`                     | Sets the datacenter name of the node                               | `DC1`                                                      |
| `config.rack_name`                   | Sets the rack name of the node                          | `RAC1`                                                     |
| `config.endpoint_snitch`             | Sets the snitch implementation the node will use                               | `SimpleSnitch`                                             |
| `config.max_heap_size`               | Sets the max heap size                               | `2048M`                                                    |
| `config.heap_new_size`               | Sets the new heap size                            | `512M`                              
| `config.start_rpc`                   | If the thrift rpc server is to be started                         | `false`                                                    |
| `config.ports.agent`                 | Sets the agent port              | `nil`                                                      |
| `config.ports.cql`                   | Sets the CQL port                                | `9042`                                                     |
| `config.ports.thrift`                | Set the thrift RPC server port                              | `9160`                                                     |                                  |                                        |
| `configOverrides`                    | Path for custom Cassandra config file    | `{}`                                                       |
| `commandOverrides`                   | To override containers startup command               | `[]`                                                       |
| `argsOverrides`                      | To override containers startup command options              | `[]`                                                       |
| `env`                                | Provide additional environment variables                            | `{}`             |                                          
| `livenessProbe.initialDelaySeconds`  | Number of seconds after the container has started before liveness probe is initiated        | `90`                                                       |
| `livenessProbe.periodSeconds`        | How often (in seconds) to perform the probe                  | `30`                                                       |
| `livenessProbe.timeoutSeconds`       | Number of seconds after which the probe times out.                       | `5`                                                        |
| `livenessProbe.successThreshold`     | Minimum consecutive successes for the probe to be considered successful after having failed.           | `1` |
| `livenessProbe.failureThreshold`     | Number of failures to accept before giving up and marking the pod as Unready.             | `3` |
| `readinessProbe.initialDelaySeconds` | Number of seconds after the container has started before the probe is initiated.      | `90`                                                       |
| `readinessProbe.periodSeconds`       | How often (in seconds) to perform the probe                  | `30`                                                       |
| `readinessProbe.timeoutSeconds`      | Number of seconds after which the probe times out.                      | `5`                                                        |
| `readinessProbe.successThreshold`    | Minimum consecutive successes for the probe to be considered successful after having failed.           | `1` |
| `readinessProbe.failureThreshold`    | Number of failures to accept before giving up and restarting the.             | `3` |
| `podAnnotations`                     | Pod Annotations             | `{}`                                                       
| `affinity`                           | Affinity for pod assignment                        | `{}`                                                       |
| `tolerations`                        | Toleration labels for pod assignment, e.g. [{\"key\": \"key\", \"operator\":\"Equal\", \"value\": \"value\", \"effect\":\"NoSchedule\"}]                    | `[]`                                                       |
| `podSettings.terminationGracePeriodSeconds`                        |Grace period before a pod is terminated                   |`{}`
| `podDisruptionBudget`                | Pod Disruption Budget settings                        | `{}`                                                       |
| `podManagementPolicy`                | Management of order in which pods are created and deleted          | `OrderedReady`   
| `updateStrategy.type`                | Update stratergy type              | `OnDelete`                                                 

## Scale cassandra
When you want to change the cluster size of your cassandra, you can use the helm upgrade command.

```bash
helm upgrade --set config.cluster_size=5 cassandra community/ibm-cassandra-dev
```

## Get cassandra status
You can get your cassandra cluster status by running the command

```bash
kubectl exec -it --namespace cassandra $(kubectl get pods --namespace cassandra -l app=cassandra-cassandra -o jsonpath='{.items[0].metadata.name}') nodetool status
```

Output
```bash
Datacenter: asia-east1
======================
Status=Up/Down
|/ State=Normal/Leaving/Joining/Moving
--  Address    Load       Tokens       Owns (effective)  Host ID                               Rack
UN  10.8.1.11  108.45 KiB  256          66.1%             410cc9da-8993-4dc2-9026-1dd381874c54  a
UN  10.8.4.12  84.08 KiB  256          68.7%             96e159e1-ef94-406e-a0be-e58fbd32a830  c
UN  10.8.3.6   103.07 KiB  256          65.2%             1a42b953-8728-4139-b070-b855b8fff326  b
```

## Benchmark
You can use [cassandra-stress](https://docs.datastax.com/en/cassandra/3.0/cassandra/tools/toolsCStress.html) tool to run the benchmark on the cluster by the following command

```bash
kubectl exec -it --namespace cassandra $(kubectl get pods --namespace cassandra -l app=cassandra-cassandra -o jsonpath='{.items[0].metadata.name}') cassandra-stress
```

Example of `cassandra-stress` argument
 - Run both read and write with ration 9:1
 - Operator total 1 million keys with uniform distribution
 - Use QUORUM for read/write
 - Generate 50 threads
 - Generate result in graph
 - Use NetworkTopologyStrategy with replica factor 2

```bash
cassandra-stress mixed ratio\(write=1,read=9\) n=1000000 cl=QUORUM -pop dist=UNIFORM\(1..1000000\) -mode native cql3 -rate threads=50 -log file=~/mixed_autorate_r9w1_1M.log -graph file=test2.html title=test revision=test2 -schema "replication(strategy=NetworkTopologyStrategy, factor=2)"
```
## Support

The helm charts are provided "as-is" and without warranty of any kind.

All helm charts and packages are supported through standard open source forums and helm charts are updated on a best effort basis.

Any issues found can be reported through the links below, and fixes may be proposed/submitted using standard git issues as noted below.

[Submit issue to Helm Chart] ( https://github.com/ppc64le/charts/issues )

[Submit issue to Cassandra docker image] ( https://hub.docker.com/_/cassandra )

[Submit issue to helm open source community] ( https://github.com/helm/helm/issues )

## Note

This chart is validated on ppc64le.