# OpenQ Helm

Helm Chart for deploying the full OpenQ stack.

## Automated CI/CD with CircleCI

We deploy using [Git tags](https://git-scm.com/book/en/v2/Git-Basics-Tagging).

As can be seen in this line of code:

`- run: helm upgrade -f values-$CIRCLE_TAG.yaml -n $CIRCLE_TAG openq-$CIRCLE_TAG .`

We take the name of the tag applied on a commit, and use it to fill in the namespace and environment where Helm will operate.

The pipeline WILL NOT run on a simple commit to the main branch.

The pipeline is configured to ignore all branches and only run when a new tag matching `development.*`, `staging.*` or `production.*` is pushed.

```yaml
filters:
  branches:
    ignore: /.*/
  tags:
    only:
      - /^development.*/
      - /^staging.*/
      - /^production.*/
```

## Manual

### Initial Deploy to Environments

```shell
helm install -n development -f values-development.yaml openq-development .
helm install -n staging -f values-staging.yaml openq-staging .
helm install -n production -f values-production.yaml openq-production .
```

### Upgrade Environments (after initial install)

```shell
helm upgrade -n development -f values-development.yaml openq-development .
helm upgrade -n staging -f values-staging.yaml openq-staging .
helm upgrade -n production -f values-production.yaml openq-production .
```

## Clear Environments

```shell
helm uninstall -n development openq-development .
helm uninstall -n staging openq-staging .
helm uninstall -n production openq-production .
```