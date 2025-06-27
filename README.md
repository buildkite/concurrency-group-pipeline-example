# Buildkite Concurrency Gates with Parallelism Pipeline Example

[![Build status](https://badge.buildkite.com/a947f64837044296a1ea4394819872e0544a4647a3400e6634.svg?branch=main)](https://buildkite.com/buildkite/concurrency-group-pipeline-example)
[![Add to Buildkite](https://img.shields.io/badge/Add%20to%20Buildkite-14CC80)](https://buildkite.com/new)

This repository is an example of using [Buildkite](https://buildkite.com/) concurrency groups and gates to control the flow of a pipeline.
It shows how to use parallelism, step dependencies, and concurrency limits to model gated workflows.

👉 **See this example in action:** [buildkite.com/buildkite/concurrency-group-pipeline-example](https://buildkite.com/buildkite/concurrency-group-pipeline-example)

See the full [Getting Started Guide](https://buildkite.com/docs/guides/getting-started) for step-by-step instructions on how to get this running, or [Add to Buildkite](https://buildkite.com/new) to try it yourself.

[![Add to Buildkite](https://buildkite.com/button.svg)](https://buildkite.com/new)

<a href="https://buildkite.com/buildkite/concurrency-group-pipeline-example/builds/latest?branch=main">
  <img width="2400" alt="Screenshot of concurrency group example pipeline build page" src=".buildkite/screenshot.png" />
</a>

## How it works
This example models a simple gated workflow with concurrency limits and parallelism:

![Visual Description](./images/image.png)

1. The **Running concurrency gate example** step prints a quick message to start the pipeline.
2. The **Tests** step runs in parallel (up to 5 at once), controlled by a concurrency gate.
3. When tests finish, the gates for **Deploy to Staging** and **Deploy to Production** open. These run independently.
4. Once both deploy steps complete, **Integration Tests** run in parallel (up to 3 at once).
5. After integration tests finish, the **Approve Deployment** step is triggered.

> 🔄 **Parallelism** runs multiple jobs at once.
> ⛔ **Concurrency groups** limit how many jobs run at once — even across separate builds.
>
> Think of concurrency like a traffic light: it controls flow, even when you’ve got lots of lanes.

More on this in the [Buildkite docs on controlling concurrency](https://buildkite.com/docs/pipelines/configure/workflows/controlling-concurrency).

## Pipeline Steps

- **Running concurrency gate example**: Just prints a message to kick things off.
- **Tests**: Runs up to 5 jobs in parallel, using the `tests` concurrency group.
- **Deploy to Staging**: Waits for tests to finish. Uses the `deploy-staging` concurrency group.
- **Deploy to Production**: Also waits for tests. Uses the `deploy-production` concurrency group.
- **Integration Tests**: Waits for both deploy steps. Runs up to 3 jobs in parallel in the `integration-tests` group.
- **Approve Deployment**: Waits for integration tests, and runs in the `approval` concurrency group.

Each step uses `depends_on` and `concurrency_group` to manage gates and limits.

## 🧠 Advanced Usage Notes

This setup mixes `depends_on` with concurrency groups to create gates between phases.

You can:
- Gate steps across branches or fan-in/fan-out setups
- Limit job execution globally (not just per pipeline)
- Create human approval gates at the end of automated flows

Want help modeling a complex pipeline? [Reach out to support](https://buildkite.com/support) — we love this stuff.

## License

See [LICENSE.md](LICENSE.md) (MIT)