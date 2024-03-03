# Registry Configuration

## Docker
> To use default [Kind](https://kind.sigs.k8s.io/) images you can set the follwing configuration: 

```bash
cat << EOF > ~/.qbo/api.json
{
"registry_user":"kindest",
"registry_auth":"hub.docker.com",
"registry_token":"",
"registry_repo":"",
"registry_hostname":"hub.docker.com",
"registry_type":"docker"
}
EOF
```

## Gitlab

> To use [custom Kind images](custom_images.md) with `Gitlab` registries see configuration below:

```bash
cat << EOF > ~/.qbo/api.json
{
"registry_user":"alex",
"registry_auth":"git.gitlab.com",
"registry_token":"4Xo5X241L5vmsFSpkzXX",
"registry_repo":"qbo-ce",
"registry_hostname":"registry.gitlab.com",
"registry_type":"gitlab"
}
E0F

```