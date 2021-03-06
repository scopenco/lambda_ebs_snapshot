# Terraform config for automatic EBS snapshots

This repo contains a terraform configuration that creates two lambda functions
that will take automatic EBS snapshots at regular intervals. It is based on
the code at
<https://serverlesscode.com/post/lambda-schedule-ebs-snapshot-backups/> and
<https://serverlesscode.com/post/lambda-schedule-ebs-snapshot-backups-2/>.

This is a fork of [chef/lambda_ebs_snapshot](https://github.com/chef/lambda_ebs_snapshot)

## Usage

Include this repository as a module in your existing terraform code:

```hcl
module "lambda_ebs_snapshot" {
  source = "github.com/paxful/terraform-aws-ebs-snapshot"
  # Setting these variables is optional
  # ebs_snapshot_backups_schedule = "cron(00 19 * * ? *)"
  # ebs_snapshot_janitor_schedule = "cron(05 19 * * ? *)"
  # backup_tag                    = "Backup"
  # backup_lambda_name            = "schedule_ebs_snapshot_backups"
  # retention_lambda_name         = "ebs_snapshot_janitor"
  # examples 7d, 2h, 60
  # retention                     = "7d"
}
```

### Configuring your instances to be backed up

Tag any instances you want to be backed up with `Backup = true`.

By default, old backups will be removed after 7 days, to keep them longer, set
another tag: `Retention = 14`, where 14 is the number of days you want to keep
the backups for.
