### Notes on implementation details of ePBS

This section contains running notes on the implementation details and nitty gritties of ePBS which will keep changing in coming months. The details are written with Prysm as the consensus client and geth as the execution client in mind. This document will mostly contain details of what changes will be required to support ePBS and modules will be affected. Will also try to add some sequence and logical diagrams for better understanding.

#### Overview

At a very very high level, the project can be broken down into few components which can be tackeled in parallel.

- Builder onboarding
  - Any validator which stake more than a certain threshold will be considered as a builder. Most probably, a single list will be maintained with either a field for builder or knowing if one is a builder or not using balance (at runtime).
- Bidding
  - The builders will at the beginning of `slot N` will be sending their bids on the gossip channel. Proposers need to listen to these bids, validate them and also choose the best one (for simplicity, proposer can go with the highest one).
- Sync changes
  - Modifications in the state transition will be required as the validation conditions have changed.
- Fork choice
  - Yet to think more on this - but will have to decide on different cases as mentioned in the PTC design doc.
- PTC (if we proceed to move with the PTC design for PoC)
  - New committee (mostly of length 512) needs to be maintained to vote on revealed payload. Whether the committee will be short running or long running is yet to be decided (networking).
- IL related changes
  - To support inclusion lists based on new summary based design, changes will be required both in the EL as well as CL. EL will include few engine api's which will be used to validate the IL. This includes the initial validation as well as filtering of txs post the payload is revealed.
  - CL will have to support IL in the beacon state and will also affect the beacon state transition function.
- Changes to other components
  - Withdrawals?
