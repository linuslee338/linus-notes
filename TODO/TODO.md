# TODO

### ccomperp
- setup aggregators on ccomperp
- calc margin properly

### kucoinperp
- fix kucoinperp leverage
- fix current_open_interest (currently its in # of contracts but it checks against balance usd) will need to see how other exchanges do it
- look into rate limit issues

### my tokens
- my tokens market_dq report in jupyter
- review params for my tokens

##  Setup cryptocom
it will run on cefi-cryptocom-colo-2 (the same vm as spot, make sure names don't clash)
you have an api key at ~/keys/ccomsg_trading.key, let me know if it doesn't work or you need more keys
i have sent funds from fb, any of the guys can send more if you want more to test
set up balance (main + per subacc)
- use wsbalancev3
check md/exe/dropcopy, once the devs have it ready (they will need a few weeks +)
set up premiums+risks (monitored now, get me to check the configs)
funding fetcher + funding payment fetcher
aggregators
EEs + UTs (once devs done with exec)

## Setup Gateioperp 
- balance
- execution
- skews (position, sawtooth, premium)
- risk
- mdq
- funding fetcher
- lev monitor (oi + lev)

## Personal stuff


