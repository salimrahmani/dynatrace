Act as a Senior Dynatrace Observability Architect.

We use Dynatrace Managed (Service Detection v1) to monitor Spring Boot applications running on Kubernetes.

For one workload, Dynatrace detects:
- Tomcat: Web request service
- Spring controller: Web service
- Java: Background activity service

Our SDv1 service-detection rule categories have zero configured rules.

You have access to `dtmgd` and `kubectl`.

**Your mission is to investigate the current behavior and recommend the best architecture, without modifying anything.**

1. Identify the Kubernetes workload, process groups, and corresponding Dynatrace services.
2. Inspect service identification, naming, detected technologies, and relevant configuration.
3. Analyze representative PurePaths to understand why the Tomcat and Spring services are detected separately.
4. Investigate what the Java background activity service represents.
5. Determine the recommended Dynatrace SDv1 service model for this Spring Boot application.
6. If consolidation is appropriate, investigate `builtin:service-detection.full-web-service` and `detectAsWebRequestService`.
7. Evaluate the safest way to scope a potential rule exclusively to our application in `myapp-development`, considering management zones, process-group tags, and namespace metadata.

**Principles:**
- Prefer official Dynatrace recommendations and built-in behavior.
- Challenge the assumption that consolidation is necessary.
- Verify against our actual Dynatrace API schemas and existing configuration.
- Distinguish service identity, service naming, and process-group detection.
- Do not modify Dynatrace or Kubernetes resources.

**Deliverable:** A concise technical assessment with root cause, supporting evidence, recommended target architecture, proposed configuration and scope, potential risks, and an implementation/validation plan.

Do not proceed to implementation.

TWO:

Continue from the Dynatrace SDv1 audit and approved architecture recommendation.

Your mission is to implement the recommended configuration and validate the result.

1. Reconfirm the existing configuration and retrieve the actual Dynatrace schema before making changes.
2. Apply only the necessary service-detection rule, using the narrowest verified scope for our application in `myapp-development`.
3. Ensure no unrelated Kubernetes workload, management zone, or existing detection rule is affected.
4. Validate the resulting service topology, request attribution, metrics, and representative PurePaths using new monitoring data.
5. Confirm that legitimate background activity remains observable.
6. Document the applied configuration, before/after results, any limitations, and the exact rollback procedure.

**Constraints:**
- Do not change Kubernetes workloads.
- Do not modify production or other environments.
- Do not modify or delete existing shared rules.
- If the implementation differs materially from the approved design, stop and explain why instead of improvising.
- If safe scope isolation cannot be demonstrated, do not activate the rule.
- Never claim successful validation without evidence.

Prefer the smallest, safest change consistent with Dynatrace best practices.
