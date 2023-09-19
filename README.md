# 在有市場摩擦下的複利模型

2016 年 10 月 3 日，[v神在 reddit 上提出了恆定乘積做市的模型](https://www.reddit.com/r/ethereum/comments/55m04x/lets_run_onchain_decentralized_exchanges_the_way/)。2018 年 11 月 2 日，[Uniswap 在以太坊上部屬第一個合約](https://twitter.com/haydenzadams/status/1058376395108376577?ref_src=twsrc%5Etfw)，從此以後我們可以在區塊鍊上運行去中心化交易所（AMM）。

中文版（施工中）：

> - [Markdown]()
> - [PDF]()

English version (Work in progress):

> - [Markdown]()
> - [PDF]()

會有這篇文章是因為我在 Uniswap 上鎖倉賺取收益，但是在 Uniswap 做市產生的收益不能自動回存到流動池中。所以需要手動的提出收益，然後再存入流動池中才能產生複利。因此我需要知道怎樣的存入週期才會使成長率最大，好讓我賺更多的錢。但是我找了很久實在是找不到有人有做過類似的事情，只好自己推導。同時作為練習用 Markdown 格式寫論文。

這篇文章是作為 Markdown 論文格式的實驗，與做為開源論文的實驗，有任何錯誤、建議或想法都歡迎提出。格式參考李笑來翻譯的《[比特币白皮书中文版](https://github.com/xiaolai/bitcoin-whitepaper-chinese-translation)》。排版參考 [sparanoid/chinese-copywriting-guidelines](https://github.com/sparanoid/chinese-copywriting-guidelines)。
