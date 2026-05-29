# DNS Exfiltration Detector

Passive DNS anomaly detection pipeline targeting an edge deployment
on a Raspberry Pi Zero W. Work in progress.

## Status

**Phase 1 of 3 complete.** Stateless Isolation Forest trained on
CIC-Bell-DNS-EXF-2021 benign traffic (221k queries), evaluated against
the full attack set (294k queries across 6 payload types).

## Phase 1 Results (Stateless Baseline)

- Detection rate: 7.2% at 5% false positive rate
- Attack queries show clear feature shifts vs. benign (subdomain_length
  +4.2, numeric +5.2, FQDN_count +6.9), but ~90% of individual attack
  queries score within the benign distribution
- Finding: per-query stateless features cannot reliably detect modern
  DNS exfiltration. The signal lives in *patterns across queries* to
  the same domain, not in any single query
- This motivates Phase 2

## Roadmap

- [x] Phase 1: Stateless Isolation Forest baseline
- [ ] Phase 2: Stateful windowed features (per-domain query rate,
      rolling feature distributions), ensemble scoring
- [ ] Phase 3: Live deployment to Pi Zero W with scapy sniffer +
      GPIO LED indicators
- [ ] Phase 4 : Evaluation against homelab-generated
      attacks using dnscat2 / iodine
