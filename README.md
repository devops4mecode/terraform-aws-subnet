<p align="center"> <img src="https://user-images.githubusercontent.com/50652676/62349836-882fef80-b51e-11e9-99e3-7b974309c7e3.png" width="100" height="100"></p>


<h1 align="center">
    Terraform AWS Subnet
</h1>

<p align="center" style="font-size: 1.2rem;"> 
    Terraform module to create public, private and public-private subnet with network acl, route table, Elastic IP, nat gateway, flow log.
     </p>

<p align="center">

<a href="https://www.terraform.io">
  <img src="https://img.shields.io/badge/Terraform-v0.13-green" alt="Terraform">
</a>
<a href="LICENSE.md">
  <img src="https://img.shields.io/badge/License-MIT-blue.svg" alt="Licence">
</a>


</p>
<p align="center">

<a href='https://facebook.com/sharer/sharer.php?u=https://github.com/devops4mecode/terraform-aws-subnet'>
  <img title="Share on Facebook" src="https://user-images.githubusercontent.com/50652676/62817743-4f64cb80-bb59-11e9-90c7-b057252ded50.png" />
</a>
<a href='https://www.linkedin.com/shareArticle?mini=true&title=Terraform+AWS+Subnet&url=https://github.com/devops4mecode/terraform-aws-subnet'>
  <img title="Share on LinkedIn" src="https://user-images.githubusercontent.com/50652676/62817742-4e339e80-bb59-11e9-87b9-a1f68cae1049.png" />
</a>
<a href='https://twitter.com/intent/tweet/?text=Terraform+AWS+Subnet&url=https://github.com/devops4mecode/terraform-aws-subnet'>
  <img title="Share on Twitter" src="https://user-images.githubusercontent.com/50652676/62817740-4c69db00-bb59-11e9-8a79-3580fbbf6d5c.png" />
</a>

</p>
<hr>
## Prerequisites

This module has a few dependencies: 

