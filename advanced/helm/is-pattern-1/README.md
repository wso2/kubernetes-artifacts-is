# Helm Chart for deployment of WSO2 Identity Server 

## Contents

* [Prerequisites](#prerequisites)
* [Quick Start Guide](#quick-start-guide)

## Prerequisites

* In order to use WSO2 Helm resources, you need an active WSO2 subscription. If you do not possess an active WSO2
  subscription already, you can sign up for a WSO2 Free Trial Subscription from [here](https://wso2.com/free-trial-subscription)
  . Otherwise you can proceed with docker images which are created using GA releases.<br><br>

* Install [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git), [Helm](https://github.com/kubernetes/helm/blob/master/docs/install.md)
(and Tiller) and [Kubernetes client](https://kubernetes.io/docs/tasks/tools/install-kubectl/) (compatible with v1.10) in order to run the 
steps provided in the following quick start guide.<br><br>

* An already setup [Kubernetes cluster](https://kubernetes.io/docs/setup/pick-right-solution/).<br><br>

* Install [NGINX Ingress Controller](https://kubernetes.github.io/ingress-nginx/deploy/). This can
 be easily done via 
  ```
  helm install stable/nginx-ingress --name nginx-wso2is --set rbac.create=true
  ```
  
## Quick Start Guide
>In the context of this document, <br>
>* `KUBERNETES_HOME` will refer to a local copy of the [`wso2/kubernetes-is`](https://github.com/wso2/kubernetes-is/)
Git repository. <br>
>* `HELM_HOME` will refer to `<KUBERNETES_HOME>/advanced/helm`. <br>

##### 1. Clone the Kubernetes Resources for WSO2 Identity Server Git repository.

```
git clone https://github.com/wso2/kubernetes-is.git
```

##### 2. Provide configurations.

a. The default product configurations are available at `<HELM_HOME>/is-pattern-1/confs` folder. Change the
configurations as necessary.

b. Open the `<HELM_HOME>/is-pattern-1/values.yaml` and provide the following values.

###### MySQL Configurations

| Parameter                                                                   | Description                                                                               | Default Value               |
|-----------------------------------------------------------------------------|-------------------------------------------------------------------------------------------|-----------------------------|
| `wso2.mysql.enabled`                                                        | Enable MySQL chart as a dependency                                                        | true                        |
| `wso2.mysql.host`                                                           | Set MySQL server host                                                                     | wso2ei-rdbms-service-mysql  |
| `wso2.mysql.username`                                                       | Set MySQL server username                                                                 | wso2carbon                  |
| `wso2.mysql.password`                                                       | Set MySQL server password                                                                 | wso2carbon                  |
| `wso2.mysql.driverClass`                                                    | Set JDBC driver class for MySQL                                                           | com.mysql.jdbc.Driver       |
| `wso2.mysql.validationQuery`                                                | Validation query for the MySQL server                                                     | SELECT 1                    |

###### WSO2 Subscription Configurations

| Parameter                                                                   | Description                                                                               | Default Value               |
|-----------------------------------------------------------------------------|-------------------------------------------------------------------------------------------|-----------------------------|
| `wso2.subscription.username`                                                | Your WSO2 Subscription username                                                           | ""                          |
| `wso2.subscription.password`                                                | Your WSO2 Subscription password                                                           | ""                          |

If you do not have active WSO2 subscription do not change the parameters `wso2.subscription.username`, `wso2.subscription.password`. 

###### Centralized Logging Configurations

| Parameter                                                                   | Description                                                                               | Default Value               |
|-----------------------------------------------------------------------------|-------------------------------------------------------------------------------------------|-----------------------------|
| `wso2.centralizedLogging.enabled`                                           | Enable Centralized logging for WSO2 components                                            | true                        |                                                                                         |                             |    
| `wso2.centralizedLogging.logstash.imageTag`                                 | Logstash Sidecar container image tag                                                      | 7.2.0                       |  
| `wso2.centralizedLogging.logstash.elasticsearch.username`                   | Elasticsearch username                                                                    | elastic                     |  
| `wso2.centralizedLogging.logstash.elasticsearch.password`                   | Elasticsearch password                                                                    | changeme                    |  
| `wso2.centralizedLogging.logstash.indexNodeID.wso2ISNode`                   | Elasticsearch IS Node log index ID(index name: ${NODE_ID}-${NODE_IP})                     | wso2                        |

###### Identity Server Configurations

| Parameter                                                                   | Description                                                                               | Default Value               |
|-----------------------------------------------------------------------------|-------------------------------------------------------------------------------------------|-----------------------------|
| `wso2.deployment.wso2is.imageName`                                          | Image name for IS node                                                                    | wso2ei                      |
| `wso2.deployment.wso2is.imageTag`                                           | Image tag for IS node                                                                     | 6.5.0                       |
| `wso2.deployment.wso2is.replicas`                                           | Number of replicas for IS node                                                            | 1                           |
| `wso2.deployment.wso2is.minReadySeconds`                                    | Refer to [doc](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.10/#deploymentspec-v1-apps)| 1  75                        |
| `wso2.deployment.wso2is.strategy.rollingUpdate.maxSurge`                    | Refer to [doc](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.10/#deploymentstrategy-v1-apps) | 1                           |
| `wso2.deployment.wso2is.strategy.rollingUpdate.maxUnavailable`              | Refer to [doc](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.10/#deploymentstrategy-v1-apps) | 0                           |
| `wso2.deployment.wso2is.livenessProbe.initialDelaySeconds`                  | Initial delay for the live-ness probe for IS node                                         | 40                           |
| `wso2.deployment.wso2is.livenessProbe.periodSeconds`                        | Period of the live-ness probe for IS node                                                 | 10                           |
| `wso2.deployment.wso2is.readinessProbe.initialDelaySeconds`                 | Initial delay for the readiness probe for IS node                                         | 40                           |
| `wso2.deployment.wso2is.readinessProbe.periodSeconds`                       | Period of the readiness probe for IS node                                                 | 10                           |
| `wso2.deployment.wso2is.imagePullPolicy`                                    | Refer to [doc](https://kubernetes.io/docs/concepts/containers/images#updating-images)     | Always                       |
| `wso2.deployment.wso2is.resources.requests.memory`                          | The minimum amount of memory that should be allocated for a Pod                           | 1Gi                          |
| `wso2.deployment.wso2is.resources.requests.cpu`                             | The minimum amount of CPU that should be allocated for a Pod                              | 2000m                        |
| `wso2.deployment.wso2is.resources.limits.memory`                            | The maximum amount of memory that should be allocated for a Pod                           | 2Gi                          |
| `wso2.deployment.wso2is.resources.limits.cpu`                               | The maximum amount of CPU that should be allocated for a Pod                              | 2000m                        |

**Note**: The above mentioned default, minimum resource amounts for running WSO2 Enterprise Integrator server profiles are based on its [official documentation](https://docs.wso2.com/display/IS580/Installation+Prerequisites).

##### 3. Deploy WSO2 Identity server.

```
helm install --dep-up --name <RELEASE_NAME> <HELM_HOME>/is-pattern-1 --namespace <NAMESPACE>
```

`NAMESPACE` should be the Kubernetes Namespace in which the resources are deployed

##### 4. Access Management Console.

Default deployment will expose `wso2is` host (to expose Administrative services and Management Console).

To access the console in the environment,

a. Obtain the external IP (`EXTERNAL-IP`) of the Ingress resources by listing down the Kubernetes Ingresses.

```
kubectl get ing -n <NAMESPACE>
```

```
NAME                       HOSTS          ADDRESS        PORTS     AGE
wso2is-ingress             wso2is         <EXTERNAL-IP>  80, 443   3m
```

b. Add the above host as an entry in /etc/hosts file as follows:

```
<EXTERNAL-IP>	wso2is
```

c. Try navigating to `https://wso2is/carbon` from your favorite browser.