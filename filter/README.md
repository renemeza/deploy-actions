# Filters for GitHub Deployment Actions

This action includes common filters to stop workflows unless certain conditions are met.

For example, here is a workflow that publishes a package to npm when the deployemtn task is `publish`:

```workflow
workflow "Build, Test, and Publish" {
  on = "deployment"
  resolves = ["Publish"]
}

action "Build" {
  uses = "actions/npm@master"
  args = "install"
}

action "Test" {
  needs = "Build"
  uses = "actions/npm@master"
  args = "test"
}

# Filter for deployment task
action "Publish Task" {
  needs = "Test"
  uses = "renemeza/deploy-actions/filter@master"
  args = "task publish"
}

action "Publish" {
  needs = "Publish Task"
  uses = "actions/npm@master"
  args = "publish --access public"
  secrets = ["NPM_AUTH_TOKEN"]
}
```

## Available filters

### deleted

Continue if the event deletes a branch or tag.

```workflow
action "deleted-filter" {
  uses = "actions/bin/filter@master"
  args = "deleted"
}
```

### task

Continue if the deployment task matches the pattern

```workflow
action "deploy-task-filter" {
  uses = "renemeza/deploy-actions/filter@master"
  args = "task deploy"
}
```

## License

The Dockerfile and associated scripts and documentation in this project are released under the [MIT License](LICENSE).

Container images built with this project include third party materials. See [THIRD_PARTY_NOTICE.md](THIRD_PARTY_NOTICE.md) for details.
