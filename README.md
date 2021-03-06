<!-- This file was automatically generated by the `build-harness`. Make all changes to `README.yaml` and run `make readme` to rebuild this file. -->

[![Cloud Posse](https://cloudposse.com/logo-300x69.svg)](https://cloudposse.com)

# terraform-aws-ses-lambda-forwarder [![Build Status](https://travis-ci.org/cloudposse/terraform-aws-ses-lambda-forwarder.svg?branch=master)](https://travis-ci.org/cloudposse/terraform-aws-ses-lambda-forwarder) [![Latest Release](https://img.shields.io/github/release/cloudposse/terraform-aws-ses-lambda-forwarder.svg)](https://github.com/cloudposse/terraform-aws-ses-lambda-forwarder/releases/latest) [![Slack Community](https://slack.cloudposse.com/badge.svg)](https://slack.cloudposse.com)


This is a terraform module that creates an email forwarder using a combination of AWS SES and Lambda running the [aws-lambda-ses-forwarder](https://www.npmjs.com/package/aws-lambda-ses-forwarder) NPM module.


---

This project is part of our comprehensive ["SweetOps"](https://docs.cloudposse.com) approach towards DevOps. 


It's 100% Open Source and licensed under the [APACHE2](LICENSE).









## Introduction

This module provisions a NodeJS script as a AWS Lambda function that uses the inbound/outbound capabilities of AWS Simple Email Service (SES) to run a "serverless" email forwarding service.

Use this module instead of setting up an email server on a dedicated EC2 instance to handle email redirects. It uses AWS SES to receive email and then trigger a Lambda function to process it and forward it on to the chosen destination.  This script will allow forwarding emails from any sender to verified destination emails (e.g. opt-in).

## Limitations

The SES service only allows sending email from verified addresses or domains. As such, it's mostly suitable for transactional emails (e.g. alerts or notifications). The incoming messages are modified to allow forwarding through SES and reflect the original sender. This script adds a `Reply-To` header with the original sender's email address, but the `From` header is changed to display the SES email address.

For example, an email sent by `John Doe <jonh@example.com>` to `hello@example.com` will be transformed to:
```
From: John Doe at john@example.com <hello@example.com> 
Reply-To: john@example.com
```

To override this behavior, set a verified `fromEmail` address (e.g., `noreply@example.com`) in the config 
object and the header will look like this.
```
From: John Doe <noreply@example.com>
Reply-To: john@example.com
```

__NOTE__: SES only allows receiving email sent to addresses within verified domains. For more information, 
see: http://docs.aws.amazon.com/ses/latest/DeveloperGuide/verify-domains.html
```

Initially SES users are in a sandbox environment that has a number of limitations. See: 
http://docs.aws.amazon.com/ses/latest/DeveloperGuide/limits.html

## Usage

```hcl
variable "relay_email" {
  default     = "example@example.com"
  description = "Email that used to relay from"
}

variable "forward_emails" {
  type = "map"

  default = {
    "some_email@example.com"  = ["my_email@example.com"]
    "other_email@example.com" = ["my_email@example.com"]
  }

  description = "Emails forward map"
}

module "ses" {
  source = "git::https://github.com/cloudposse/terraform-aws-ses-lambda-forwarder.git?ref=tags/0.1.0"

  namespace = "${var.namespace}"
  name      = "${var.ses_name}"
  stage     = "${var.stage}"

  region = "${var.ses_region}"

  relay_email = "${var.relay_email}"
  domain      = "${var.parent_domain_name}"

  forward_emails = "${var.forward_emails}"
}
```






## Makefile Targets
```
Available targets:

  help                                This help screen
  help/all                            Display help for all targets
  lint                                Lint terraform code

```

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|:----:|:-----:|:-----:|
| attributes | Additional attributes (e.g. `1`) | list | `<list>` | no |
| delimiter | Delimiter to be used between `namespace`, `stage`, `name` and `attributes` | string | `-` | no |
| domain | Root domain name | string | - | yes |
| forward_emails | Emails forward map | map | `<map>` | no |
| name | Application or solution name (e.g. `app`) | string | `ses` | no |
| namespace | Namespace (e.g. `cp` or `cloudposse`) | string | - | yes |
| region | AWS Region the SES should reside in | string | `us-west-2` | no |
| relay_email | Email that used to relay from | string | - | yes |
| spf | DNS SPF record value | string | `v=spf1 include:amazonses.com -all` | no |
| stage | Stage (e.g. `prod`, `dev`, `staging`) | string | - | yes |
| tags | Additional tags (e.g. map(`BusinessUnit`,`XYZ`) | map | `<map>` | no |





## References

For additional context, refer to some of these links. 

- [aws-lambda-ses-forwarder](https://www.npmjs.com/package/aws-lambda-ses-forwarder) - A Node.js script for AWS Lambda that uses the inbound/outbound capabilities of AWS Simple Email Service (SES) to run a "serverless" email forwarding service.


## Help

**Got a question?**

File a GitHub [issue](https://github.com/cloudposse/terraform-aws-ses-lambda-forwarder/issues), send us an [email][email] or join our [Slack Community][slack].

## Commercial Support

Work directly with our team of DevOps experts via email, slack, and video conferencing. 

We provide [*commercial support*][commercial_support] for all of our [Open Source][github] projects. As a *Dedicated Support* customer, you have access to our team of subject matter experts at a fraction of the cost of a full-time engineer. 

[![E-Mail](https://img.shields.io/badge/email-hello@cloudposse.com-blue.svg)](mailto:hello@cloudposse.com)

- **Questions.** We'll use a Shared Slack channel between your team and ours.
- **Troubleshooting.** We'll help you triage why things aren't working.
- **Code Reviews.** We'll review your Pull Requests and provide constructive feedback.
- **Bug Fixes.** We'll rapidly work to fix any bugs in our projects.
- **Build New Terraform Modules.** We'll develop original modules to provision infrastructure.
- **Cloud Architecture.** We'll assist with your cloud strategy and design.
- **Implementation.** We'll provide hands-on support to implement our reference architectures. 


## Community Forum

Get access to our [Open Source Community Forum][slack] on Slack. It's **FREE** to join for everyone! Our "SweetOps" community is where you get to talk with others who share a similar vision for how to rollout and manage infrastructure. This is the best place to talk shop, ask questions, solicit feedback, and work together as a community to build *sweet* infrastructure.

## Contributing

### Bug Reports & Feature Requests

Please use the [issue tracker](https://github.com/cloudposse/terraform-aws-ses-lambda-forwarder/issues) to report any bugs or file feature requests.

### Developing

If you are interested in being a contributor and want to get involved in developing this project or [help out](https://github.com/orgs/cloudposse/projects/3) with our other projects, we would love to hear from you! Shoot us an [email](mailto:hello@cloudposse.com).

In general, PRs are welcome. We follow the typical "fork-and-pull" Git workflow.

 1. **Fork** the repo on GitHub
 2. **Clone** the project to your own machine
 3. **Commit** changes to your own branch
 4. **Push** your work back up to your fork
 5. Submit a **Pull Request** so that we can review your changes

**NOTE:** Be sure to merge the latest changes from "upstream" before making a pull request!


## Copyright

Copyright © 2017-2018 [Cloud Posse, LLC](https://cloudposse.com)



## License 

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0) 

See [LICENSE](LICENSE) for full details.

    Licensed to the Apache Software Foundation (ASF) under one
    or more contributor license agreements.  See the NOTICE file
    distributed with this work for additional information
    regarding copyright ownership.  The ASF licenses this file
    to you under the Apache License, Version 2.0 (the
    "License"); you may not use this file except in compliance
    with the License.  You may obtain a copy of the License at

      https://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing,
    software distributed under the License is distributed on an
    "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
    KIND, either express or implied.  See the License for the
    specific language governing permissions and limitations
    under the License.









## Trademarks

All other trademarks referenced herein are the property of their respective owners.

## About

This project is maintained and funded by [Cloud Posse, LLC][website]. Like it? Please let us know at <hello@cloudposse.com>

[![Cloud Posse](https://cloudposse.com/logo-300x69.svg)](https://cloudposse.com)

We're a [DevOps Professional Services][hire] company based in Los Angeles, CA. We love [Open Source Software](https://github.com/cloudposse/)!

We offer paid support on all of our projects.  

Check out [our other projects][github], [apply for a job][jobs], or [hire us][hire] to help with your cloud strategy and implementation.

  [docs]: https://docs.cloudposse.com/
  [website]: https://cloudposse.com/
  [github]: https://github.com/cloudposse/
  [commercial_support]: https://github.com/orgs/cloudposse/projects
  [jobs]: https://cloudposse.com/jobs/
  [hire]: https://cloudposse.com/contact/
  [slack]: https://slack.cloudposse.com/
  [linkedin]: https://www.linkedin.com/company/cloudposse
  [twitter]: https://twitter.com/cloudposse/
  [email]: mailto:hello@cloudposse.com


### Contributors

|  [![Igor Rodionov][goruha_avatar]][goruha_homepage]<br/>[Igor Rodionov][goruha_homepage] |
|---|

  [goruha_homepage]: https://github.com/goruha
  [goruha_avatar]: https://github.com/goruha.png?size=150


