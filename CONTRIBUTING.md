# Contributing to OMG

Thank you for considering a contribution. OMG is a commercial product with its
source published, which makes contributing a little different from a typical
open-source project — please read the [License of
contributions](#license-of-contributions) section before you send a pull
request.

## Ways to contribute

- **Report a bug.** Open an issue with the OMG model version, your Dynamics 365
  version, what you expected, what happened, and the smallest configuration
  that reproduces it. Say whether you were running an unmodified release.
- **Suggest a transport or handler.** If you have built one for your own use
  and would like it in the product, open an issue before writing the pull
  request so we can agree on the shape.
- **Improve the documentation.** Corrections to the README, the tutorial model
  or the walkthrough are welcome and are the easiest contributions to merge.
- **Report a security vulnerability.** Not here — see
  [SECURITY.md](SECURITY.md). Please do not open a public issue.

We do not have an obligation to respond to or merge any contribution; see
[SUPPORT.md](SUPPORT.md).

## Before you write code

Open an issue first for anything beyond a small fix. A pull request that
changes the message tables, the status flow, the transport base classes or the
handler interface affects every existing installation, and we would rather
discuss the design with you than decline finished work.

## Development setup

You need a Dynamics 365 Finance and Operations development (Tier-1)
environment. The licence permits this at no cost — see [LICENSING.md](LICENSING.md).

1. Clone the repository and place `Metadata/OMG` in your
   `PackagesLocalDirectory`, or point your model store at your clone.
2. Build the `OMG` model and run a database synchronize.
3. Build `OMGTutorial` as well — it is the reference for every extension point
   and must keep compiling.

## Coding standards

- Follow the Microsoft X++ coding conventions and the style of the surrounding
  code. Match the existing naming: every element in the model is prefixed
  `OMG`.
- **The build must produce no compiler errors, no compiler warnings, and no
  best-practice errors.** Pull requests that do not meet this are not merged.
- Do not use deprecated or `SysObsolete` APIs.
- All user-facing text goes in label files. Do not hard-code strings in X++.
- New public methods and classes get a short comment explaining what they are
  for — the audience includes functional consultants reading the code to
  understand behaviour.
- Prefer adding an extension point over changing an existing signature.

## Commits and pull requests

- One logical change per pull request. Keep unrelated formatting out of it.
- Write commit messages that say what changed and why.
- In the pull request description, state what you tested and in which D365
  version.
- Note explicitly whether the change alters the database schema, the status
  flow, or any published data entity or service contract — those need an
  upgrade note.

## Backward compatibility

OMG runs in customer production environments. Assume every table field, entity,
service contract, enum value and public method signature is in use somewhere.
Additive changes are strongly preferred; removals and renames need a
deprecation path and a discussion in an issue first.

## License of contributions

OMG is distributed under the Business Source License 1.1 and is also offered by
Obtain ApS under a paid commercial subscription. So that we can keep doing
both, we need clear rights in every contribution.

By submitting a pull request, issue attachment, or any other contribution to
this repository, you agree that:

1. You are the author of the contribution, or you have the right to submit it
   under these terms, and it does not include third-party code that you are
   not entitled to contribute.
2. You retain the copyright in your contribution.
3. You grant Obtain ApS a perpetual, worldwide, non-exclusive, royalty-free,
   irrevocable license — with the right to sublicense — to reproduce, modify,
   distribute, publicly perform and otherwise use your contribution and
   derivative works of it, under any license terms, including the Business
   Source License 1.1, the Apache License 2.0, and Obtain's commercial
   subscription terms.
4. You grant every recipient of the Licensed Work a perpetual, worldwide,
   non-exclusive, royalty-free, irrevocable patent license to make, use, sell,
   offer to sell, import and otherwise transfer your contribution, limited to
   the patent claims necessarily infringed by your contribution alone or in
   combination with the Licensed Work.
5. Your contribution is provided "as is", without warranty of any kind.

If you are contributing on behalf of your employer, you confirm that you are
authorised to do so and that your employer accepts these terms.

## Questions

For anything about contributing, open an issue. For commercial and licensing
questions, contact sales@obtain.dk.
