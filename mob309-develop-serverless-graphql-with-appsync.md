# MOB309: Develop serverless GraphQL architectures using AWS AppSync

## Notes

### How BMW does it

Harmonized data model mapped from Vehicle Signal Specification

AWS AppSync subscriptions vs. SNS topics

### GraphQL

Define a strong interface for data. Similar to Swagger model.

Queries, mutations, & subscriptions

### AppSync

AppSync integrationes with Velocity

ElasticSearch, Lambda, HTTP endpoints

#### Authentication

API key, IAM, Cognito User Pools, or other OIDC provider

#### Use cases

- Many Lambdas to one API

  - use your own language
  - Can distribute ownership of functions composing single apu
  - Lambdas from different accounts
    - Uses IAM assume role API

- Why put lambdas in mult. accounts

  - Lambda parallel execution limits
  - Security

- Create organization-wide APIs

  - single, strongly typed API
  - Interface/ontract between front-end and backend teams. Better agility
    - Mock & replace implmentations
    - Build Graph API to match domain model instead of needs of application

- GraphiQL (similar to postman for restful APIs)
  - Documentation supported

* Subscriptions
  - - App Sync is a websocket fleet. Manages connections for you
  - Subscription events triggered by mutations
    - backend changes more complicated
* Caching

  - caching backend data sources
  - no extra infrastructure
  - TTL (& other settings) for diff. endpoints in API
    - Max TTL is one hour
  - Can flush the cache manually
    - Entire or by particular keys

- Ease of working with VTL
  - Amplify local runtime
    - Mocking/testing framework for Velocity templates\

AppSync local

- mobile-first approach
- conflict reolution & sync state issues.

#### Best practices

- One lambda to house all resolver logic

\*
