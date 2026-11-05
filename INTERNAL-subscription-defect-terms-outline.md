# Defect-correction terms for the OMG subscription agreement (outline)

Not for the public repository. This is the checklist of what the commercial
subscription agreement must say, since the license deliberately says nothing.
Have counsel draft the final wording.

## 1. Limited warranty (deliberately narrow)

Obtain warrants only that, during the subscription term, OMG will perform
substantially in accordance with the then-current documentation. No warranty
of uninterrupted or error-free operation, of fitness for the customer's
particular integration scenarios, or that every defect will be corrected.

## 2. Sole remedy

For breach of that warranty, the customer's sole and exclusive remedy is, at
Obtain's option: (a) correction of the defect, (b) a workaround achieving
substantially equivalent functionality, or (c) termination of the affected
subscription and a pro-rata refund of prepaid fees. Naming a sole remedy is
what stops a defect claim from becoming an open-ended damages claim.

## 3. Severity levels and response times

Commit to **response** times, not resolution times. Resolution commitments on
software you do not control the runtime of (a customer's D365 tenant) are a
trap.

| Severity | Definition | Target response |
| --- | --- | --- |
| S1 | Production message flow stopped, no workaround | 4 business hours |
| S2 | Major function impaired, workaround exists | 1 business day |
| S3 | Minor defect, limited business impact | 5 business days |
| S4 | Cosmetic, documentation, enhancement request | Next release cycle |

State business hours and time zone (CET/CEST, Danish business days). If you
want to promise more, promise a *status update cadence* per severity rather
than a fix date.

## 4. Supported versions

Fixes are delivered only in the current release. The customer must be on a
release published within the last N months, and on a Microsoft-supported D365
platform version, to be eligible for defect correction. Without this, you owe
fixes on branches you no longer build.

## 5. Exclusions

No obligation to correct defects caused by: customer or partner modifications
to the OMG model; extensions built on OMG; a Microsoft platform change or
regression; environment or configuration problems; use outside the
documentation; or refusal to install a release containing an available fix.

## 6. Limitation of liability

Cap total liability at the fees paid in the preceding 12 months, and exclude
indirect, incidental and consequential damages, lost profits, and loss or
corruption of data. This is the single most important clause for a message
gateway: the plausible failure mode is lost, duplicated or misrouted business
documents, and the customer's claimed loss is unbounded relative to your
monthly fee.

Carve out what Danish and EU law does not allow to be excluded — at minimum
intent and gross negligence, and personal injury. An unlimited exclusion is
more likely to be struck down as a whole than a properly carved-out one.

## 7. Customer obligations

Defect correction is conditional on the customer providing reproduction steps,
relevant telemetry and logs, and a sandbox environment where the defect can be
reproduced, and on installing corrections within a reasonable period.

## 8. End of life

Reserve the right to discontinue OMG on notice (e.g. 12 months), with a
pro-rata refund. Note that the license's Change Date means the customer keeps
an Apache-2.0 version of older releases regardless — worth saying out loud in
the sales conversation, because it is a genuine reassurance against your
company disappearing.
