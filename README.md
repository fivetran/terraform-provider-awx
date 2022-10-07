# Terraform Provider AWX

_Fork from [denouche/terraform-provider-awx](https://github.com/denouche/terraform-provider-awx) for develop additional functions.

Coming soon.

## Local Development

### 1) Start a Local AWX instance in a kubernetes cluster
A fresh AWX instance is required for automated tests, so they can ensure terraform provider is working by targetting a live AWX instance.

A new instance can be re-created by invoking the `reCreate` [mage](https://magefile.org/) target defined in `tools/magefile.go`.
Once executed it will deploy an AWX instance to a Kubernetes cluster, using [kind](https://kind.sigs.k8s.io/).

```sh
cd ./tools && go run mage.go -v reCreate && cd ..
```

### 2) Build Provider
1. Ensure [GoReleaser](https://goreleaser.com/install/) is installed
2. Run build command:
```sh
goreleaser build --snapshot --rm-dist
```

### 3) Copy Provider
Copy the provider to user's `~/.terraform.d` folder.
> Important: if building the provider in an operating system other than Linux x86_64, adjust the paths below replacing `linux_amd64` with the corresponding platform code. E.g.: `darwin_amd64` for macOS.

```sh
mkdir -p ~/.terraform.d/plugins/github.com/denouche/awx/0.1/linux_amd64/terraform-provider-awx
find ./dist/terraform-provider-awx_linux_amd64/* -name 'terraform-provider-awx*' -print0 | xargs -0 -I {} mv {} ~/.terraform.d/plugins/github.com/denouche/awx/0.1/linux_amd64/terraform-provider-awx
```

### 4) Run tests and ensure they're all passing
```sh
go test ./test -count=1
```

### Optional: All in one command
For convenience, all the steps above can be achieved by a single command that combines all of them:
```sh
goreleaser build --snapshot --rm-dist \
    && mkdir -p ~/.terraform.d/plugins/github.com/denouche/awx/0.1/linux_amd64/ \
    && find ./dist/terraform-provider-awx_linux_amd64/* -name 'terraform-provider-awx*' -print0 | xargs -0 -I {} mv {} ~/.terraform.d/plugins/github.com/denouche/awx/0.1/linux_amd64/terraform-provider-awx \
    && go test ./test -count=1
```

## Update documentation

The files in `./docs` folder are generated by executing the `genDocumentation` target defined in `tools/magefile.go` file:
```sh
cd ./tools && go run mage.go -v genDocumentation && cd ..
```
