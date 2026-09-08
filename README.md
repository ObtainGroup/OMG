# Obtain Message Gateway (OMG)

[![License: BUSL-1.1](https://img.shields.io/badge/license-BUSL--1.1-blue)](LICENSE.md)
[![Free for dev and test](https://img.shields.io/badge/dev%20%26%20test-free-brightgreen)](LICENSING.md)
[![Production: subscription](https://img.shields.io/badge/production-subscription-orange)](LICENSING.md)

A message gateway for Microsoft Dynamics 365 Finance and Operations. OMG gives
you one place to send, receive, monitor and retry the files and messages that
move between D365 and the systems around it — instead of a different one-off
integration for every interface.

> **Source-available, not free for production.** You may download, run, test
> and extend OMG at no cost in development and sandbox environments. Running it
> in production requires a paid monthly subscription from Obtain ApS. See
> [Licensing](#licensing) below — please read it before you build on OMG.

---

## Why OMG

A typical D365 implementation accumulates integrations one at a time: a file
drop here, a recurring data job there, a custom service somewhere else. Each
has its own error handling, its own logging, and its own answer to "did that
order actually arrive last night?" — and usually the answer involves a
developer.

OMG replaces that with a single, configurable pipeline:

- **One place to look.** Every inbound and outbound message is a record, with
  its status, payload, size, timestamps and error text. Functional consultants
  can answer "what happened to this message?" without opening Visual Studio.
- **Configuration instead of code.** New interfaces are set up as *message
  types* on a form. Code is only needed for genuinely new behaviour.
- **Retry and reprocess.** Failed messages stay in the queue and can be
  reprocessed once the underlying problem is fixed, rather than being lost.
- **Built to be extended.** New transports and new message handlers are
  ordinary X++ classes deriving from documented base classes. A complete
  worked example ships in this repository.

## How it works

```
                 ┌─────────────┐     ┌────────────┐     ┌─────────────┐
  Azure Blob ──> │             │ ──> │  Inbound   │ ──> │   Handler   │ ──> D365
  File share ──> │  Transport  │     │  message   │     │ (e.g. DMF)  │
  Your own   ──> │             │     │   table    │     └─────────────┘
                 └─────────────┘     └────────────┘

                 ┌─────────────┐     ┌────────────┐
  Azure Blob <── │             │ <── │  Outbound  │ <── D365 event, batch job
  File share <── │  Transport  │     │  message   │     or service call
  Your own   <── │             │     │   table    │
                 └─────────────┘     └────────────┘
```

Four concepts, and that is most of the product:

| Concept | What it is |
| --- | --- |
| **Message type** | The configuration record for one interface: direction, transport, handler, parameters, retention, priority, whether it runs synchronously, and whether it logs to Application Insights. |
| **Transport** | How a message physically moves — reads from or writes to Azure Blob storage or an Azure file share today, or anything you implement. |
| **Message** | The inbound or outbound record itself: payload, status, size, timing and errors, kept for the retention period you configure. |
| **Handler** | What happens to an inbound message once it has arrived — for example a Data Management Framework import, or your own X++ class. |

Messages move through the statuses `Ready` → `Processing` → `Processed`, or
land in `Error` for investigation and reprocessing. Inbound files can be
archived, deleted or left in place after import.

## What's included

**Transports (out of the box)**

- Azure Blob storage containers, including per-subdirectory routing
- Azure file shares, including subdirectories
- Credentials resolved through Azure Key Vault rather than stored in D365

**Inbound handlers**

- Data Management Framework import, for single entities and for data packages
- `OMGIInboundMessageHandler` for anything else you need

**Operations and monitoring**

- Inbound and outbound message workspaces with cues and KPIs
- Optional per-message-type logging to Azure Application Insights — message
  counts, sizes, durations, error counts and message IDs as queryable custom
  properties
- A batch cleanup job that enforces your retention periods
- OData data entities for message headers, details and status, so an external
  system can ask D365 about the fate of a message it sent
- Service endpoints for pushing a message in or pulling status out
- `OMGAdmin` and `OMGUser` security roles

## Requirements

- Microsoft Dynamics 365 Finance and Operations, version `<10.0.46>` or later
- A development (Tier-1) or sandbox environment — a cloud-hosted dev box or a
  local VM
- An Azure storage account if you want to use the Azure transports, and an
  Azure Key Vault for their credentials
- An Application Insights resource if you want telemetry (optional)

## Getting started

1. **Clone the repository.**

   ```bash
   git clone https://github.com/ObtainGroup/OMG.git
   ```

   Copy `Metadata/OMG` into your `PackagesLocalDirectory` (commonly
   `K:\AosService\PackagesLocalDirectory`), or point your Visual Studio model
   store at this repository.

2. **Build** the `OMG` model from Visual Studio (*Dynamics 365 → Build models*)
   or with `xppc`, then run a **database synchronize**.

3. **Enable the configuration key** for OMG and assign yourself the `OMGAdmin`
   role.

4. **Create a message type.** Pick a direction, choose a transport class and —
   for inbound — a handler class, then fill in the parameters the transport
   asks for. For a first test, an inbound Azure Blob container feeding a DMF
   import of a simple entity is the shortest path to a working interface.

5. **Run it.** Fetch and processing run as batch jobs; you can also trigger a
   single message from the message form while testing.

6. **Watch it.** Open the inbound message workspace and follow the record
   through its statuses.

A step-by-step walkthrough with screenshots is in the [documentation](docs/).

## Extending OMG

OMG is designed to be extended, and the `OMGTutorial` model in this repository
is a complete, compiling example of every extension point:

| You want to | Derive from / implement | Tutorial example |
| --- | --- | --- |
| Add a transport (SFTP, an API, a queue) | `OMGInboundTransport` / `OMGOutboundTransport` | `OMGTutorialInboundTransport` |
| Give that transport its own parameters | `OMGDataContract` | `OMGTutorialInboundDataContract` |
| Validate those parameters in the UI | the `OMGAzureValidator` pattern | `OMGTutorialInboundValidator` |
| Do something custom with an inbound message | `OMGIInboundMessageHandler` | `OMGTutorialMessageProcessor` |
| Import through DMF with custom logic | `OMGMessageProcessorDMF` | `OMGTutorialMessageProcessorDMF` |
| Emit a document from D365 | the outbound message API | `OMGTutorialSalesOrderExport` |
| Trigger on a business event | a standard D365 event handler | `OMGTutorialLedgerJournalCreatePostHandler` |

The transport base classes derive from `SysOperationFrameworkService`, so your
transport gets batch execution, parameter dialogs and the standard D365
operation behaviour without extra work.

The OMG model allows customisation, so you can extend its tables and forms
directly as well — but prefer the extension points above, so that upgrades
stay painless.

## Repository layout

```
Metadata/OMG           The OMG model — the product itself
Metadata/OMGTutorial   Worked examples of every extension point
Projects/              Visual Studio projects and solutions
BuildAutomation/       Azure DevOps YAML pipeline for X++ builds
```

## Licensing

OMG is released under the [Business Source License 1.1](LICENSE.md).

**Free, no subscription needed:** reading the source, development, testing,
QA, demonstrations, training, and building extensions — in Tier-1 development
and Tier-2+ sandbox and UAT environments, *including environments holding a
copy of production data*. Forking and modifying is allowed, and so is
redistributing your fork with the licence attached.

**Requires a paid subscription:** running OMG in production, or using it to
process live business data anywhere. This applies equally to forks and to
solutions built on top of OMG. Subscriptions are billed monthly per Dynamics
365 tenant — contact sales@obtain.dk.

Each released version converts to the Apache License 2.0 four years after it
was first published.

Partners and implementation consultants may install, configure, extend and
demo OMG for customers free of charge in non-production environments; the
customer takes the subscription when they go live.

The plain-language summary is in [LICENSING.md](LICENSING.md); the licence
itself governs.

## Support, security and contributing

- **Support:** [SUPPORT.md](SUPPORT.md). Free use comes with no support
  obligation. Subscribers should use the support channel named in their
  agreement rather than GitHub issues.
- **Security:** [SECURITY.md](SECURITY.md). Report vulnerabilities privately
  to security@obtain.dk — never as a public issue.
- **Contributing:** [CONTRIBUTING.md](CONTRIBUTING.md). Pull requests are
  welcome; contributors sign a CLA so that we can ship their work in both the
  public and the commercial editions.

## Trademarks

"Obtain" and "Obtain Message Gateway" are trademarks of Obtain ApS and are not
licensed by [LICENSE.md](LICENSE.md). Please give modified distributions a
different name.

Microsoft, Dynamics 365 and Azure are trademarks of the Microsoft group of
companies. OMG is an independent product of Obtain ApS, not affiliated with or
endorsed by Microsoft. You are responsible for your own Microsoft licences.

---

Made in Denmark by [Obtain ApS](https://www.obtain.dk).
