# Breaking Changes

- On IBM Cloud Kubernetes Service you need to set `security.initVolumeAsRoot` to `true`

# What’s new in the MQ Advanced for Developers Chart, Version 3.0.x

- Updated to IBM MQ 9.1.2
- Improved security (including running as non-root)
- Additional IBM Cloud Pak content
- Added ILMT annotations
- README updates

# Fixes

- V3.0.1: Fix capabilities when running init volume as root 
- V3.0.0: Kibana dashboard fix

# Prerequisites

- None

# Documentation

- [What's new and changed in IBM MQ Version 9.1.x](https://www.ibm.com/support/knowledgecenter/en/SSFKSJ_9.1.0/com.ibm.mq.pro.doc/q113110_.htm)

# Version History

| Chart | Date | Kubernetes Required | Image(s) Supported | Breaking Changes | Details |
| ----- | ---- | ------------ | ------------------ | ---------------- | ------- |
| 3.0.1 | March 2019 | >= 1.9 | = MQ 9.1.2.0 | None | Fix capabilities when running init volume as root |
| 3.0.0 | March 2019 | >= 1.9 | = MQ 9.1.2.0 | Set initVolumeAsRoot on IKS | Updated to IBM MQ 9.1.2; Improved security (including running as non-root); Additional IBM Cloud Pak content; Added ILMT annotations; README updates; Kibana dashboard fix |
| 2.2.0 | November 2018 | >= 1.9 | = MQ 9.1.1.0 | None | Updated to IBM MQ 9.1.1 |
| 2.1.0 | September 2018 | >= 1.9 | = MQ 9.1.0.0  | None | Declaration of securityContext; Configurable service account name; New IBM Cloud Pak content |
| 2.0.2 | August 2018 | >= 1.9 | = MQ 9.1.0.0  | None | Fixed error in service selector for helm tests |
| 2.0.1 | July 2018 | >= 1.9 | = MQ 9.1.0.0  | None | Reverted statefulset to apps/v1beta2 to prevent deletion failures |
| 2.0.0 | July 2018    | >= 1.9 | = MQ 9.1.0.0  | New Kubernetes resource names and labels | Added metrics service |
| 1.3.0 | May 2018     | >= 1.6 | = MQ 9.0.5.0  | None | Added POWER and z/Linux support |
| 1.2.1 | Apr 30, 2018 | >= 1.6 | >= MQ 9.0.4.0 | None | README fixes |
| 1.2.0 | Apr 3, 2018  | >= 1.6 | >= MQ 9.0.4.0 | None | Added liveness and readiness probes; Optional JSON logging; New README format |
| 1.0.2 | Nov 6, 2017  | >= 1.6 | >= MQ 9.0.3.0 | None | Updates for MQ 9.0.4.0 |
| 1.0.1 | Oct 25, 2017 | >= 1.6 | MQ 9.0.3.0    | None | Initial version |
