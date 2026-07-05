# Skills Assessment — apigee-opdk-setup-analytics-group-add

> **Skill domain:** Apigee analytics topology modeling — the directed object graph managed via the Management Server REST API (OPDK). Part of the broader Apigee platform-operations portfolio; see the [`bap_coe` portfolio hub →](https://github.com/carlosfrias/apigee-hybrid-workspace/blob/main/SKILLS-ASSESSMENT.md) for the cloud-native (Hybrid/K8s) counterpart and the full corpus.

---

## Why this role is notable

- **Models analytics as a topology, not "Qpid + Postgres."** Creates the axgroup, then its consumer group, then sets the `consumer-type=ax` and `region` properties — the start of a directed object graph managed through the MS REST API.
- **Region-tagged axgroups.** The `region` property ties an axgroup to a geographic datacenter — multi-region analytics topology is a first-class concern.
- **Idempotent with rescue-based local delegation.** Every MS call has a `block/rescue` that re-runs against `127.0.0.1:8080` delegated to the MS host — robust against the case where the executing node can't reach the MS over the network but the MS can call itself.

---

## Expertise demonstrated

> Ansible is the medium. The engineering evidence lives in the [project README →](README.md). What follows is the skills assessment for the business reader.

- **Apigee analytics topology modeling** — axgroup → consumer-group → properties (consumer-type, region); the directed object graph managed via the MS REST API. The expertise is the object model, not the REST calls.
- **Region-aware multi-region analytics** — region-tagged axgroups enable per-DC analytics topologies. Multi-region analytics is a first-class concern from the first role in the lifecycle.
- **Idempotent REST reconciliation** — block/rescue local-delegation fallback on every MS call. The pattern: try against the API endpoint, and if the executing node can't reach it, delegate to the MS host which can call itself.
- **Ordered lifecycle composition** — this role is the first act; `qpid-add`, `postgres-add` (pair-as-datastore), and `scopes-add` follow in dependency order. The analytics topology is built incrementally, role by role, in sequence.

---

## How this shows the expertise

This role is the first step in modeling the Apigee analytics topology as a directed object graph. The expertise is not "making REST API calls" — it is **modeling analytics as a topology** (axgroup → consumer-group → {consumers, datastores} + {scopes}), and **reconciling that topology idempotently** against the Management Server with rescue-based local delegation.

The clearest single signal: the region-tagged axgroup. `region` is not an afterthought — it is set in the same four-step bootstrap that creates the group. Multi-region analytics topology is a first-class concern from the first role in the lifecycle, not a configuration bolted on later.

---

## Related expertise

| Skill | Repository | Assessment |
|-------|-----------|-----------|
| Rolling upgrade / DR / traffic fencing | [`apigee-opdk-playbook-maintenance-opdk-upgrade`](https://github.com/carlosfrias/apigee-opdk-playbook-maintenance-opdk-upgrade) | [SKILLS-ASSESSMENT.md →](https://github.com/carlosfrias/apigee-opdk-playbook-maintenance-opdk-upgrade/blob/main/SKILLS-ASSESSMENT.md) ✅ |
| Postgres HA / controlled switchover | [`apigee-opdk-setup-postgres-failover`](https://github.com/carlosfrias/apigee-opk-setup-postgres-failover) | [SKILLS-ASSESSMENT.md →](https://github.com/carlosfrias/apigee-opdk-setup-postgres-failover/blob/main/SKILLS-ASSESSMENT.md) ✅ |
| Cloud-native (Hybrid/K8s) counterpart | [`apigee-hybrid-workspace`](https://github.com/carlosfrias/apigee-hybrid-workspace) | [SKILLS-ASSESSMENT.md →](https://github.com/carlosfrias/apigee-hybrid-workspace/blob/main/SKILLS-ASSESSMENT.md) ✅ portfolio hub |

---

## Provenance

Authored and maintained by **Carlos Frias** during his tenure on Apigee Edge Private Cloud. This skills assessment is the companion to the engineering [README →](README.md). For the full engineering detail — task steps, variables, and composition — see the project README.

## License

See [LICENSE](./LICENSE).