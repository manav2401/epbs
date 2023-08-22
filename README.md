### What the hell is this?

This repository will contain some details on ePBS i.e. enshrined proposer builder separation which I am pursing as a part of the Ethereum Protocol Fellowship.

### Resources

Here are some of the resources I am referring to while working on this project.

General resources on PBS
- [Notes on PBS by Barnabe](https://barnabe.substack.com/p/pbs)
- [PBS by Vitalik](https://ethresear.ch/t/proposer-block-builder-separation-friendly-fee-market-designs/9725/3)
- [How PBS works and it's importance - session by Vitalik with Flashbots](https://www.youtube.com/watch?v=8mcm-jT2nq4)
- [List of other sources on PBS (overlapping)](https://notes.ethereum.org/@domothy/pbs_links)
- [Workflow of a builder](https://github.com/flashbots/builder/blob/main/docs/builder/builder-diagram.png)

Censorship resistance with PBS
- [Censorship resistance under PBS](https://notes.ethereum.org/@vbuterin/pbs_censorship_resistance)
- [Notes on CR under PBS by Francesco](https://notes.ethereum.org/@fradamt/H1TsYRfJc)
- [Notes on forward inclusion list by Francesco](https://notes.ethereum.org/@fradamt/forward-inclusion-lists)
- [Free DA problem for using IL and alternate design by Mike](https://ethresear.ch/t/no-free-lunch-a-new-inclusion-list-design/16389)

ePBS
- [Why ePBS by Mike](https://ethresear.ch/t/why-enshrine-proposer-builder-separation-a-viable-path-to-epbs/15710)
- [Two slot design for ePBS by Vitalik](https://ethresear.ch/t/two-slot-proposer-builder-separation/10980)
- [PTC design for ePBS by Mike](https://ethresear.ch/t/payload-timeliness-committee-ptc-an-epbs-design/16054)
- [ePBS roadmap and designs by Mike](https://www.youtube.com/watch?v=7OxDXd9C2SY)
- [Three dichotomies in ePBS by Potuz](https://ethresear.ch/t/three-dichotomies-in-epbs/16267)

### Specs

[Potuz](https://github.com/potuz) and [Terrence](https://github.com/terencechain) from the Prysm have been really kind to provide some initial specs based on their thoughts on ePBS. 

- [p2p spec](https://github.com/terencechain/consensus-specs/pull/1)
- [Beacon chain and consensus spec](https://github.com/potuz/consensus-specs/pull/1)

I have tried to put together a summary from the specs to guage the implementation details better in the [design](./design.md) section. 
