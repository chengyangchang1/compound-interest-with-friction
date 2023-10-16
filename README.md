# 在有市場摩擦下的複利模型

2016 年 10 月 3 日，[v神在 reddit 上提出了恆定乘積做市的模型](https://www.reddit.com/r/ethereum/comments/55m04x/lets_run_onchain_decentralized_exchanges_the_way/)。2018 年 11 月 2 日，[Uniswap 在以太坊上部屬第一個合約](https://twitter.com/haydenzadams/status/1058376395108376577?ref_src=twsrc%5Etfw)，從此以後我們可以在區塊鍊上運行去中心化交易所（AMM）。

> **Abstract**: With a traditional central bank deposit, the bank will pay us interest based on the classical compound interest model. The classical compound interest model assumes that the handling fee is zero, and the interest that the bank gives us has already taken into account and deducted the necessary handling fee, so all we have to do is collect the interest without doing anything else. However, in the case of decentralised contracts, no one will provide such a service and we will have to do it manually. For example, if you lock a position on Uniswap, you will generate revenue every day, and you can unlock the position and then lock it again to inject liquidity, but both unlocking and injecting liquidity require a fee, and the newly injected assets will also generate additional revenue, so we have to calculate the optimal deposit cycle ourselves and when we should perform the operation to maximise revenue. In the case where the handling fee is proportional to the amount of funds, we find the period that optimises the return in this particular case. We then approximate this to the case where the fee is fixed and discuss whether such an approximation is reasonable. We use the formula for the real example and discuss the effect of the handling fee on the interest rate.
>
> **摘要**：在傳統的中心化銀行存款，銀行會給我們利息，而這種利息的基礎是來自古典的複利模型，古典的複利模型是假設手續費為零的，而銀行給我們的利息已經考慮並扣除了必要手續費，我們只需要放著領利息就好，不用做其他的操作。但如果是在去中心化的合約上，不會有人提供這樣的服務，必須自己手動操作。比如說在Uniswap上鎖倉，每天會產生手續費，手續費可以解鎖後再鎖倉注入流動性，但是解鎖與輸入流動性都要手續費，而新注入的資產也會額外產生收益，所以我們必須自己計算最佳的存入週期，應該在甚麼時候執行操作，才能讓收益最大化。

English version (Work in progress):

> - [Markdown]()
> - [PDF]()

中文版（施工中）：

> - [Markdown](compound-interest-with-friction-zh.md)
> - [PDF]()

會有這篇文章是因為我在 Uniswap 上鎖倉賺取收益，但是在 Uniswap 做市產生的收益不能自動回存到流動池中。所以需要手動的提出收益，然後再存入流動池中才能產生複利。因此我需要知道怎樣的存入週期才會使成長率最大，好讓我賺更多的錢。但是我找了很久實在是找不到有人有做過類似的事情，只好自己推導。同時作為練習用 Markdown 格式寫論文。

這篇文章是作為 Markdown 論文格式的實驗與練習，與做為開源論文的實驗，有任何錯誤、建議或想法都歡迎提出。格式參考李笑來翻譯的《[比特币白皮书中文版](https://github.com/xiaolai/bitcoin-whitepaper-chinese-translation)》。排版參考 [sparanoid/chinese-copywriting-guidelines](https://github.com/sparanoid/chinese-copywriting-guidelines)。
