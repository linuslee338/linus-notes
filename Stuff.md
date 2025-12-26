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
- mm_pnl: pos + dq0
- pnl: pos + dq0 + perp_pospnl + funding + strategies


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

borrowing the coin has the same "profile" as buying spot and shorting perp

## setting up existing coin on a new exchange
1. update `mdfeed_config_settings_prod.py` to read that particular exchanges' market data (feed + xl8r)
2. update `prod-pricing-servers.yaml` and `pricing_config_shared.py` to update the manhattan model if we are using it to price our theo
3. run configurator for both market data and pricing and then **check and review changes carefully** before pushing to master. **COPY ALL CHANGED COMPONENTS TO RESTART**
4. Restart components that was copied in previous step. **BINANCE SPOT DONT RESTART MDFEED**
5. check `wallet_addresses.json` and update if necessary (differs for different exchange)
6. Setup trading params (for UT + EE) copy and check that they make sense
7. Setup Trading params GUI and use `-1` to price base and `1` to price quote
8. restart tokens and then monitor trading for a bit


## Wallet Addresses whitelisting
- wallet_addresses list the whitelisted addresses on different venues (exchanges/fb)

## Booking TWAP
### When starting a TWAP for client
1. Book in TWAP GUI using ctrl+D
2. forward message in twap channel to `traders-only`

### When TWAP is completed
1. Send twap completed message to tg chat
2. write done in twap gui to send confirmation email


## Chasing transfers
look at stale approved, stale in-progress/in-progress-inv and failed transfers
check wd/deposits if necessarya and chase exchanges


## Initial Margin and Maintenance/Liquidation Margin


## What is perp_pos_pnl


global_hedger - (does hedges for majors)


## FIsherman (usually for rfq retail where we think the flow is not adverse)
want to be top of book compared to other mms
so widen everytime we get trades (and correspondingly tighten if we don't get trades in a while)
the params can be seen in `trading params gui`

## MAB vs bid_max_skew/ask_min_skew
bid_max caps the skew, then applies credit to it
MAB caps the (skew + credit)




# Shift
bitfinex non-crypto e.g. fiats are j stuck there
set ee_min_bps for gbp/eur
bcb (blinc system) - kraken, bitstamp, bitvavo
bcb_wire



Shift
- quite a bit of otc flow today
- prospect has been trading in many small clips today and is net burn but we are giving good price as they are good potential cp acc to bd (should try to bundle the trades and settle together)
- struggling to hedge VINU (turned submerine on and skewed 50bps), getting more revolut flow than we can hedge on bybit and market up 10% today
- fiat slightly more back in line but still trading above. waiting for eur/gbp on natwest
- funding good for a bunch of alts incl. pump/ena/fartcoin so can keep an eye
- lowered ENA perp balance from -10m to -15m cos funding is good




- cro trading was poor in the morning, added okex ees, removed all ees extra credit, added extra tob ut lvl with skew to help hedge faster. qing helped to add okexperp and okex into pricing which helped to stop the bleeding.
- trying to wd pyth from lmax to get more inv as funding was rly -ve, pending their wl. refer to lmax slack channel
- minted another 10m usd1, need to manually adjust theo + zp
- sol pospnl horrendous tdy, think we are too tight on defi for infinite size. cant hedge out fast enough. adi widened out a little on defi, but can monitor whether the deltas are ok for the credit we are charging
- xtz pos from yest hedged out, but rh hitting us with more clips. qing helped to change fisherman credit multiplier decay but can watch if we're charging enough
- stg -> zro conversion open, not sure until when. sold alm all our stg inventory by doing the arb selling stg and buying zro. 


# Order transition
class WMPYEXEOrderEventType(enum.Enum):
    NONE = 0

    NEW = 4

    NEW_ACK = 5

    NEW_REJECT = 134

    CANCEL = 8

    CANCEL_ACK = 9

    CANCEL_REJECT = 138

    FILL = 176

    PARTIAL_FILL = 32
