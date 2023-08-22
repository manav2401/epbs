### Minimal ePBS design

Below mentioned is my personal understanding of ePBS from the specs. It'll be pretty much high level and rough as of now. Lot of statements might be simplified version of what's already mentioned in the spec. 

### Overview

The generic idea behind enshrining proposer builder separation is to reduce the dependency on centralised builders in the current out-of-protocol PBS mechanism such as Flashbots. Moreover, this also helps validators with high stake to become builders and have the whole interaction between a proposer and builder happen in the protocol itself which will keep them accountable for dishonest behaviour.

### Slot lifecycle

- The slot will now have additional phases (which might lead to increase in slot time from the current 12s value). The builders who are sophisticated entites (able to capture MEV) will submit bids for their `ExecutionPayload` to get included in the next block in a seperate gossip p2p channel. 
- The proposer who is selected by the beacon chain will be able to listen to these bids. Ideally it will choose the bid with the highest value and include the signature of builder from the bid in the beacon block. Proposer will propose block at time `t=t0`. 
- Note that the `ExecutionPayload` which has the txs is still with the builder. The proposer will only include the signature of the builder and also include the `ExecutionPayloadHeader` for `slot N`. 
- The attestation committee who will see the beacon block will perform the voting on the `BlindedBeaconBlock` (i.e. BeaconBlock without payload) at time `t=t1`. 
- The original builder whose bid is included in the block will monitor the attestations. It won't reveal the block if there are less votes as it leads to next-slot reorging. If enough votes are received (Q: How much is enough?), builder reveals and propagates it's block i.e. `ExecutionPayload` to the whole network in a separate gossip p2p channel at time `t=t2`.
- Based on the [PTC](https://ethresear.ch/t/payload-timeliness-committee-ptc-an-epbs-design/16054) design, there will be a separate committee known as the payload timliness committee which will vote on the revealed builder payload at `t=t3`. Others will validate against their own EL state by executing txs.

### Inclusion list

- As the builders have full authority to build blocks, if there are less number of builders, it might lead to them censoring some transactions. In order to get rid of censorship, there will be a seperate list known as Inclusion list. As the name suggests, it includes the list of transactions which the builder will have to compulsary include in the next block. The general design and flow of IL is mentioned below. 
- Proposer of `slot N` along with submitting `BlindedBeaconBlock`, will also submit a list of transactions which should be executable on `slot N pre-state` as we don't know the state of `slot N` yet. 
- Others vote on these txs to confirm. Once the builder reveals the payload, the list is filtered and only those transactions which are executable in `slot N post-state OR slot N+1 pre-state` will be kept. 
- Builder of `slot N+1` will have to make sure that it's block initially contains these transactions from IL and then has it's own transactions.
- Unfortunately, this leads to the free DA problem where non-executable transactions are required to be kept in the beacon state for validating the block giving them free storage without any actual cost. Another proposal to solve this is already out from Vitalik, Mike and others which can be found [here](https://ethresear.ch/t/no-free-lunch-a-new-inclusion-list-design/16389).
