# Pipelines glossary

The following terms describe key concepts to help you use Pipelines.

## Agent

An agent is a small, reliable, and cross-platform build runner that connects your infrastructure to Buildkite. It polls Buildkite for work, runs jobs, and reports results. You can install agents on local machines, cloud servers, or other remote machines. You need at least one agent to run builds.

To learn more, see the [Agent overview](/docs/agent/v3).

## Artifact

An artifact is a file generated during a build. You can keep artifacts in a Buildkite-managed storage service or a third-party cloud storage service like Amazon S3, Google Cloud Storage, or Artifactory. Common uses include storing assets like logs and reports, or passing files between steps.

To learn more, see [Build artifacts](/docs/pipelines/artifacts).

## Build

A build is a single run of a pipeline. You can trigger a build in various ways, including through the dashboard, API, as the result of a webhook, on a schedule, or even from another pipeline using a trigger step.

## Cluster

A cluster groups queues of agents along with pipelines. They allow teams to self-manage their agent pools, let admins create isolated sets of agents and pipelines within the one Buildkite organization, and help to make agents and queues more discoverable across your organization.

## Dynamic pipeline

Dynamic pipelines define their steps at runtime using scripts, giving you the flexibility to only run the steps relevant to particular code changes and workflows.

Dynamic pipelines are helpful when you have a complex build process that requires different steps to execute based on runtime conditions, such as the branch, the environment, or the results of previous steps.

To learn more, see [Dynamic pipelines](/docs/pipelines/defining-steps#dynamic-pipelines).

## Hook

A hook is a method of customizing the behavior of Buildkite through lifecycle events. They let you run scripts at different points of the agent or job lifecycle. Using hooks, you can extend the functionality of Buildkite and automate tasks specific to your workflow and requirements.

To learn more, see [Hooks](/docs/agent/v3/hooks).

## Job

A job is the execution of a command step during a build. Jobs run the commands, scripts, or plugins defined in the step.

## Pipeline

A pipeline is a container for modeling and defining workflows. They contain a series of steps to achieve goals like building, testing, and deploying software.

To learn more, see the [Pipeline overview](/docs/pipelines).

## Plugin

Plugins are small, self-contained pieces of extra functionality that help you customize Buildkite to your specific workflow. They modify command steps using hooks to perform actions like checking code quality, deploying to cloud services, or sending notifications.

Plugins can be open source and available for anyone to use or private for just your organization.

To learn more, see [Plugins](/docs/plugins).

## Queue

Queues contain jobs waiting to run on an agent. You can configure queues for particular groups of agents, isolating a set of jobs and agents. Typical uses for queues include separating deployment agents and pools of agents for specific pipelines or teams.

To learn more, see [Queues](/docs/agent/v3/queues).

## Step

A step describes a single, self-contained task as part of a pipeline. You define a step in the pipeline configuration using one of the following types:

- **Command step:** Runs one or more shell commands on one or more agents.
- **Wait step:** Pauses a build until all previous jobs have completed.
- **Block step:** Pauses a build until unblocked.
- **Input step:** Collects information from a user.
- **Trigger step:** Creates a build on another pipeline.
- **Group step:** Displays a group of sub-steps as one parent step.

To learn more, see [Defining steps](/docs/pipelines/defining-steps).
