# Mission 12 - Understanding Licensing

## Objective

Understand how Microsoft Copilot Studio licensing and Copilot Credits work when building and deploying agents.

---

# What Are Copilot Credits?

Copilot Credits are used to measure agent usage in Copilot Studio.

Credits are consumed when an agent:

* Answers questions
* Uses generative AI
* Runs tools or flows
* Uses grounding or connectors
* Performs automated actions

More complex operations consume more credits.

---

# Licensing Models

## 1. Pay-As-You-Go (PAYGO)

* Usage-based billing
* Requires Azure subscription
* Billed monthly
* Good for development and variable workloads

### Pricing

```text id="p3m8f1"
$0.01 per Copilot Credit
```

---

## 2. Capacity Pack

Monthly prepaid licensing model.

### Includes

```text id="mg8y9d"
25,000 Copilot Credits per month
```

### Pricing

```text id="q5x3eu"
$200 per pack per month
```

### Best For

* Production environments
* Predictable workloads

---

## 3. Pre-Purchase Plan (P3)

Annual prepaid model.

### Features

* Credits purchased yearly
* Lower cost at scale
* Shared across Microsoft AI workloads

---

# Copilot Studio User License

Users who create or manage agents require a:

```text id="8dx4v2"
Copilot Studio User License
```

This license is free.

---

# Microsoft 365 Copilot Licensing

M365 Copilot licensed users can use agents internally in:

* Teams
* SharePoint
* Microsoft 365 Copilot Chat

without consuming Copilot Studio credits (fair use applies).

---

# When Credits Are Consumed

Credits are consumed when:

* User is not M365 Copilot licensed
* Agent is external
* Agent runs autonomously
* Agent is published on websites or apps

---

# Credit Costs

| Feature                | Cost                         |
| ---------------------- | ---------------------------- |
| Classic answer         | 1 credit                     |
| Generative answer      | 2 credits                    |
| Agent action           | 5 credits                    |
| Tenant graph grounding | 10 credits                   |
| Agent flow actions     | 13 credits per 100 actions   |
| Premium AI tools       | 100 credits per 10 responses |

---

# Overage Enforcement

If usage exceeds capacity:

* Agents may stop responding
* Users may see usage limit errors
* Admins receive notifications

Recommended approach:

* Combine Capacity Packs with PAYGO

---

# Best Practices

* Use the Copilot Usage Estimator
* Disable unused tools
* Monitor usage regularly
* Configure PAYGO backup
* Estimate usage before production deployment

---

# Common Licensing Scenarios

| Scenario                            | Credit Usage |
| ----------------------------------- | ------------ |
| Internal M365 Copilot licensed user | No charge    |
| External website agent              | Uses credits |
| Autonomous agent                    | Uses credits |
| Unlicensed user                     | Uses credits |
| Internal Teams agent                | No charge    |

---

# Result

You now understand:

* What Copilot Credits are
* Licensing models in Copilot Studio
* When credits are consumed
* How M365 Copilot licensing works
* Basic cost planning and monitoring practices
