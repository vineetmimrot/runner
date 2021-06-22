# ADR 0550: Use CredScan for credential scanning

**Date**: 2020-06-22

**Status**: Proposed

## Context
Currently, there are couple of scenario's where user's credentials can leak into workflow logs during an Action run.
- User is using some CLI based action and his/her script can print credentials on console.
- Action has a bug due to which it prints user's credential. This could happen due to an unintentional oversight on Author's part.
Though, the current runner has the capability to detect secrets like GITHUB_TOKEN, secrets set by the user under repo/org/environment and ::add-mask:: tag set by the user, etc but it is not exhaustive.

## Decision
Microsoft Security team (Strike) provides [CredScan library](https://strikecommunity.azurewebsites.net/articles/4114/credential-scanner-overview.html) which can detect an exhaustive list of secret types like [[1]](https://strikecommunity.azurewebsites.net/articles/7016/credential-types-detected-by-credscan-v2.html) & [[2]](https://github.com/milidoshi26/runner/blob/main/src/Misc/layoutbin/ConfigFiles/FullTextProvider.json).
We can extend the existing SecretMasker of Runner with CredScan library to detect rest of the secret types and thus minimize the secret leakage.
We have created PoC and found that each invocation of CredScan takes around 15-20ms.

## Consequences
By using this library into Runner, we can prevent many types of secret leaks.
