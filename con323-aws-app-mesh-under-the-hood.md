# CON 323: AWS App Mesh Under the Hood

## Takeaways

- Learn more about as-of-then name-unknown application design patternsL
  - level-driven architecture
  - Actor systems
- Understand better how App Mesh will apply

## Notes

### Step 1: Develop principal design doc

#### Questions that it should answer

- What do customers want?
  - Routing, observability, security, & transparency
- What should the service do?
- What should the UX for the service be?

### Application centric network model

- Application-centric network model
  - traffic source --> virtual node (source/destination of all traffic within the mesh)
  - service discovery --> virtual service (logical target for traffic)
  - load balancer --> virtual router (paths, ports, retry strategies, TLS termination, etc.)
- The App Mesh config is published to Envoy. Distributed.
  - That's why "virtual" is in all the nomenclature...those primitives don't actually "exist"
  - They're just instructions dictating Envoy's behavior.

### App Mesh Architecture

- Frontend service --> Transformer --> Envoy management service
  - Why 3? Keep # to minimum. Understand scale needs of application, distribute domain based on anticipated load.
  - Frontend service
    - Picking the right database
      - JournalDB
  - Consistent hashing
  - Transformer service
    - level-driven instead of event-based
      - always performs all the work. no bimodal behavior
      - less efficient
    - picking the right database:
      - dynamodb
    - Envoy management service
      - connects to upstream sources
      - Using actor system via Akka
        - Enables ~1000 Envoy connections per host
          - don't want to have more than that to minimize blast radius
- Configuration updates are scaled to the virtual nodes
  - Only the consumers of a service are updated when a service is updated.
- Envoy
  - can do secrets
  - streaming API --> Connects to EMS
    - Authenticate and authorize that Envoy can get configuration for virtual node
  - clusters --> connection pools (retry configuration, etc.)
  - periodically disconnect envoys

### Operations

- Deployment:
  - Artifacts: container images & Configuration
  - Deploy to staging environment
  - Deloy to gamma environment (very similar to production)
  - Deploy to first production stage (bake for about a day)
  - Deploy to remaining regions
  - Fully automated
- Infrastructure
  - Using cucumbuer for all testing
- Monitoring
  - Monitor your Service Level Indicators
  - Use RED (rates/inputs, errors & duration)
  - Duration: customer impact
  - Canaries testing everything end-to-end
    - Canary faiures "but service is fine" --> WRONG
    - emit metrics from log messages
    - CloudWatch logs insihgts to find crosscutting issues or issues with a particular mesh
