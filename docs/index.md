# LMOS

LMOS is a Language Model Orchestration tool.

It is designed as a scalable system to deploy large language models and other ML systems. Some core components include:

## ALMoAPI

ALMoAPI is a language model runtime forked from tabbyAPI, but with some extra enhancements to support mode advanced tool calling and to better integrate with the LMOS ecosystem. This is one of the few parts licensed under AGPL.

## InferRoute

InferRoute is a reverse proxy designed from the ground up with support for the LMOS system. The closest alternatives are liteLLM and routeLLM (which bundles liteLLM). This component ties the multiple backends together into one cohesive endpoint.

## Why LMOS

LMOS has the advantage of maintaining an ecosystem of tools. We can closely integrate applications with more efficient communication protocols and take advantage of full stack optimizations, compared to separate tooling like vLLM with a much smaller scope. In order to take advantage of the work from industry giants we have runners built for various existing inference frameworks.

Having easy integration with external runners makes LMOS an incredibly competitive solution for both small scale and large scale operations. Maximize the inference capacity with large scale vLLM runners, or take advantage of the [exllamav2](https://github.com/turboderp/exllamav2) for optimal quantization. Every implementation has its strengths and LMOS allows you to leverage them for your environment, and swap them out as you scale operations.

Unlike some other smaller scale projects, we support containerization as a first class citizen and as our preferred method of deployment. This includes the whole stack, for example we use [devcontainers](https://containers.dev/) for development, and for testing we use a [testcontainers](https://github.com/testcontainers/testcontainers-python). This has the advantage of keeping the entire build and its dependencies in a known state. We can even debug running instances live to reduce the time to fix issues.

We strive to ensure all code is thoroughly tested, using both unit testing and static analysis tools like mypy, reducing the chance that bugs reach production at all. This expands to other systems, where we have submitted PRs into our dependencies to enable a deeper level of type checking.

## LMOS Design Philosophy

This document outlines the key design principles and philosophies that guide the development and operation of the LMOS (Language Model Orchestration System) project. These principles are fundamental to understanding how LMOS is structured and why certain decisions have been made.

### 1. Immutable Configurations

One of the core tenets of LMOS is the concept of immutable configurations. This principle is applied as follows:

- Once a container or pod is loaded with a configuration, that configuration remains unchanged throughout its lifecycle.
- There is no mechanism to modify the configuration of a running instance.
- To update the configuration, a rolling update process is employed:
  1. New nodes with the updated configuration are spun up.
  2. Old nodes are gradually shut down.
  3. This process continues until the entire system has refreshed to the new configuration.

This approach offers several benefits:
- Ensures system consistency and predictability
- Simplifies debugging and troubleshooting
- Supports easier rollbacks if issues are encountered
- Aligns with immutable infrastructure practices