- [Terraform 0.13](https://learn.hashicorp.com/terraform/getting-started/install.html)
- [Go](https://golang.org/doc/install)
- [github.com/stretchr/testify/assert](https://github.com/stretchr/testify)
- [github.com/gruntwork-io/terratest/modules/terraform](https://github.com/gruntwork-io/terratest)

## Examples


**IMPORTANT:** Since the `master` branch used in `source` varies based on new modifications, we suggest that you use the release versions [here](https://github.com/devops4mecode/terraform-aws-subnet/releases).


Here are some examples of how you can use this module in your inventory structure:
### Private Subnet
```hcl
  module "subnets" {
    source              = "devops4mecode/terraform-aws-subnet/aws"
    version             = "1.0.0"
    name                = "subnets"
    application         = "devops4me"
    environment         = "test"
    label_order         = ["environment", "name", "application"]
    availability_zones  = ["ap-southeast-1a", "ap-southeast-1b", "ap-southeast-1c"]
    vpc_id              = "vpc-xxxxxxxxx"
    type                = "private"
    nat_gateway_enabled = true
    cidr_block          = "10.0.0.0/16"
    ipv6_cidr_block     = module.vpc.ipv6_cidr_block
    public_subnet_ids   = ["subnet-XXXXXXXXXXXXX", "subnet-XXXXXXXXXXXXX"]
  }
```

### Public-Private Subnet
```hcl
  module "subnets" {
    source              = "devops4mecode/terraform-aws-subnet/aws"
    version             = "1.0.0"
    name                = "subnets"
    application         = "devops4me"
    environment         = "test"
    label_order         = ["environment", "name", "application"]
    availability_zones  = ["ap-southeast-1a", "ap-southeast-1b", "ap-southeast-1c"]
    vpc_id              = "vpc-xxxxxxxxx"
    type                = "public-private"
    igw_id              = "ig-xxxxxxxxx"
    nat_gateway_enabled = true
    cidr_block          = "10.0.0.0/16"
    ipv6_cidr_block     = module.vpc.ipv6_cidr_block
  }
```

### Public Subnet
```hcl
  module "subnets" {
    source              = "devops4mecode/terraform-aws-subnet/aws"
    version             = "1.0.0"
    name                = "subnets"
    application         = "devops4me"
    environment         = "test"
    label_order         = ["environment", "name", "application"]
    availability_zones  = ["us-east-1a", "us-east-1b", "us-east-1c"]
    vpc_id              = "vpc-xxxxxxxxx"
    type                = "public"
    igw_id              = "ig-xxxxxxxxx"
    cidr_block          = "10.0.0.0/16"
    ipv6_cidr_block     = module.vpc.ipv6_cidr_block
  }
```

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| application | Application (e.g. `do4m` or `devops4me`). | `string` | `""` | no |
| attributes | Additional attributes (e.g. `1`). | `list` | `[]` | no |
| availability\_zones | List of Availability Zones (e.g. `['ap-southeast-1a', 'ap-southeast-1b', 'ap-southeast-1c']`). | `list(string)` | `[]` | no |
| az\_ngw\_count | Count of items in the `az_ngw_ids` map. Needs to be explicitly provided since Terraform currently can't use dynamic count on computed resources from different modules. https://github.com/hashicorp/terraform/issues/10857. | `number` | `0` | no |
| az\_ngw\_ids | Only for private subnets. Map of AZ names to NAT Gateway IDs that are used as default routes when creating private subnets. | `map(string)` | `{}` | no |
| cidr\_block | Base CIDR block which is divided into subnet CIDR blocks (e.g. `10.0.0.0/16`). | `string` | n/a | yes |
| delimiter | Delimiter to be used between `organization`, `environment`, `name` and `attributes`. | `string` | `"-"` | no |
| enable\_acl | Set to false to prevent the module from creating any resources. | `bool` | `true` | no |
| enable\_flow\_log | Enable subnet\_flow\_log logs. | `bool` | `false` | no |
| enabled | Set to false to prevent the module from creating any resources. | `bool` | `true` | no |
| environment | Environment (e.g. `prod`, `dev`, `staging`). | `string` | `""` | no |
| igw\_id | Internet Gateway ID that is used as a default route when creating public subnets (e.g. `igw-9c26a123`). | `string` | `""` | no |
| ipv6\_cidr\_block | Base CIDR block which is divided into subnet CIDR blocks (e.g. `10.0.0.0/16`). | `string` | n/a | yes |
| label\_order | Label order, e.g. `name`,`application`. | `list` | `[]` | no |
| managedby | ManagedBy, eg 'DevOps4Me' or 'NajibRadzuan'. | `string` | `"najibradzuan@devops4me.com"` | no |
| map\_public\_ip\_on\_launch | Specify true to indicate that instances launched into the subnet should be assigned a public IP address. | `bool` | `true` | no |
| max\_subnets | Maximum number of subnets that can be created. The variable is used for CIDR blocks calculation. | `number` | `6` | no |
| name | Name  (e.g. `app` or `cluster`). | `string` | `""` | no |
| nat\_gateway\_enabled | Flag to enable/disable NAT Gateways creation in public subnets. | `bool` | `false` | no |
| private\_network\_acl\_id | Network ACL ID that is added to the private subnets. If empty, a new ACL will be created. | `string` | `""` | no |
| public\_network\_acl\_id | Network ACL ID that is added to the public subnets. If empty, a new ACL will be created. | `string` | `""` | no |
| public\_subnet\_ids | A list of public subnet ids. | `list(string)` | `[]` | no |
| s3\_bucket\_arn | S3 ARN for vpc logs. | `string` | `""` | no |
| tags | Additional tags (e.g. map(`BusinessUnit`,`XYZ`). | `map` | `{}` | no |
| traffic\_type | Type of traffic to capture. Valid values: ACCEPT,REJECT, ALL. | `string` | `"ALL"` | no |
| type | Type of subnets to create (`private` or `public`). | `string` | `""` | no |
| vpc\_id | VPC ID. | `string` | n/a | yes |

## Outputs

| Name | Description |
|------|-------------|
| private\_route\_tables\_id | The ID of the routing table. |
| private\_subnet\_cidrs | CIDR blocks of the created private subnets. |
| private\_subnet\_cidrs\_ipv6 | CIDR blocks of the created private subnets. |
| private\_subnet\_id | The ID of the private subnet. |
| private\_tags | A mapping of private tags to assign to the resource. |
| public\_route\_tables\_id | The ID of the routing table. |
| public\_subnet\_cidrs | CIDR blocks of the created public subnets. |
| public\_subnet\_cidrs\_ipv6 | CIDR blocks of the created public subnets. |
| public\_subnet\_id | The ID of the subnet. |
| public\_tags | A mapping of public tags to assign to the resource. |

## Testing
In this module testing is performed with [terratest](https://github.com/gruntwork-io/terratest) and it creates a small piece of infrastructure, matches the output like ARN, ID and Tags name etc and destroy infrastructure in your AWS account. This testing is written in GO, so you need a [GO environment](https://golang.org/doc/install) in your system. 

You need to run the following command in the testing folder:
```hcl
  go test -run Test
```