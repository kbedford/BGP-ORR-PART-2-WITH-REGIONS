# BGP-ORR-PART-2-WITH-REGIONS

# BGP ORR with 4 Regional RR Clusters (cRPD + containerlab)

This lab demonstrates BGP Optimal Route Reflection (ORR) with four regional route reflector (RR) clusters. Each cluster has two clients. 

The topology uses iBGP everywhere and IS-IS as the underlay IGP. ORR is configured per region using `optimal-route-reflection` with `igp-primary` and `igp-backup`.

The key idea is to show how ORR lets an RR select a best path based on the IGP view of a client location, not the RR's own IGP perspective.

## Topology summary

- 4 regions: A, B, C, D
- 4 RRs: `rr1`–`rr4`
- 8 clients: `a1 a2 b1 b2 c1 c2 d1 d2`
- iBGP between each RR and its two clients
- iBGP full mesh between RRs
- IS-IS L2 across all nodes
- ORR enabled on each RR for its regional client group

Clients are dual-homed into the IGP (to their regional RR and the adjacent RR) to create different IGP viewpoints between the RR and the client group. This allows ORR to actually change the chosen egress for a shared prefix.

## Topology diagram

```mermaid
graph LR
  rr1((rr1)) --- rr2((rr2))
  rr2 --- rr3((rr3))
  rr3 --- rr4((rr4))
  rr4 --- rr1

  a1((a1)) --- rr1
  a2((a2)) --- rr1
  b1((b1)) --- rr2
  b2((b2)) --- rr2
  c1((c1)) --- rr3
  c2((c2)) --- rr3
  d1((d1)) --- rr4
  d2((d2)) --- rr4

  a1 --- rr4
  a2 --- rr4
  b1 --- rr1
  b2 --- rr1
  c1 --- rr2
  c2 --- rr2
  d1 --- rr3
  d2 --- rr3
