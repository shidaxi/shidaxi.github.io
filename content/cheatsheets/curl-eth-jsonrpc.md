---
title: curl with Eth Json RPC
date: "2021-02-23T00:00:00+08:00"
cover: "cover.png"
tags: 
  - curl
  - eth
  - web3
keywords: 
  - curl
  - eth
  - web3
description: ""
showFullContent: false
readingTime: false
---

{{< carbon lang="shell" >}}

# get height

curl -s -H "Content-Type: application/json" https://bscrpc.com \
  -d '{"jsonrpc": "2.0", "method": "eth_blockNumber", "params":[], "id":1}' -s \
  | jq -r -j .result \
  | xargs -0 printf "%d\n"

curl -s -H "Content-Type: application/json" https://rpc.ankr.com/bsc \
  -d '{"jsonrpc": "2.0", "method": "eth_blockNumber", "params":[], "id":1}' -s \
  | jq -r -j .result \
  | xargs -0 printf "%d\n"

curl -s -H "Content-Type: application/json" https://rpc.ankr.com/eth \
  -d '{"jsonrpc": "2.0", "method": "eth_blockNumber", "params":[], "id":1}' -s \
  | jq -r -j .result \
  | xargs -0 printf "%d\n"

{{< /carbon >}}
