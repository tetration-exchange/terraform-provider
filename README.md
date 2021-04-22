# terraform-provider-cisco-tetration

Terraform Provider for managing Cisco Tetration resources.

## Usage

### Environment Setup


#### Git Setup

1. Make a [Gitlab personal access token](https://docs.gitlab.com/ee/user/profile/personal_access_tokens.html#creating-a-personal-access-token)
2. Create a `.netrc` file in your home directory:

```bash
# contents of ~/.netrc:

machine gitlab.com
login <your_gitlab_username>
password <your_personal_access_token>
```

#### Go Setup

Add

```
export GOPRIVATE="gitlab.com/ignw1/internal/*,bitbucket.org/ignw,github.com/ignw"
```

to your `~/.bashrc` or `~/.profile`

Source the updated environment configuration:

```
source ~/.bashrc # or ~/.profile
```

### Building and Consuming

Build the plugin

```bash
make build
```

Copy the plugin to your terraform plugin directory, e.g.

```
mkdir ~/.terraform.d/plugins/darwin_amd64
cp terraform-provider-tetration ~/.terraform.d/plugins/darwin_amd64
```

Add plugin to terraform for the current module you are working on

```bash
cd /path/to/terraform/module
terraform init -plugin-dir ~/.terraform.d/plugins/darwin_amd64
```

Write terraform code using this provider.

```hcl
provider "tetration" {
  api_key                  = "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
  api_secret               = "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
  api_url                  = "https://ignwpov.tetrationpreview.com"
  disable_tls_verification = false
}

resource "tetration_filter" "filter" {
  name            = "Terraform created filter"
  query_type      = "eq"
  query_field     = "ip"
  query_value     = "10.0.0.1"
  app_scope_id = "5ceea87b497d4f753baf85bc"
}
```

More [example terraform modules for managing tetration resources with this provider.](./examples)

## Development

### Testing

Tests can be executed via

```bash
make test
```

When the test process is running any variable set in a top level `.env` file in this project will be available to the tests as an environment variable.

Example `.env` file

```bash
VARIABLE=value
```

This file is gitignored to prevent any sensitive material such as api keys from being published.

## Publishing

To build binaries for mac, linux(amd64), windows(x86), run

```bash
make cross-compile
```

The built binaries will be placed in the [bin directory](./bin).