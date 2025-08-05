# Notes for easier reference

## Setting up an existing token on a new exchange

## Reading plog messages
```
cd /mnt/wmdata/md/plogs/oe/2025/06/binance/binance-wmpy-execution/cefi-binance-2
/devdata/releases/current/utils/packet_log_reader.bin --filename 20250625.binance-wmpy-execution.binance.multistream.000.plog --text-mode --grep 'EOKBGQKCQKOQIFA' 
```

## Pricing
- dynamic weights is a ML model that deprecates local weighting
- manual maker fee and local weighting is not used (should all be set to 0)
- manual taker fee (x bps) means ignore x bps from mid on both sides of the book

## DQ
mm_pnl: pos + dq0
pnl: pos + dq0 + perp_pospnl + funding + strategies


shm/snq (shared memory) to use instead of tcp for risk
sts (for logging latency)
zmq for messaging queue

## quant stuff
### dynamic weighting in pricing
has 2 tracks which are the fast and slow track
fast track has 
1. beta
2. "perceptron"-like thing, which is basically linear regression wrapped in relu
3. and is trained online

slow track is slob which is a ML model (that is pre-trained)
- can adjust this param using slob_scale

component that reconciles fast and slow and can prevent "double-counting"

all fast and slow are using same infra but are independent. slow is pre-trained, while fast is running independently from each other.

### local weighting (which is deprecated)
- sums up to <=1, linear combination of different books with local weighting and add the remaining (1- sum(weights)) from manhattan pricing


### Jupyter
`~/.jupyter/jupyter.py`

borrowing the coin has the same profile as buying spot and shorting perp