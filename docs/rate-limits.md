# Let's Encrypt production rate limits

Use the **staging** directory while developing:
`https://acme-staging-v02.api.letsencrypt.org/directory`
(staging certificates are not trusted by browsers, so a warning there is expected).

| Limit | Value |
|---|---|
| Failed validations | 5 per hostname per account per hour |
| Same set of names | 5 certificates per week |
| Per registered domain | 50 certificates per week |

Renewals of the same names skip the registered-domain limit, but still count against the other two. Check Let's Encrypt's own rate-limit page for current numbers.

Certificates default to 90 days, and Let's Encrypt has announced shorter lifetimes, so automatic renewal is not optional.

A full cluster rebuild requests a certificate per app at once, so keep the 50-per-week limit in mind.
