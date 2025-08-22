# Questions


# TODO

- Setup cryptocom
it will run on cefi-cryptocom-colo-2 (the same vm as spot, make sure names don't clash)
you have an api key at ~/keys/ccomsg_trading.key, let me know if it doesn't work or you need more keys
i have sent funds from fb, any of the guys can send more if you want more to test
set up balance (main + per subacc)
check md/exe/dropcopy, once the devs have it ready (they will need a few weeks +)
set up premiums+risks (monitored now, get me to check the configs)
funding fetcher + funding payment fetcher
aggregators
EEs + UTs (once devs done with exec)

- ADL Script
i'm trying to figure out how to investigate if we get avoid getting ADL'd on binance by transferring tokens to different subaccounts on binance, because the criteria to get ADL'd is from unrealised PnL and we can potentially get around this if transferring tokens to different sub-accounts resets the unrealised PnL.

1. get balance for some random token from api (which should have UnPnL that is not 0)
2. move token to separate desk
3. check whether unrealise pnl got reset   

## Personal stuff
- write scripts to
1. start all GUI in 1 tmux terminal (this is too mafan alr - next step is prob to do it in a tmux terminal, but need to figure out how to do that)
2. start personal monitoring script (health + relelvant tradeslist) + trading_params + dev_component_gui in separate tmux terminal
