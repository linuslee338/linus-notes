# Questions


# TODO
- write scripts for morgan + chris
    - binance (done)
    - okex
    - kraken
    - coinbase (done)
    - gateio

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
1. start all GUI in 1 tmux terminal
2. start personal monitoring script (health + relelvant tradeslist) + trading_params + dev_component_gui in separate tmux terminal
3. write 
