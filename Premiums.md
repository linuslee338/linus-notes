# Premiums
There are 2 main ways that we price perpetuals
1. fitted premium (for more liquid exchanges e.g. binance)
2. stable premium

there are 3 types of skews:
1. position (skew based on zp and current position + now also influenced by zp optimzier)
2. premium (either fitted or stable)
3. sawtooth

# Fitted Premium


# Stable Premium


idea: we fade based on trades not pos
this means that the trade path i.e. how we get the position matters, not the cur_pos


## sawtooth
we need to account for the time to the next funding.

1. we expect that at every funding event, the price of the token drops by the amt of funding. if not, 
    - let t be the time of funding
    - we can do an arb by trading at $t - \epsilon$ and $t + \epsilon$
2. we expect that the price of the token increases linearly with gradient approximately the funding at that time

as a result, we expect this sawtooth pattern if we combine the above 2 effects.  to account for this, we calculate sawtooth skew to remove this.

### Implementation
- use some cubic spline for some `mixin_time` after each expiry (i think its to smoothen out between 2 funding periods)
- `skew = -funding * fraction of current funding period`
- prev funding is snapped 5 min before funding event