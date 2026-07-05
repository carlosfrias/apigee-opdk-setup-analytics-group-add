# apigee-opdk-setup-analytics-group-add — Apigee Analytics Group (axgroup) Lifecycle

> **An Ansible role that creates an Apigee analytics (ax) group and its consumer group via the Management Server REST API** — the first step in modeling the Apigee analytics topology as a directed object graph: `axgroup → consumer-group → {consumers (qpid), datastores (postgres master/standby)} + {scopes (org, env)}`.

> [!NOTE]
> Engineering portfolio note — this project demonstrates Apigee analytics topology modeling and idempotent REST reconciliation. See the [skills assessment →](SKILLS-ASSESSMENT.md) for the expertise applied.

This is one role in the **analytics-topology lifecycle** — the Expert-tier sub-domain of the OPDK corpus. Ansible is the medium; the durable work is the **Apigee analytics object model** and the **ordered, idempotent reconciliation** of that topology against the Management Server.

<!-- BEGIN Google Required Disclaimer -->

## Not Google Product Clause

This is not an officially supported Google product.
<!-- END Google Required Disclaimer -->

---

## What the role actually does

`tasks/main.yml` performs the axgroup bootstrap against the Management Server (`/v1/analytics/groups/...`):

1. **Create the analytics group** — `POST /v1/analytics/groups/{new_ax_group}`.
2. **Create the consumer group** — `POST /v1/analytics/groups/{ax}/consumer-groups/?name={new_consumer_group}`.
3. **Set `consumer-type=ax`** — `POST .../properties?propName=consumer-type&propValue=ax`.
4. **Set `region`** — `POST .../properties?propName=region&propValue={region}`.

Each step is wrapped in `block/rescue` with a `delegate_to` fallback to the MS host.

---

## When this role is used

Composed into the analytics-provisioning runbooks when onboarding a new analytics group or adding a region. The full lifecycle (this role + `apigee-opdk-setup-qpid-add` + `apigee-opdk-setup-postgres-add` + `apigee-opdk-setup-scopes-add`/`-state`) creates the complete analytics topology. See the [`apigee-edge-opdk`](https://github.com/carlosfrias/apigee-edge-opdk) framework for composition playbooks.

## Role variables (selected)

| Variable | Purpose |
|----------|---------|
| `new_ax_group` | The axgroup name to create |
| `new_consumer_group` | The consumer group name to create |
| `region` | The region tag applied to the axgroup (multi-DC analytics) |
| `opdk_user_email` / `opdk_user_pass` | MS API credentials |
| `local_mgmt_ip` | The Management Server IP (with `delegate_to` fallback) |

---

## Provenance

Authored and maintained by **Carlos Frias** during his tenure on Apigee Edge Private Cloud. One of the analytics-topology roles in the `apigee-opdk-*` corpus — the analytics object-model expertise is aggregated in the [`apigee-edge-opdk`](https://github.com/carlosfrias/apigee-edge-opdk) framework.

Contributions welcome — see [CONTRIBUTING.md](./CONTRIBUTING.md).

## License

See [LICENSE](./LICENSE).