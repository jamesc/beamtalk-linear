# linear

Linear GraphQL issue tracker client for [Beamtalk](https://beamtalk.dev).

Extracted from [beamtalk-symphony](https://github.com/jamesc/beamtalk-symphony):
the generic `query:variables:` GraphQL core plus three canned queries
(candidate fetch, state refresh, terminal fetch) and the normalized
`Issue` / `IssueRef` models. The only dependency is the `http` package
(plus stdlib `Json`).

## Installation

Add the dependency to your `beamtalk.toml`:

```toml
[dependencies]
linear = "0.1.0"
```

Then run:

```bash
beamtalk build
```

## Usage

```beamtalk
client := LinearClient
  endpoint: "https://api.linear.app/graphql"
  apiKey: "lin_api_key"
  projectSlug: "my-project-slug-id"

// Fetch all issues in active states (paginated).
issues := (client fetchCandidateIssues: #("Todo", "In Progress")) unwrap

// Refresh current states for known issue IDs.
refs := (client fetchIssueStatesByIds: #("issue-uuid-1")) unwrap

// Fetch issues in terminal states (startup cleanup).
done := (client fetchIssuesByStates: #("Done", "Cancelled")) unwrap

// Escape hatch: arbitrary GraphQL.
data := (client query: "query { viewer { id } }" variables: #()) unwrap
```

Errors surface as `LinearError` (a `TaggedError` subclass); use
`LinearClient errorKind:` to classify any `Result` failure.

## Building

```bash
just build
```

## Testing

```bash
just test
```

Live-API tests are skipped without `LINEAR_API_TOKEN` in `.env`.
