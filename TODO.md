# Questions


# TODO
for funding rate, funding_rate is being updated in db as 0 for all tokens. funding rates is being fetched (but didnt check if the rates are correct).  could also be an issue within write-funding-rate but have not dug deep enough yet
- setup kucoinperp exchange
1. fix funding payment
2. fix funding rate

- ADL Script
i'm trying to figure out how to investigate if we get avoid getting ADL'd on binance by transferring tokens to different subaccounts on binance, because the criteria to get ADL'd is from unrealised PnL and we can potentially get around this if transferring tokens to different sub-accounts resets the unrealised PnL.

1. get balance for some random token from api (which should have UnPnL that is not 0)
2. move token to separate desk
3. check whether unrealise pnl got reset   

## Personal stuff
- write scripts to
1. start all GUI in 1 tmux terminal (this is too mafan alr - next step is prob to do it in a tmux terminal, but need to figure out how to do that)
2. start personal monitoring script (health + relelvant tradeslist) + trading_params + dev_component_gui in separate tmux terminal
