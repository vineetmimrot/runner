# ADR 0550: Use CredScan for credential scanning

**Date**: 2020-06-22

**Status**: Proposed

## Context
Currently, there are two scenario's where there are chances that credentials can leak into workflow logs.
- Action author can unintentionally introduces bug which prints non-github credentials on console.
- User is using some CLI based action and his/her script can print non-github credentials on console.
Though the current runner has the capability to detect some type of credentials but it is not exhaustive.

## Decision
Microsoft Security team (Strike) provides [CredScan library](https://strikecommunity.azurewebsites.net/articles/4114/credential-scanner-overview.html) which can detect an exhaustive list of secret types like [[1]](https://strikecommunity.azurewebsites.net/articles/7016/credential-types-detected-by-credscan-v2.html) & [[2]](https://github.com/milidoshi26/runner/blob/main/src/Misc/layoutbin/ConfigFiles/FullTextProvider.json).
We have created PoC and found that each invocation of CredScan takes around 15-20ms and for a workflow of 200 lines it takes around 5s extra. 
We can use this library into Runner to detect many types of secret and thus prevent any secret leakage.

## Consequences
By using this library into Runner, we can prevent many types of secret leaks. 
