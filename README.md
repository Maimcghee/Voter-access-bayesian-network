# Voter Access Bayesian Network

A Bayesian network that models which barrier would keep more eligible citizens from voting under a hypothetical scenario combining the proposed **SAVE Act** (proof of citizenship, shown in person, to register) and a **2026 mail-ballot executive order** (verified eligibility required to get a mail ballot):

- **Documents:** not having the required paperwork, or
- **Physical access:** not being able to show up in person.

> **Note:** Neither policy is currently in effect. This project models a hypothetical scenario using real survey and Census data as inputs.

## Key findings

| Measure | Result |
|---|---|
| P(lacks documents \| didn't vote) | **15.1%** |
| P(lacks physical access \| didn't vote) | **71.95%** |

In the model, a person who didn't vote was **about 5× more likely** to have been blocked by physical access than by missing documents.

| Measure | Today (real) | Predicted (scenario) |
|---|---|---|
| Registration rate | 73.6% | 46.8% |
| Turnout rate | 65.3% | 41.5% |

The whole predicted drop happens at **registration**. People who do register still vote at the same rate (88.7%).

**Why it matters:** public discussion of these rules tends to focus on the paperwork. This model suggests the in-person requirement may be the bigger real-world obstacle.

## How the model works

A discrete Bayesian network with four variables:

```
has_documents ─┐
               ├─► registered ─► voted
can_access_in_person ─┘
```

Probability tables are built from real data:
- **Documents:** national proof-of-citizenship survey (Brennan Center / CDCE, fall 2023, n = 2,386)
- **Physical access:** Census CPS Table 10, reasons for not voting (Nov 2024), used as a proxy
- **Real-world anchor:** *Fish v. Kobach* (Kansas, 2013–2016), where a federal court found about 14% of registration attempts were blocked under a similar law

Results come from exact inference with `pgmpy`'s `VariableElimination`.

## Repo contents

| File | What it is |
|---|---|
| `voter-access.ipynb` | Main model: builds the network and runs inference |
| `data_and_figures.ipynb` | Documents data verification and figures |
| `data_stage2.ipynb` | Physical-access data verification and cross-checks |
| `data/` | Source datasets |
| `figures/` | Generated charts |
| `RESULTS_SUMMARY.md` | Full results write-up |
| `RUN_INSTRUCTIONS.md` | Detailed setup and run steps |

## Quick start

```bash
git clone https://github.com/Maimcghee/Voter-access-bayesian-network.git
cd Voter-access-bayesian-network
python3 -m venv venv
source venv/bin/activate
pip install ipykernel pgmpy networkx matplotlib pandas numpy
```

Then open `voter-access.ipynb` and run all cells. See [`RUN_INSTRUCTIONS.md`](RUN_INSTRUCTIONS.md) for full steps and expected output.

## Limitations

- Three of the four registration probabilities are reasoned estimates; only the best-case value is grounded in the Kansas case.
- The physical-access figure is a proxy, since the Census table covers registered non-voters, not people blocked from registering.
- Name-change mismatches weren't measured, which likely undercounts the documents barrier for women.
- The mail-ballot verification risk is discussed qualitatively only; no data exists to measure it.

See `RESULTS_SUMMARY.md` for the full discussion.

## Team

Built by **Mai McGhee** and **Ty**. This project is the starting point for ongoing research with the Institute for Mathematics and Democracy.
