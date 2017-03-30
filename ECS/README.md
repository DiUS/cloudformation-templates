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

* Considering the security of the container instances.
* Building log shipping into the cluster, either native CloudWatch Logs or
a log shipper like Sumo Logic or Splunk

