# Terraform Tasks

## Task 1

### Command 1

```bash
terraform init
```

### Output

```bash
Terraform has been successfully initialized!
```

### Command 2

```bash
terraform validate
```

### Output

```bash
Success! The configuration is valid.
```

---

## Task 2

### Command

```bash
terraform plan
```

### Output

```bash
Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # aws_s3_bucket.exercise will be created
  + resource "aws_s3_bucket" "exercise" {
      + acceleration_status         = (known after apply)
      + acl                         = (known after apply)
      + arn                         = (known after apply)
      + bucket                      = "oyd-exercise-bucket-2026"
      + bucket_domain_name          = (known after apply)
      + bucket_prefix               = (known after apply)
      + bucket_regional_domain_name = (known after apply)
      + force_destroy               = false
      + hosted_zone_id              = (known after apply)
      + id                          = (known after apply)
      + object_lock_enabled         = (known after apply)
      + policy                      = (known after apply)
      + region                      = (known after apply)
      + request_payer               = (known after apply)
      + tags                        = {
          + "Environment" = "dev"
          + "ManagedBy"   = "terraform"
        }
      + tags_all                    = {
          + "Environment" = "dev"
          + "ManagedBy"   = "terraform"
        }
      + website_domain              = (known after apply)
      + website_endpoint            = (known after apply)

      + cors_rule (known after apply)
      + grant (known after apply)
      + lifecycle_rule (known after apply)
      + logging (known after apply)
      + object_lock_configuration (known after apply)
      + replication_configuration (known after apply)
      + server_side_encryption_configuration (known after apply)
      + versioning (known after apply)
      + website (known after apply)
    }

Plan: 1 to add, 0 to change, 0 to destroy.
```

### Questions

1. How many resources will be created, changed, or destroyed?
   1 to be created, 0 to be changed, 0 to be destroyed.

2. What does the `+` symbol next to each attribute mean?
   It indicates resources or attributes that will be created.

3. Find one attribute in the plan marked as `(known after apply)`. Why can't Terraform know that value before apply?
   Example: `arn = (known after apply)`
   Terraform cannot know this value beforehand because it is assigned by the provider (e.g., AWS) only after the resource is created via the API.

---

## Task 3

### Command

```bash
terraform plan
```

### Output

```bash
aws_s3_bucket.exercise: Refreshing state... [id=oyd-exercise-bucket-2026]

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  ~ update in-place

Terraform will perform the following actions:

  # aws_s3_bucket.exercise will be updated in-place
  ~ resource "aws_s3_bucket" "exercise" {
        id                          = "oyd-exercise-bucket-2026"
      ~ tags                        = {
            "Environment" = "dev"
            "ManagedBy"   = "terraform"
          + "Owner"       = "Sergio Oliva"
        }
      ~ tags_all                    = {
          + "Owner"       = "Sergio Oliva"
            # (2 unchanged elements hidden)
        }
        # (12 unchanged attributes hidden)

        # (3 unchanged blocks hidden)
    }

Plan: 0 to add, 1 to change, 0 to destroy.
```

### Questions

1. Did Terraform propose to destroy and recreate the bucket, or update it in place?
   Terraform proposed to update the resource in place.

2. Why does that distinction matter?
   Because destroying and recreating a resource has a greater impact than updating it. It can lead to changes in IDs, possible data loss, and downtime, while an in-place update preserves the existing resource.

---

## Task 4

### Questions

1. What does the absence (or presence) of the state file tell you about the relationship between plan and state?
   The state file does not exist if `terraform apply` has not been executed. Its absence indicates that Terraform has no record of existing infrastructure, so it assumes everything needs to be created.
