Act as a Senior Dynatrace Observability Architect specializing in Dynatrace Managed, Service Detection v1, Spring Boot, and Kubernetes.

## Context

We use Dynatrace Managed (SDv1) to monitor Spring Boot microservices with embedded Tomcat running on Kubernetes.

Our non-production Kubernetes cluster contains namespaces `myapp-development`, `myapp-int`, and `myapp-uat`, with corresponding Dynatrace management zones.

For a single Kubernetes workload, Dynatrace may detect multiple service entities:

- **Tomcat icon — Web request services:** Potentially multiple entities associated with different pod replicas of the same deployment.
- **Spring icon — Web services:** Potentially multiple entities representing individual Spring REST controllers.
- **Java icon — Background activity services:** Entities representing background execution.

These are observed service types, not necessarily the correct architectural boundaries. Do not assume there are exactly three services or that every replica/controller necessarily creates a separate entity.

Our four SDv1 service-detection rule categories currently contain zero configured custom rules.

You have access to `dtmgd` and `kubectl`.

## Objective

Audit how Dynatrace identifies, groups, and names services belonging to our Kubernetes workload. Determine the recommended architecture according to official Dynatrace best practices.

## Investigation

1. Discover the actual Kubernetes Deployment, pods, replicas, labels, namespaces, and associated Dynatrace process groups and service entities.
2. Inventory all detected Web request, Web service, and Background activity entities, including their IDs, names, detection properties, and relationships.
3. Investigate **replica-level fragmentation**: Are multiple Tomcat Web request services created for replicas of the same workload? If so, why aren't they grouped? Examine process-group detection, service identifiers, and Kubernetes workload metadata.
4. Investigate **controller-level fragmentation**: Are Spring REST controllers detected as separate Web services? Determine whether these represent meaningful service boundaries or unnecessary fragmentation.
5. Investigate Background activity services and whether their separation is legitimate.
6. Review the current SDv1 default behavior and relevant configuration, distinguishing process-group detection, service detection, and service naming.
7. Evaluate whether `builtin:service-detection.full-web-service` with `idContributors.detectAsWebRequestService = true` is appropriate for controller consolidation. Do not assume it also solves replica-level fragmentation.
8. Determine how a custom rule, if needed, can be scoped exclusively to our application in `myapp-development`, comparing management zones, process-group tags, and Kubernetes metadata.

## PurePath

Use available Dynatrace APIs to investigate service relationships and execution flows.

If `dtmgd` cannot retrieve individual PurePaths, identify the exact manual checks required in Dynatrace's UI.

Do not claim trace-level evidence without inspecting actual traces.

## Architectural principles

- Distinguish default Dynatrace behavior from officially recommended practices.
- Challenge existing service boundaries rather than accepting them automatically.
- Prefer grouping replicas of the same logical service when technically appropriate.
- Avoid unnecessary controller-level fragmentation while preserving meaningful service boundaries.
- Preserve legitimate background activity monitoring.
- Prefer built-in functionality before introducing custom rules.
- Verify all recommendations against the actual Dynatrace Managed schemas and official documentation.

## Deliverable

Provide a concise technical assessment covering:

1. **Current topology:** Kubernetes Deployment → Pods → Process groups → Dynatrace Services.
2. **Root causes:** Explain replica-level and controller-level fragmentation separately.
3. **Recommended target topology:** How many logical Dynatrace services should exist per Kubernetes workload, and why?
4. **Recommended configuration:** Default settings, process-group adjustments, detection rules, or naming rules where justified.
5. **Implementation strategy:** Exact scoping, risks, validation steps, and expected impact.

**READ-ONLY investigation. Do not modify Dynatrace or Kubernetes.**

Prioritize evidence, official best practices, and the simplest sustainable architecture.
