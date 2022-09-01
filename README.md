# Terraform provider for grafana mimir

This terraform provider allows you to interact with grafana mimir.

Example:

```
provider "mimir" {
  uri = "http://localhost:8080"
  org_id = "mytenant"
}
```

## Resource `mimir_rule_group_alerting`

Example:

```
resource "mimir_rule_group_alerting" "test" {
  name = "test1"
  name = "namespace1"
  rule {
    alert       = "HighRequestLatency"
    expr        = "job:request_latency_seconds:mean5m{job="myjob"} > 0.5"
    for         = "10m"
    labels      = {
      severity = "warning"
    }
    annotations = {
      summary = "High request latency"
    }
  }
}
```

## Resource `mimir_rule_group_recording`

Example:

```
resource "mimir_rule_group_recording" "record" {
  name = "test1"
  name = "namespace1"
  rule {
    expr   = "sum by (job) (http_inprogress_requests)"
    record = "job:http_inprogress_requests:sum"
  }
}
```

## Resource `mimir_alertmanager_config`

Notification integrations:

  - email
  - pagerduty
  - opsgenie (not yet supported)
  - slack (not yet supported)
  - webhook (not yet supported)
  - wechat (not yet supported)
  - email (not yet supported)
  - pushover (not yet supported)
  - victorops (not yet supported)
  - sns (not yet supported)
  - telegram (not yet supported)

Example:

```
resource "mimir_alertmanager_config" "test" {

  route {
    group_by = ["..."]
    group_wait = "30s"
    group_interval = "5m"
    repeat_interval = "1h"
    receiver = "pagerduty"
    child_route {
      group_by = ["..."]
      group_wait = "30s"
      group_interval = "5m"
      repeat_interval = "1h"
      receiver = "pagerduty"
    }
  }

  receiver {
    name = "pagerduty"
    pagerduty_configs {
      routing_key = "secret"
      severity = "info"
      details = {
        environment = "test"
        platform = "sandbox"
      }
    }
  }
}
```

## Importing existing resources
This provider supports importing existing resources into the terraform state. Import is done according to the various provider/resource configuation settings to contact the API server and obtain data.

### mimir alerting rule group

To import mimir rule group alerting
The id is build as `<namespace>/<name>`

Example:

```
terraform import 'mimir_rule_group_alerting.alert1' namespace1/alert1
mimir_rule_group_alerting.alert1: Importing from ID "namespace1/alert1"...
mimir_rule_group_alerting.alert1: Import prepared!
  Prepared mimir_rule_group_alerting for import
mimir_rule_group_alerting.alert1: Refreshing state... [id=namespace1/alert1]

Import successful!

The resources that were imported are shown above. These resources are now in
your Terraform state and will henceforth be managed by Terraform.

```

### mimir recording rule group

To import mimir rule group recording
The id is build as `<namespace>/<name>`

Example:

```
terraform import 'mimir_rule_group_recording.record1' namespace1/record1
mimir_rule_group_recording.record1: Importing from ID "namespace1/record1"...
mimir_rule_group_recording.record1: Import prepared!
  Prepared mimir_rule_group_recording for import
mimir_rule_group_recording.record1: Refreshing state... [id=namespace1/record1]

Import successful!

The resources that were imported are shown above. These resources are now in
your Terraform state and will henceforth be managed by Terraform.

```

### mimir alertmanager config

To import mimir alertmanager config
The id is build as `<org_id>`

Example:

```
terraform import 'mimir_alertmanager_config.test' test
mimir_alertmanager_config.test: Importing from ID "test"...
mimir_alertmanager_config.test: Import prepared!
  Prepared mimir_alertmanager_config for import
mimir_alertmanager_config.test: Refreshing state... [id=test]

Import successful!

The resources that were imported are shown above. These resources are now in
your Terraform state and will henceforth be managed by Terraform.

```

## Contributing
Pull requests are always welcome! Please be sure the following things are taken care of with your pull request:
* `go fmt` is run before pushing
* Be sure to add a test case for new functionality (or explain why this cannot be done)

