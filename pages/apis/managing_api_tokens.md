# Managing API access tokens

Buildkite API access tokens are issued to individuals not organizations. You can create and edit API access tokens in your [personal settings](https://buildkite.com/user/api-access-tokens).

On the [API Access Audit](https://buildkite.com/organizations/~/api-access-audit) page, organization admins can view all tokens that have been created with access to their organization data. As well as auditing user tokens and what access they have, you can also remove a token's access to your organization data if required.

## Token scopes

When you create a token, select the organizations it grants access to, and for REST APIS the scope of the access. GraphQL tokens cannot be limited by scope.

>📘 Note for contributors to public and open-source projects
> You need to be a member of the Buildkite organization to be able to generate and use an API token for it.

REST API scopes are very granular, you can select some or all of the following:

* Read Agents `read_agents` - Permission to list and retrieve details of agents
* Modify Agents `write_agents` - Permission to create, update and delete agents
* Read Teams `read_teams` - Permission to list teams
* Read Artifacts `read_artifacts` - Permission to retrieve build artifacts
* Write Artifacts `write_artifacts` - Permission to delete build artifacts
* Read Builds `read_builds` - Permission to list and retrieve details of builds
* Modify Builds `write_builds` - Permission to create new builds
* Read Job Environment Variables `read_job_env` - Permission to retrieve job environment variables
* Read Build Logs `read_build_logs` - Permission to retrieve `build` logs
* Write Build Logs `write_build_logs` - Permission to delete build logs
* Read Organizations `read_organizations` - Permission to list and retrieve details of organizations
* Read Pipelines `read_pipelines` - Permission to list and retrieve details of pipelines
* Write Pipelines `write_pipelines` - Permission to create, update and delete pipelines
* Read User `read_user` - Permission to retrieve basic details of the user
* Read Suites `read_suites` - Permission to list and retrieve details of test suites; including runs,
  tests, executions, etc.
* Write Suites `write_suites` - Permission to create, update and delete test suites

When creating API access tokens, you can also restrict which network address are allowed to use them, using [CIDR notation](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing).

## Auditing tokens

Viewing the API Access Audit page requires admin privileges. The page can be found in the Audit section of the Organization Settings sidebar.

All tokens that currently have access to your organization's data will be listed. The table includes the scope of each token, how long ago they were created, and how long since they've been used.

Click through any token to see more detailed information about its scopes and the most recent request.

<%= image "all-tokens-view.png", width: 1820/2, height: 1344/2, alt: "Screenshot of the API Access Audit page displaying a list of all tokens" %>

The list of tokens can be filtered by username, scopes, IP address, or whether the user has admin privileges.

 <%= image "filter-graphql-view.png", width: 1792/2, height: 1202/2, alt: "Screenshot of the API Access Audit page displaying a filtered list of tokens that have the GraphQL scope" %>

## Removing an organization from a token

If you have old tokens that should no longer be used, or need to prevent a token from performing further actions, administrators can remove the token's access to organization data.

From the API access audit page, find the token whose access you want to remove. You can search for tokens using usernames, token scopes, full IP addresses, admin privileges, or the value of the token itself.

<%= image "token-view.png", width: 1788/2, height: 2288/2, alt: "Screenshot of the API access token page with the Revoke Access button at the bottom of the screen" %>

Click through the token you'd like to remove, then click the 'Remove Organization from Token' button.

Removing access from a token sends a notification email to the token's owner, who cannot re-add your organization to the token's scope.

## Limiting API access by IP address

If you'd like to limit access to your organization by IP address, you can create an allowlist of IP addresses in the [organization's API security settings](https://buildkite.com/organizations/~/security/api).

You can also manage the allowlist with the [`organizationApiIpAllowlistUpdate`](/docs/apis/graphql/schemas/mutation/organizationapiipallowlistupdate) mutation in the GraphQL API.

## Inactive API tokens revocation

> 📘 Enterprise feature
> Revoking inactive API tokens automatically is only available on an [Enterprise](https://buildkite.com/pricing) plan.

To enable the inactive API tokens revocation, navigate to your [organization's security settings](https://buildkite.com/organizations/~/security) and specify the maximum timeframe for inactive tokens to remain valid.

Inactive API tokens refer to those that have not been used within the specified duration. When an API token surpasses the configured setting, Buildkite will automatically revoke the token's access to your organization.

Upon token revocation, Buildkite will notify the owner of their change in access.

## Programmatically managing tokens

The `access-token` REST API endpoint can be used to retrieve or revoke an API access token. See the [REST API access token](/docs/apis/rest-api/access-token) page for further information.

## FAQs

### Can I view an existing token?

No, you can change the scope and description of a token, or revoke it, but you can't view the actual token after creating it

### Can I re-add my organization to a token?

No. If an organization has revoked a token, it cannot be re-added to the token. The token owner would have to create a new token with access to your organization.

### Can I delete a token?

Yes. If you need to delete a token entirely, you can use the [REST API `access-token` endpoint](/docs/apis/rest-api/access-token#revoke-the-current-token). You will need to know the full token value.

If you own the token, you can revoke your token from the [API access token page](https://buildkite.com/user/api-access-tokens) in your Personal Settings.

### What happens if I remove the access for a token that's currently in use?

The token will lose access to the organization data. Any future API requests will no longer successfully authorize.

## API access token lifecycle and security

API access tokens are issued for users within a Buildkite organization. They are stored both in the Buildkite database (linked to the user ID) and with the user for which they're issued.

Access tokens are associated with a specific user and can only be revoked by that user. Admins of a Buildkite organization can remove a user from an organization, which prevents the user from accessing any organization resources and pipelines, and prevents access using any API access token associated with that user.

Access tokens have no built-in expiration date. The best practices regarding regular credential rotation recommended by [OWASP](https://cheatsheetseries.owasp.org/cheatsheets/Cryptographic_Storage_Cheat_Sheet.html#key-lifetimes-and-rotation) suggest rotating the tokens at least once a year. In case of a security compromise or breach, it is strongly recommended that the old tokens are [invalidated](/docs/apis/managing-api-tokens#removing-an-organization-from-a-token), and new tokens are issued.
