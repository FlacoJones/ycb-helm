# Commonbase Helm

Helm Chart for deploying the full Commonbase stack.

## Automated CI/CD with CircleCI

We deploy using [Git tags](https://git-scm.com/book/en/v2/Git-Basics-Tagging).

We take the name of the tag applied on a commit, and use it to fill in the namespace and environment where Helm will operate.

The pipeline WILL NOT run on a simple commit to the main branch.

The pipeline is configured to ignore all branches and only run when a new tag matching `development.*` or `production.*` is pushed. 

```yaml
filters:
  branches:
    ignore: /.*/
  tags:
    only:
      - /^development.*/
      - /^production.*/
```

## Manual

### Initial Deploy to Environments

```shell
helm install -n development -f values-development.yaml ycb-development .
helm install -n production -f values-production.yaml ycb-production .
```

### Upgrade Environments (after initial install)

```shell
helm upgrade -n development -f values-development.yaml ycb-development .
helm upgrade -n production -f values-production.yaml ycb-production .
```

## Clear Environments

```shell
helm uninstall -n development ycb-development .
helm uninstall -n production ycb-production .
```