# SOLANA SNIPER BOT

![](https://github.com/spaceboundbot/solana-sniper-bot/blob/26dffa5ab25cc1c80d45c829021a63d1aa4e850a/readme/sbot.png)

**Solana SPL Token Sniper Bot** that listens to new Raydium USDC or SOL pools and buys tokens for a fixed amount in USDC/SOL. 
Depending on the speed of the RPC node, the purchase usually happens before the token is available on Raydium UI for swapping.

- `WSOL Snipe`
- `Auto-Sell`
- `TP/SL`
- `Min Liq`
- `Burn/Lock Check`
- `Renounce Check`
- `Rug Score Check`
- `Fast Buy`

> [!NOTE]
> This is provided as is, for learning purposes.

[![TypeScript](https://badgen.net/badge/icon/typescript?icon=typescript&label)](https://typescriptlang.org)
![UPTime](https://camo.githubusercontent.com/4a67ad96d71cca235a4393b2f3b79aabb0a3d42d555030632f1110e9eedde567/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f757074696d652d3130302532352d627269676874677265656e)
[![GitHub stars](https://img.shields.io/github/stars/spaceboundbot/solana-sniper-bot.svg?style=social&label=Star&maxAge=1000)](https://GitHub.com/spaceboundbot/solana-sniper-bot/stargazers/)
[![GitHub forks](https://img.shields.io/github/forks/spaceboundbot/solana-sniper-bot.svg?style=social&label=Fork&maxAge=1000)](https://GitHub.com/spaceboundbot/solana-sniper-bot/network/)
![Watchers](https://img.shields.io/github/watchers/spaceboundbot/solana-sniper-bot.svg)

## 👾 SETUP
To run the script you need to:
1. Create a new empty Solana wallet
2. Transfer some SOL to it
3. Convert some SOL to USDC or WSOL (you need USDC or WSOL depending on the configuration set below)

`Jupiter Wrap` : https://jup.ag/

![](https://github.com/spaceboundbot/solana-sniper-bot/blob/a1973f4d8fc0001b31e97c5793199afbd48379e9/readme/jupiterwrap.png)

## 🚀 CONFIG
1. Configure the script by updating `.env.copy` file (**remove the .copy from the file name when done**).
2. `PRIVATE_KEY` (your wallet private key)
3. `RPC_ENDPOINT` (https RPC endpoint)
4. `RPC_WEBSOCKET_ENDPOINT` (websocket RPC endpoint)
5. `QUOTE_MINT` (which pools to snipe, USDC or WSOL)
6. `QUOTE_AMOUNT` (amount used to buy each new token)
7. `COMMITMENT_LEVEL`
8. `CHECK_IF_IS_BURNED` (liquidity burn check, in progress)
9. `CHECK_IF_IS_LOCKED` (liquidity lock check, in progress)
10. `RUG_SCORE_MAX` (in progress)
11. `USE_SNIPE_LIST` (buy only tokens listed in snipe-list.txt)
12. `SNIPE_LIST_REFRESH_INTERVAL` (how often snipe list should be refreshed in milliseconds)
13. `CHECK_IF_MINT_IS_RENOUNCED` (script will buy only if mint is renounced)
14. `MIN_POOL_SIZE` (script will buy only if pool size is greater than specified amount)
15. `TAKE_PROFIT=300` (in %)
16. `STOP_LOSS=50` (in %)
17. `BIRDEYE_API_KEY=` generate here : https://birdeye.so/user/profile#security

![](https://github.com/spaceboundbot/solana-sniper-bot/blob/505b1f5c4d8a9bc5fbbd0e5016d1b4bb1afa68d0/readme/botenv.png)
  
## 🛸 INSTALL
1. Install dependencies by typing: `npm install`
2. Run the script by typing: `npm run buy` in terminal

![](https://github.com/spaceboundbot/solana-sniper-bot/blob/cc1a8b2fc3f88b2a64dd254cdf6ae83124cd0578/readme/preview.png)

## 🛰 TAKE PROFIT

> [!NOTE]
> By default, 300 %

## 🎇 STOP LOSS

> [!NOTE]
> By default, 50 %

## 🌌 AUTO SELL
By default, auto sell is enabled. If you want to disable it, you need to:
1. Change variable `AUTO_SELL` to `false`
2. Update `MAX_SELL_RETRIES` to set the maximum number of retries for selling token
3. Update `AUTO_SELL_DELAY` to the number of milliseconds you want to wait before selling the token (this will sell the token after the specified delay. (+- RPC node speed)).

If you set AUTO_SELL_DELAY to 0, token will be sold immediately after it is bought.
There is no guarantee that the token will be sold at a profit or even sold at all. The developer is not responsible for any losses incurred by using this feature.

![](https://github.com/spaceboundbot/solana-sniper-bot/blob/b89b45022cf82ac9bcc592338bedbceaac1c3354/readme/token.png)

## 👾 SNIPE LIST
By default, script buys each token which has a new liquidity pool created and open for trading. 
There are scenarios when you want to buy one specific token as soon as possible during the launch event.
To achieve this, you'll have to use snipe list.
1. Change variable `USE_SNIPE_LIST` to `true`
2. Add token mint addresses you wish to buy in `snipe-list.txt` file (add each address as a new line).

This will prevent script from buying everything, and instead it will buy just listed tokens.
You can update the list while script is running. Script will check for new values in specified interval (`SNIPE_LIST_REFRESH_INTERVAL`).

Pool must not exist before the script starts.
It will buy only when new pool is open for trading. If you want to buy token that will be launched in the future, make sure that script is running before the launch.

![](https://github.com/spaceboundbot/solana-sniper-bot/blob/f796bc6c1f9cd2df3f4f4790a81f1039f9602ab8/readme/snipelist.png)


## 🚀 COMMON ISSUES

> [!IMPORTANT]
> If you have an error which is not listed here, please create a new issue in this repository.
> To collect more information on an issue, please change `LOG_LEVEL` to `debug`.
> 
> ### EMPTY TRANSACTION
> If you see empty transactions on SolScan most likely fix is to change commitment level to `finalized`.
> 
> ### UNSOPPORTED RPC NODE
> If you see following error in your log file:  
> `Error: 410 Gone:  {"jsonrpc":"2.0","error":{"code": 410, "message":"The RPC call or parameters have been disabled."}, "id": "986f3599-b2b7-47c4-b951-074c19842bad" }`  
> It means your RPC node doesn't support methods needed to execute script.
> FIX: Change your RPC node. You can use Shyft, Helius or Quicknode.
> 
> ### NO TOKEN ACCOUNT
> If you see following error in your log file:  
> `Error: No SOL token account found in wallet: `  
> it means that wallet you provided doesn't have USDC/WSOL token account.
> FIX: Go to dex and swap some SOL to USDC/WSOL. When you swap sol to wsol you should see it in wallet.

## 🛸 CONTACT
Discord: `@spaceboundbot`

## 🛰 DISCLAIMER

> [!IMPORTANT]
> Use this script at your own risk.

> `This script may contain various bugs.`
