### Notes on changes required in execution layer

Below are some rough specs of changes required in the execution layer for supporting ePBS and new desin of inclusion list (with summary as a sidecar).

A short and precise summary of the new design can be obtained [here](https://github.com/potuz/consensus-specs/pull/1/files).

### New engine APIs

Based on [this PR](https://github.com/naviechan/execution-apis/pull/1/files), the following are the new engine APIs added.

1. `engine_getInclusionListV1`: The CL calls this when it's the proposer's turn to produce the next block at let's say `slot N`. The proposer's EL will respond with an inclusion list which follows certain conditions.
- The new IL design contains 2 components
  - Summary containing paris of `(address, gasLimit)`
  - List of txs corresponding to the summary (the actual tx object)
- Below are the conditions that the EL must fulfill
  - The total txs must be no more than `MAX_TRANSACTIONS_PER_INCLUSION_LIST`
  - The total gas limit must not exceed `INCLUSION_LIST_MAX_GAS`
- The EL must also make sure that the transactions are executable in `slot N pre-state`. This is just honest proposer behaviour and not a compulsion and a dishonest proposer can send a wrong list which will be invalidated by other validators while voting.

2. `engine_newInclusionListV1`: When the proposer gossips the `Block + IL (Summary + Txs)`, other valdiators will validate before sending the block to the fork-choice. Hence, they send the block to their EL which will validate this list.
- Below are some of the checks that they will make
  - The transactions provided match the summary with it in an ordered fashion
  - The total txs must be no more than `MAX_TRANSACTIONS_PER_INCLUSION_LIST`
  - The total gas limit must not exceed `INCLUSION_LIST_MAX_GAS`
  - The `maxFeePerGas` of transactions in IL is `12.5%` more than the current slot's `maxFeePerGas`
  - The transactions are executable in `slot N pre-state`
- Based on these conditions, the EL will return a boolean indicating the CL to vote accordingly.

3. `engine_newPayloadVePBS`: When the builder of `slot N` reveals it's paylaod, it will include the summary mentioned in `slot N-1` (signed by proposer of that block) in the payload. Validators on receiving the new block on CL, will make this EL call passing the necessary data for validation.
- The EL will make sure that the payload satisfies the summary associated with it.
