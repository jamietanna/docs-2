# Hosted agent caching

Buildkite provides a caching capability for Hosted Agents to speed up builds by re-using dependencies between builds in a pipeline. You can cache downloaded resources, binaries, packages, that don't change between builds and would otherwise slow down your job run.

This feature is not supported for self-hosted agents.

### Cache volumes

Hosted Agent Caching is powered by Cache Volumes, which are attached to hosted agent instances. Cache Cache volumes are attached on best-effort basis depending on their locality, expiration and current usage (so they should not be relied upon as durable data storage).

Cache volumes are scoped to a pipeline and are shared between all steps in a pipeline. Cache volumes are disabled by default and can be enabled at the pipeline level or at the step level by providing a list of paths to cache.

Cache volumes act as regular disks with the following properties:
- They're backed by local NVMe storage. You can expect high performance.
- A Cache Volume is formatted as a regular Linux filesystem (e.g., ext4), so you can expect them to support any use-case you have that Linux supports.

### Cache paths

Cache volumes are mounted into user-specified paths. Cache paths will be mounted relative to the builds working directory. Absolute references can be provided in the cache paths configuration relative to the root of the instance (for example `/etc/cache`).

```yaml
# Enable the dependency cache for the pipeline and mount the node_modules directory on all steps in the pipeline. This will use the default cache volume for the pipeline.
cache:
  paths:
    - "node_modules"

steps:
  - command: "npm install"

  - command: "bundle install"
    # Mount the vendor/bundle directory to the dependency cache.
    # This will use the default cache volume for the pipeline.
    cache:
      paths:
        - "vendor/bundle"
```

### Lifecycle

At any point in time, multiple versions of a Cache Volume may be used by different jobs.

The first request creates the first version of the Cache Volume, which is used as the parent of subsequent forks until a new parent version is committed.

When requesting a Cache Volume, a "fork" of the previous cache volume version is attached to the agent instance (all but the first one, which starts empty). Each job gets its own private copy of the Cache Volume, as it existed at the time of the last cache commit.

Each job gets its own private copy of the Cache Volume, as it existed at the time of the last cache commit.

Version commits follow a "last write" model: whenever a job terminates successfully (e.g. exits with exit code 0), Cache Volumes attached to that job have a new parent committed: the final flushed volume of the exiting instance.

Whenever a job fails, the Cache Volume versions attached to the agent instance are abandoned.

### Custom caches

Custom caches can be created by specifying a name for the cache. This allows for multiple cache volumes to be used in a single pipeline.

```yaml
# Enable the dependency cache for the pipeline and mount the node_modules directory on all steps.
cache:
  paths:
    - "node_modules"

steps:
  - command: "npm install"

  - command: "bundle install"
    # Mount the vendor/bundle directory to a dedicated bundle cache volume. One bundle cache volume will be created for the pipeline.
    cache:
      name: "bundle-cache"
      paths:
        - "vendor/bundle"
```

### Sizing

When requesting a Cache Volume you can specify a size. When requesting x GB, a volume will be provided that has at least x GB free. In the case of a cache hit (most of the time), the actual volume size is: last used volume size + x.

The default size for the dependency cache is 20g. This can be customized with the `size` option.
  
```yaml
cache:
  size: "30g"
  paths:
    - "node_modules"
steps:
  - command: "npm install"
```

### Git mirror cache

The Git mirror cache is a special type of Cache Volume that is used to speed up Git operations by caching the Git repository between builds. This is useful for large repositories that are slow to clone.

Git mirror caching can be enabled per cluster on the Cluster's Cache Volumes settings page. Once enabled, the Git mirror cache will be used for all hosted jobs in that cluster.

