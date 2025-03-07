---
subcategory: "DocDB (DocumentDB)"
layout: "aws"
page_title: "AWS: aws_docdb_cluster_instance"
description: |-
  Provides an DocDB Cluster Resource Instance
---

# Resource: aws_docdb_cluster_instance

Provides an DocDB Cluster Resource Instance. A Cluster Instance Resource defines
attributes that are specific to a single instance in a [DocDB Cluster][1].

You do not designate a primary and subsequent replicas. Instead, you simply add DocDB
Instances and DocDB manages the replication. You can use the [count][3]
meta-parameter to make multiple instances and join them all to the same DocDB
Cluster, or you may specify different Cluster Instance resources with various
`instance_class` sizes.

## Example Usage

```terraform
resource "aws_docdb_cluster_instance" "cluster_instances" {
  count              = 2
  identifier         = "docdb-cluster-demo-${count.index}"
  cluster_identifier = aws_docdb_cluster.default.id
  instance_class     = "db.r5.large"
}

resource "aws_docdb_cluster" "default" {
  cluster_identifier = "docdb-cluster-demo"
  availability_zones = ["us-west-2a", "us-west-2b", "us-west-2c"]
  master_username    = "foo"
  master_password    = "barbut8chars"
}
```

## Argument Reference

For more detailed documentation about each argument, refer to
the [AWS official documentation](https://docs.aws.amazon.com/cli/latest/reference/docdb/create-db-instance.html).

The following arguments are supported:

* `apply_immediately` - (Optional) Specifies whether any database modifications
     are applied immediately, or during the next maintenance window. Default is`false`.
* `auto_minor_version_upgrade` - (Optional) Indicates that minor engine upgrades will be applied automatically to the DB instance during the maintenance window. Default `true`.
* `availability_zone` - (Optional, Computed) The EC2 Availability Zone that the DB instance is created in. See [docs](https://docs.aws.amazon.com/documentdb/latest/developerguide/API_CreateDBInstance.html) about the details.
* `cluster_identifier` - (Required) The identifier of the [`aws_docdb_cluster`](/docs/providers/aws/r/docdb_cluster.html) in which to launch this instance.
* `engine` - (Optional) The name of the database engine to be used for the DocDB instance. Defaults to `docdb`. Valid Values: `docdb`.
* `identifier` - (Optional, Forces new resource) The identifier for the DocDB instance, if omitted, Terraform will assign a random, unique identifier.
* `identifier_prefix` - (Optional, Forces new resource) Creates a unique identifier beginning with the specified prefix. Conflicts with `identifier`.
* `instance_class` - (Required) The instance class to use. For details on CPU and memory, see [Scaling for DocDB Instances][2]. DocDB currently
  supports the below instance classes. Please see [AWS Documentation][4] for complete details.
    - db.r5.large
    - db.r5.xlarge
    - db.r5.2xlarge
    - db.r5.4xlarge
    - db.r5.12xlarge
    - db.r5.24xlarge
    - db.r4.large
    - db.r4.xlarge
    - db.r4.2xlarge
    - db.r4.4xlarge
    - db.r4.8xlarge
    - db.r4.16xlarge
    - db.t3.medium
* `preferred_maintenance_window` - (Optional) The window to perform maintenance in.
  Syntax: "ddd:hh24:mi-ddd:hh24:mi". Eg: "Mon:00:00-Mon:03:00".
* `promotion_tier` - (Optional) Default 0. Failover Priority setting on instance level. The reader who has lower tier has higher priority to get promoter to writer.
* `tags` - (Optional) A map of tags to assign to the instance. If configured with a provider [`default_tags` configuration block](https://registry.terraform.io/providers/hashicorp/aws/latest/docs#default_tags-configuration-block) present, tags with matching keys will overwrite those defined at the provider-level.

## Attributes Reference

In addition to all arguments above, the following attributes are exported:

* `arn` - Amazon Resource Name (ARN) of cluster instance
* `db_subnet_group_name` - The DB subnet group to associate with this DB instance.
* `dbi_resource_id` - The region-unique, immutable identifier for the DB instance.
* `endpoint` - The DNS address for this instance. May not be writable
* `engine_version` - The database engine version
* `kms_key_id` - The ARN for the KMS encryption key if one is set to the cluster.
* `port` - The database port
* `preferred_backup_window` - The daily time range during which automated backups are created if automated backups are enabled.
* `storage_encrypted` - Specifies whether the DB cluster is encrypted.
* `tags_all` - A map of tags assigned to the resource, including those inherited from the provider [`default_tags` configuration block](https://registry.terraform.io/providers/hashicorp/aws/latest/docs#default_tags-configuration-block).
* `writer` – Boolean indicating if this instance is writable. `False` indicates this instance is a read replica.
* `ca_cert_identifier` - (Optional) The identifier of the CA certificate for the DB instance.

[1]: /docs/providers/aws/r/docdb_cluster.html
[2]: https://docs.aws.amazon.com/documentdb/latest/developerguide/db-cluster-manage-performance.html#db-cluster-manage-scaling-instance
[3]: https://www.terraform.io/docs/configuration/meta-arguments/count.html
[4]: https://docs.aws.amazon.com/documentdb/latest/developerguide/db-instance-classes.html#db-instance-class-specs

## Timeouts

[Configuration options](https://developer.hashicorp.com/terraform/language/resources/syntax#operation-timeouts):

- `create` - (Default `90m`)
restoring from Snapshots
- `update` - (Default `90m`)
- `delete` - (Default `90m`)
the time required to take snapshots

## Import

DocDB Cluster Instances can be imported using the `identifier`, e.g.,

```
$ terraform import aws_docdb_cluster_instance.prod_instance_1 aurora-cluster-instance-1
```
