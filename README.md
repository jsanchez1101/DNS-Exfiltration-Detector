# DNS Exfiltration Detector

ML-based detection pipeline for DNS exfiltration, evaluated across
stateless baselines, stateful windowed features, and novel attack
tools generated in a controlled homelab. 
This is the classical-ML portion of a two-project cyber+ML portfolio with the other
half being agentic/LLM based: [Local LLM Network Agent](https://github.com/jsanchez1101/Homelab-netops-agent).
Both run on the same self-built homelab.

## Status

**Phase 1 complete.** Stateless Isolation Forest baseline established
on CIC-Bell-DNS-EXF-2021 (221k benign training queries, 294k attack
queries across 6 payload types). Phase 2 (windowed features + 
autoencoder comparison) in progress.

## Phase 1 Results (Stateless Baseline)

- Detection rate: 7.2% at 5% FPR
- Attack queries show clear feature shifts vs. benign (subdomain_length
  +4.2, numeric_ratio +5.2, fqdn_count +6.9), but ~90% of individual
  attack queries score within the benign distribution
- **Finding:** per-query stateless features cannot reliably detect
  modern DNS exfiltration. Signal lives in *patterns across queries*
  to the same domain, not in any single query
- This is an honest baseline, not a target — it defines what Phase 2
  needs to beat

## Infrastructure

Two-node homelab built for isolated detection research:
- **node1** (Ubuntu 26.04): ML inference, feature engineering, model 
  training
- **node2** (EVE-NG 6.2): virtualized attacker/victim topology for 
  Phase 4 attack generation
- Tailscale mesh connecting both nodes + dev machine for remote 
  access regardless of subnet
- tcpdump captures from both hosts feed real-world benign traffic 
  into the evaluation pipeline (separate from synthetic CIC-Bell 
  benign, to surface model drift)

## Roadmap

- [x] **Phase 1:** Stateless Isolation Forest baseline
- [ ] **Phase 2:** Stateful windowed features (per-domain query rate,
      rolling distributions), autoencoder comparison vs. IF
- [ ] **Phase 3:** Real-world FPR evaluation against captured benign 
      traffic from production hosts; document concrete model drift 
      sources (mDNS, DoH, telemetry bursts)
- [ ] **Phase 4:** Generalization evaluation against novel attack 
      tools (dnscat2, iodine) inside the EVE-NG topology — tests 
      whether the detector handles attack variants outside its 
      training distribution

## Stack

- Python (pandas, scikit-learn, PyTorch for autoencoder)
- Scapy / tcpdump for packet capture and parsing
- Zeek (planned) for production-shaped telemetry layer
- Docker (planned) for reproducible deployment
