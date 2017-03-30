# ECS CloudFormation templates

The purpose of these templates is to demonstrate how nested templates can be
used to simplify the orchestration of ECS Clusters and Services. By using
the nested template, each service added to the cluster can be expressed in
less than 30 lines of JSON.

See the associated blog article at https://dius.com.au/blog/...

## How to use

Upload `ecs-cluster.template` and `ecs-service.template` to an S3 bucket
accessible to your account. The example `master.template` uses the nested
templates to stand up a cluster and a single service using path-based
routing and an ALB. (Note that the httpd service used in the example has
no content).

The container instances need to access the AWS APIs, so must be in a subnet
that has Internet access either directly or via a NAT gateway.

While the cluster template is fairly generic, the service template will
probably have to be customised according to the type of containers being
stood up, e.g. Spring, Rails etc.

## Production use

Using this approach for a production workload would require at a minimum:

* Considering the security of the container instances. Preferably they
should be located in a private subnet with ssh access locked down to trusted
sources.
* Building log shipping into the cluster, either native CloudWatch Logs or
a log shipper like Sumo Logic or Splunk

## Template Outputs ##

### ecs-cluster.template ###

| Output                          | Purpose                                                                                                                                                   |
|---------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| Cluster                         | Passed into the service template.                                                                                                                         |
| Listener                        | Passed into the service template.                                                                                                                         |
| ApplicationLoadBalancerEndpoint | DNS name of the load balancer.                                                                                                                            |
| SecurityGroup                   | Provided such that security groups for other network-based services can be constructed. For example, to allow access to an RDS instance from a container. |

### ecs-service.template ###

| Output     | Purpose                                                                                     |
|------------|---------------------------------------------------------------------------------------------|
| ServiceArn | In conjunction with the cluster, can be used to manipulate the service via the CLI and API. |