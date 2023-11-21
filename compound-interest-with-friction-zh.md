# 在交易有摩擦情況下的複利模型

作者：Cheng-Yang Chang chengyangchang1@gmail.com 2023.9.19

[Check it out on GitHub](https://github.com/chengyangchang1/compound-interest-with-friction)



> **摘要**：在傳統的中心化銀行存款，銀行會給我們利息，而這種利息的基礎是來自古典的複利模型，古典的複利模型是假設手續費為零的，而銀行給我們的利息已經考慮並扣除了必要手續費，我們只需要放著領利息就好，不用做其他的操作。但如果是在去中心化的合約上，不會有人提供這樣的服務，必須自己手動操作。比如在Uniswap上鎖倉，每天會產生收益，收益可以解鎖後再鎖倉注入流動性，但是解鎖與輸入流動性都要手續費，而新注入的資產也會額外產生收益，所以我們必須自己計算最佳的存入週期，以及何時執行操作才能讓收益最大化。在手續費與資金量呈正比時，我們求出了在這種特定的情況下使收益最佳化的週期。然後我們把它近似到手續費固定時的情況上，並討論這種近似是否合理。我們用公式進行實際例子的計算，並且我們討論了手續費對利率的影響。

-----

## 1. 引言 (Introduction)


古典的複利模型，是指我們把單利的利息不斷的提領並再存入，當再存入沒有手續費時（沒有成本），我們會發現應該讓再存入的間隔的時間趨近於零，這樣會讓收益最大化。我們知道這就是指數函數的定義，也是複利成長的模型。

但是這種無手續費的假設，並不一定符合真實的狀況，如果我們是存在中心化的銀行，銀行給我們的利率已經考慮了手續費，銀行已經扣除了他們需要的費用與利潤，我們只需要放著領利息就好，不用做特殊的操作。但如果是在去中心化的合約上，不會有人提供這樣的服務，所以我們必須自己計算最佳的存入週期，自己手動操作。

舉例來說：在Uniswap上鎖倉，每天會產生收益，這些收益可以解鎖後再重新鎖倉注入流動性。但是解鎖與輸入流動性都要繳交鍊上礦工手續費，而新注入的資產會產生額外的收益，我們應該在甚麼時候（或是未領取手續費到達多少時）執行操作（提領並再存入），才能讓收益最大化。

由於我們進行提領利息再存入的這個動作需要手續費，所以我們就不能像古典的情況一樣讓再存入的時間間隔趨近於 $0$，因為時間間隔越短，會讓手續費增加，但是時間間隔越長，會讓複利效果下降。所以我們應該可以找到一個合適的時間間隔，讓收益最大化。如果手續費降低到 $0$，此時就退化成古典的複利模型。

## 2. 古典無手續費的複利模型 (The classical model of compound interest without fees)

我們回顧古典的指數函數定義，在單位時間內的複利成長公式[^1]如下所示：

$$
(\displaystyle 1 + \displaystyle \frac{1}{\displaystyle n} )^{\displaystyle n}
$$

當再存入沒有手續費時，我們會發現當 $n$ 趨向無限時，收入最大，且雖然表面上利率是 $1$，但由於不斷的複利投入所以利率從 $1$ 變為 $e$。現在我們讓利率（單利）從 $1$ 變為 $a$，單位時間內的複利成長公式變為：

$$
(\displaystyle 1 + \displaystyle \frac{a}{\displaystyle n} )^{\displaystyle n}
$$

讓 $n$ 趨向無限，得到：

$$
\lim_{n \to \infty} (\displaystyle 1 + \displaystyle \frac{a}{\displaystyle n} )^{\displaystyle n} = e^{\displaystyle a}
$$

## 3. 當交易有摩擦時的複利機制 (The mechanism of compound interest when trading is not frictionless)

在一次的存入週期中，我們把一開始存入的資金稱為初始資金，我們叫它 $V_i$。從上一次存入後累積的利息收入我們叫它 $I$。我們把初始資金加利息收入就會得到總資金我們令它為 $V_f$，我們可以得到 $V_i + I = V_f$。現在我們假設利率是 $a$，也就是說如果放著不動，經過單位時間後，累積到的利息收入為 $\displaystyle V_i a$。假設進行提領再存入需要手續費 $F$，我們現在定義一個新的量 $\beta$，可以寫為 $\displaystyle\beta = \frac{V_f - F}{V_f} = \frac{V_i + I - F}{V_i + I}$，可以認為 $\beta$ 代表經過再存入後剩餘的資金與存入前總資金的比，所以 $\beta V_f$ 也是下一個周期的初始資金，也就是說上一個周期的總資金乘 $\beta$ 以後是下一個周期的初始資金，因此 $\beta$ 可以當作每次再存入後的剩餘資金比例，你可以當作 $\beta$ 越接近 $1$ 手續費越低，而 $\beta$ 越接近 $0$ 手續費越高（ $\beta < 1$，所以 $1 - \beta$ 就是收取手續費的費率）。

現在我們讓利率一樣是 $a$，假設單位時間一樣分割 $n$ 次（這裡假設分割的時間間隔是均勻的），則每次的資金變為前一次的：

$$
\left ( 1+\frac{a }{\displaystyle n} \right )
$$

但是現在假設提領收益並再存入需要手續費，則我們令再存入之後資金量變為原來的 $\beta$ 倍。**假設提領再存入所需要的手續費與當前的資金量成正比**，因為 $F \propto V_f$ 所以 $\beta$ 跟 $V_f$ 無關，我們可以發現此時是 $\beta$ 定值。則收了手續費後每次存入資金變為前一次的：

$$
\beta \left ( 1+\frac{a }{\displaystyle n} \right )
$$

由於單位時間內要重複這個動作 $n$ 次，所以單位時間後總資金變為原本的：

$$
\left [ \beta \left ( 1+\frac{a }{\displaystyle n} \right ) \right ] ^{\displaystyle n} \tag{1}
$$

我們可以想像當有手續費時，如果太頻繁操作，手續費會侵蝕獲利，導致收益下降，而如果操作周期太長，會減弱複利的效果，同樣造成收益下降。若我們想知道怎樣的 $n$，可以使收益最大化。我們可以對 $n$ 微分，極值點就在微分等於 $0$ 的地方。我們讓 $\displaystyle \beta \left ( 1+\frac{a }{\displaystyle n} \right ) = g$，則對上面的式子 $(1)$ 的微分就變為對 $g^{\displaystyle n}$ 的微分。由於我們只是要求微分等於 $0$ 時的 $n$ 值是多少，而對數函數是增函數。所以取對數後使微分等於 $0$ 的 $n$，也是使原函數微分後等於 $0$ 的 $n$。所以我們可以得到：

$$
\begin{align*}
&\displaystyle \frac{\mathrm{d}}{\mathrm{d} n} \left ( g^{\displaystyle n} \right ) = 0 \\
\Rightarrow \ &\displaystyle \frac{\mathrm{d}}{\mathrm{d} n} \ln{\left ( g^{\displaystyle n} \right ) } = 0 \\
\Rightarrow \ &\displaystyle \ln{g} + n \frac{g^{\prime } }{\displaystyle g} = 0 \tag{2} \\
\end{align*}
$$

將 $\displaystyle \beta \left ( 1+\frac{a }{\displaystyle n} \right ) = g$ 代入 $(2)$，化簡後得到：

$$
\ln{\left [ \displaystyle \beta \left ( 1+\frac{a }{\displaystyle n} \right ) \right ]} = \frac{1}{\displaystyle \left ( 1+\frac{a }{\displaystyle n} \right )}\frac{a }{\displaystyle n} \tag{3}
$$

我們令 $\displaystyle \frac{\displaystyle a }{\displaystyle n} = A$，代入上式 $(3)$ 就可以改寫成：

$$
\ln{\left [ \beta \left ( 1 + A \right ) \right ]} = \frac{1}{\left ( 1 + A \right )}A \tag{4}
$$

再把上式化簡可以得到：

$$
\beta = \frac{e^{\frac{\displaystyle A}{\displaystyle 1 + A} } }{1 + A} \tag{5}
$$

我們可以發現 $\beta$ 是 $A$ 的函數，當 $A$ 確定時 $\beta$ 也確定了。因為 $a$ 一定大於等於 $0$，所以 $A$ 也一定大於等於 $0$，反過來說當 $\beta$ 確定時 $A$ 也唯一確定了，也就是說 $A$ 也是 $\beta$ 的函數。所以當 $\beta$ 決定時，只要求這個方程式的根就可以求出 $A$。

我們來看看 $A$ 是什麼，我們知道 $A = \displaystyle \frac{\displaystyle a }{\displaystyle n}$。我們會發現 $a$ 是單位時間的利率（單利），如果我們存入 $1$ 塊錢，經過單位時間後會有 $a$ 塊的利息收入，如果存入 $V_i$ 塊錢，經過單位時間後會有 $\displaystyle V_i a$ 塊的利息收入。但是我們在單位時間中切分成 $n$ 次，所以 $\displaystyle \frac{\displaystyle a }{\displaystyle n}$ 就是單位資金，從上一次再存入後累積的收入。如果初始資金是 $V_i$，則 $V_i\displaystyle \frac{\displaystyle a }{\displaystyle n}$ 就是從上一次再存入後累積的收入，可以寫為 $V_i A$，所以每次週期結束累計的利息可以寫為 $I = V_i A$。又我們知道當 $\beta$ 決定時 $A$ 也決定了，所以當 $\beta$ 決定時 $a$ 就跟 $n$ 成正比，並且它們之間的比值由 $\beta$ 決定。

為什麼我們不用 $a$ 跟 $n$ 而要用 $A$？因為在鍊上利率時常是波動的，所以在利率波動的情況下，計算出的 $n$ 也會波動，進而影響到我們得出的最佳存入的週期，使得存入週期也是波動的。但是我們會發現 $a$ 總是與 $n$ 一起出現（總是相除），所以 $A$ 只跟 $\beta$ 有關與 $a$ 無關。因此改用 $A$ 後就可以忽略利率波動的影響，只要給定 $\beta$ 後就可以求出 $A$，只要累積的利息收入與本來存入資金的比率到達 $A$，就是要再存入的時機。

我們接著看這個解的性質，我們會發現這個解如果改變時間的單位，比如我們用天當基本單位或用星期當基本單位，得出的結果也是一樣的。我們知道 $A = \displaystyle \frac{\displaystyle a }{\displaystyle n}$。由於是在單位時間內分割 $n$ 個次數，所以 $\displaystyle \frac{\displaystyle 1 }{\displaystyle n}$ 是最佳分割週期，我們可以得到 $\displaystyle \frac{\displaystyle 1}{\displaystyle n} = \frac{A}{a}$。由於當 $\beta$ 確定時 $A$ 也確定了，所以 $A$ 除以 $a$ 就是最佳分割週期。如果我們用天當單位，則 $a$ 是一天收到的利息，如果用星期當單位，則 $a$ 是一周收到的利息。當然以周為單位 $a$ 會變大 $7$ 倍，而 $\displaystyle \frac{\displaystyle 1 }{\displaystyle n}$ 也就是最佳分割週期，也縮小 $7$ 倍，但不要忘記此時的時間單位由天改為周，所以實際經過的時間是不變的。也就是說這個公式的解不會因為我們改變時間的單位，就造成最佳存入周期的時間跨度改變。這很合理，因為時間單位是人為設定的，不應該我們使用不同的單位制就造成結果的不同。

同樣的道理，如果我們改變資金的單位，比如我們把單位從美元改成美分，得出的結果也是一樣的。因為首先 $\beta$ 是比例假如說是 $0.9$ 好了，代表每次再存入資金會剩下百分之九十，又由於 $\beta$ 決定時 $A$ 也決定了，我們把 $\beta = 0.9$ 代入得出對應的 $A$ 為 $0.644$。因為 $A$ 是累積的利息收入與初始資金的比例，如果初始資金是 $1$ 美元，則累積的利息達到 $0.644$ 美元就是再存入的時機。如果我們改用美分計價，由於 $1$ 美元是 $100$ 美分 $A$ 為 $0.644$ 所以累積利息要達到 $64.4$ 美分，才是我們再存入的時機。而 $0.644$ 美元與 $64.4$ 美分相等，也就是說不管我們怎麼改變計價的單位，也不會影響最後的結果。因為這跟前面時間單位的例子相似，計價的單位是人為設定的，沒道理我們改變計價單位就造成結果不同。

我們現在再回頭看為什麼要讓手續費 $F$ 與總資金量 $V_f$ 成正比，因為這樣才能讓 $\beta$ 是定值，使得 $\beta$ 跟 $V_f$ 無關，可以得出 $\beta$ 與 $n$ 無關。如此我們對 $n$ 微分時，就可以把 $\beta$ 當作常數，進而使我們得到想要的答案。但是這樣的假設合理不合理呢？我們必須檢驗我們解的性質，它必須在改變時間單位與資金單位的情況下，也不會改變結果，因為時間單位與資金單位是人為設定的，沒理由改變單位就造成結果不同，那我們剛剛已經說明過了確實是這樣。並且我們一開始是假設分割時間的間隔是平均的，所以我們還必須證明當分割數 $n$ 固定時，平均的分割可以使收益最大，這個等一下會在下面做推演。如果我們的解可以符合前面說的所有條件，至少我們可以說它在我們假設的前提下（手續費與總資金量成正比時），是使成長率最快的解。

可以看我建立的[Desmos頁面](https://www.desmos.com/calculator/9imklswzy5?lang=zh-TW)，其中的 $x$ 軸代表 $n$，這裡紫色的線代表成長率，藍色的線代表成長率對 $n$ 微分，而橘色的虛線就是方程式 $(3)$ 的解。我們可以看到橘色虛線是一條直線，也就是橘色線的 $x$ 的數值，就是公式 $(3)$ 的解。我們會發現藍色的線（也就是成長率對 $x$ 的微分）它的根的 $x$ 值正好是橘色虛線的 $x$ 值。再看此時的紫色線也正好是最高點而不是最低點。也就是說我們用數值的方法驗證了，我們前面求出的方程式 $(3)$，的確就是使成長率最大的公式。你可以改變不同的 $\beta$ 跟 $a$ 來驗證，剛剛說的關係在不同的 $\beta$ 跟 $a$ 下都會成立。

## 4. 使成長率最大的分割間隔 (The best split interval to maximise growth rate)

但是我們前面是假設，每次再存入的時間間隔是平均的，如果間隔不平均，有沒有可能使成長率更高？想要知道怎樣的間隔可以讓收益最大化，我們看前面的式子 $(1)$。我們知道過了單位時間後資金變為 $\displaystyle\left [ \beta \left ( 1+\frac{a }{\displaystyle n} \right ) \right ] ^{\displaystyle n}$，我們可以拆解它，得到：

$$
\left [ \beta \left ( 1+\frac{a }{\displaystyle n} \right ) \right ] ^{\displaystyle n} = \beta^{\displaystyle n} \left ( 1+\frac{a }{\displaystyle n} \right )^{\displaystyle n} \tag{6}
$$

我們可以看到，單位時間的成長率由兩個部分構成，前半部分是 $\beta^{\displaystyle n}$ 後面是 $\displaystyle\left ( 1+\frac{a }{\displaystyle n} \right )^{\displaystyle n}$。我們看後面的 $\displaystyle\left ( 1+\frac{a }{\displaystyle n} \right )^{\displaystyle n}$ 這個部分，我們會發現這其實就是 $\displaystyle\left ( 1+\frac{a }{\displaystyle n} \right )$ 乘以 $n$ 次，那如果 $n$ 不是正整數怎麼辦？我們前面已經論證時間的單位是人為設定的，不會影響結果，所以如果 $n$ 不是正整數，我們只需要找一個恰當的時間單位，使得 $n$ 是整數。所以這裡我們只要假設 $n$ 是正整數就好，不會影響結果。繼續往下推導 $\displaystyle\left ( 1+\frac{a }{\displaystyle n} \right )$ 就是每次存入後到提領出來前總資金對比初始資金的倍率，單位時間分割 $n$ 次，所以每次的時間長度就是 $\displaystyle \frac{\displaystyle 1 }{\displaystyle n}$，我們令 $\displaystyle \frac{\displaystyle 1 }{\displaystyle n}$ 為一個新的物理量，稱它為 $x$。把 $x$ 代入 $\displaystyle\left ( 1+\frac{a }{\displaystyle n} \right )$ 就變為 $\displaystyle\left ( 1 + x a \right )$。現在如果分割間隔的時間不是平均的，我們令第 $1$ 次再存入的時間間隔是 $x_1$，第 $2$ 次的間隔是 $x_2$，依此類推到第 $n$ 次是 $x_n$。在時間分割間隔不平均的情況下 $\displaystyle\left ( 1+\frac{a }{\displaystyle n} \right )^{\displaystyle n}$ 就可以寫為 $\displaystyle\left ( 1 + x_1 a \right )\left ( 1 + x_2 a \right ) \cdots \left ( 1 + x_n a \right )$，我們把它代入上面的成長率公式 $(6)$，成長率就可以寫為：

$$
\displaystyle\beta^{\displaystyle n}\left ( 1 + x_1 a \right )\left ( 1 + x_2 a \right ) \cdots \left ( 1 + x_n a \right ) \tag{7}
$$

我們可以改用 $\displaystyle\left ( 1 + x_1 a \right )\left ( 1 + x_2 a \right ) \cdots \left ( 1 + x_n a \right )$ 的**幾何平均數(Geometric mean)**[^2]來取代它，我們令幾何平均數為 $G$，因為 $G^{\displaystyle n} = \displaystyle\left ( 1 + x_1 a \right )\left ( 1 + x_2 a \right ) \cdots \left ( 1 + x_n a \right )$。把 $G^{\displaystyle n}$ 代入上面的式子 $(7)$ 我們可以把成長率改寫為：

$$
\displaystyle\beta^{\displaystyle n} G^{\displaystyle n} \tag{8}
$$

我們會發現考慮手續費後的成長率，正好是古典分割 $n$ 次的複利成長公式乘上 $\beta^{\displaystyle n}$。但是當分割次數是固定的情況下，我們會發現不論分割間隔如何改變，都不會影響到分割的總次數 $n$，所以 $\beta^{\displaystyle n}$ 不會改變。如果我們希望成長率是極大值，由於 $n$ 是定值不會改變，所以只要 $G$ 越大，成長率也就越高。也就是說使 $G$ 最大的時間間隔分布，就是使成長率最大的時間間隔分布。我們看看**算術平均數(Arithmetic mean)**[^3] $\displaystyle\left ( 1 + x_1 a \right )\left ( 1 + x_2 a \right ) \cdots \left ( 1 + x_n a \right )$ 的算術平均數是：

$$
\displaystyle\frac{n + \left ( x_1 + x_2 + \cdots + x_n \right ) a}{n} \tag{9}
$$

但是 $\left ( x_1 + x_2 + \cdots + x_n \right ) = 1$，因為單位時間是分割成 $n$ 份，所以不管如何分割，這些時間間隔加起來一定是單位時間也就是 $1$。我們把它代入 $(9)$ 式化簡後得到 $\displaystyle\left ( 1 + \frac{a}{n} \right )$，我們會發現不論分割間隔為何，由於 $n$ 是固定的，所以算術平均數總是相同的值。我們看看**算幾不等式**[^4]，算術平均數總是大於等於幾何平均數，只有數列中每一個數都相等時，它們才會相等。也就是說當 $x_1 = x_2 = \cdots = x_n$ 時，此時的 $G$ 大於等於其他所有可能的幾何平均數，也就是說此時的 $G$ 是最大的。所以我們證明使 $G$ 最大的時間間隔分布為平均分布，此時 $\displaystyle G = \left ( 1 + \frac{a}{n} \right )$，代入前面的公式 $(8)$，此時成長率寫為：

$$
\beta^{\displaystyle n} \left ( 1+\frac{a }{\displaystyle n} \right )^{\displaystyle n}
$$

我們會發現又回到我們最一開始的公式，所以我們證明了當分割次數 $n$ 固定時，使成長率最高的分割是平均分割。前面又得出了當時間間隔是均勻分布的時候，使成長率最高的 $n$ 的公式。因此可以得出，如果讓時間間隔與分割次數都可以自由改變，我們前面求的公式依然是讓成長率最高的公式，因為我們的公式可以同時滿足這兩個條件。

## 5. 等效利率 (Equivalent interest rate)

現在我們了解到了，當交易有摩擦時，我們並不能像傳統的複利一樣，無限短的提領再存入，而是必須在利息收入到達一定量的時候再操作，才能到達最佳的成長效果。也就是說這種交易的摩擦會造成我們資金利用能力的下降，也就是我們實際上收到的利率會小於交易無摩擦時的利率。我們想知道這樣的損失會造成多大的影響。

回顧古典複利，我們知道：

$$
\lim_{n \to \infty} \left ( \displaystyle 1 + \displaystyle \frac{a}{\displaystyle n}  \right )^{\displaystyle n} = e^{\displaystyle a}
$$

意思是說經過單位時間，資金會成長為原來的 $e^{\displaystyle a}$ 倍，其中 $a$ 我們稱為**對數報酬率(logarithmic rate of return)**[^5]。現在我們想知道當當交易有摩擦時，如果我們嚴格依照最佳循環週期來操作，則在利率維持 $a$ 不變的情況下，我們等效的對數報酬變為多少。

由於前面我們已經知道了，在考慮交易手續費的情況下，經過單位時間後資金會成長為原來的 $\displaystyle\left [ \beta \left ( 1+\frac{\displaystyle a }{\displaystyle n} \right ) \right ] ^{\displaystyle n}$ 倍，我們令它等於 $e^{\displaystyle b}$，其中 $b$ 就是等效對數報酬率：

$$
\left [ \beta \left ( 1+\frac{a }{\displaystyle n} \right ) \right ] ^{\displaystyle n} = e^{\displaystyle b}
$$

同時對它們取對數，化簡後得到：

$$
n\ln{\left [ \beta \left ( 1+\frac{a }{\displaystyle n} \right ) \right ]} = b
$$

又前面我們已經知道符合最佳循環週期的 $n$ 是 $\displaystyle\ln{\left [ \displaystyle \beta \left ( 1+\frac{a }{\displaystyle n} \right ) \right ]} = \frac{1}{\displaystyle \left ( 1+\frac{a }{\displaystyle n} \right )}\frac{a }{\displaystyle n}$ 的解，代入上面的式子化簡後得到：

$$
b = \frac{1}{\left ( 1 + A \right ) } a
$$

我們引入一個新的量，我們令 $\displaystyle\gamma = \frac{b}{a}$，然後代入並將上面的式子化簡後得到：

$$
\gamma = \frac{1}{(1 + A)}
$$

我們會發現因為 $A$ 一定大於 $0$，所以 $\gamma$ 一定小於 $1$。並且 $\gamma$ 跟 $a$ 無關只跟 $\beta$（或 $A$）有關。也就是說摩擦力會造成我們實際的收益的下降，並且對數報酬率的下降，正好等於初始資金與再存入前總資金的比值。

並且移項後我們可以得到 $\displaystyle(1 + A) = \frac{1}{\gamma }$，與 $\displaystyle A = \frac{1}{\gamma } - 1$，將它們代入上面的方程式 $(5)$ 化簡後得到：

$$
\displaystyle\beta = \gamma e^{\displaystyle1 - \gamma } \tag{10}
$$

## 6. 擴展到手續費固定時的狀況 (Extended to the situation where the handling fee is fixed)

然而，在鍊上交易更常見的狀況是，收取一個與總資金量無關的固定手續費，而不是百分比的手續費，因此我們要改寫我們的公式。在這裡我們直接假設前面推導出的公式（假設手續費與資金量成正比），可以套用到手續費固定時的情況，下面會討論這種假設是否合理。我們回來看我們之前得到的式子 $(4)$：

$$
\ln{\left [ \beta \left ( 1 + A \right ) \right ]} = \frac{1}{\left ( 1 + A \right )}A \tag{4}
$$

我們先看 $\beta$ 是什麼，我們知道 $\beta$ 是在存入後剩餘資金與總資金的比。由上面的定義我們知道 $\displaystyle\beta = \frac{V_f - F}{V_f} = \frac{V_i + I - F}{V_i + I}$。而 $A$ 是累積的利息收入與初始資金的比值，可以寫為 $\displaystyle A = \frac{I}{V_i}$。那麼 $\left ( 1 + A \right )$ 就可以寫成：

$$
\left ( 1 + A \right ) = 1 + \frac{I}{V_i} = \frac{V_f}{V_i}
$$

我們把 $\beta$ 跟 $A$ 代入之前得到的式子 $(4)$ 化簡後得到：

$$
\begin{align*}
&\ln{\left [ \beta \left ( 1 + A \right ) \right ]} = \frac{1}{\left ( 1 + A \right )}A \\
\Rightarrow \ &\ln{\left [ \frac{V_f - F}{V_f} \left ( \frac{V_f}{V_i} \right ) \right ] } = \frac{V_i}{V_f} \frac{I}{V_i} \\
\Rightarrow \ &\ln{\left ( \frac{V_f - F}{V_i} \right ) } = \frac{I}{V_f} \\
\end{align*}
$$

我們把 $V_f = V_i + I$ 代入上面的公式，就可以得到 $F$ 與 $I$ 的關係：

$$
\ln{\left ( \frac{V_i + I - F}{V_i} \right ) } = \frac{I}{V_i + I}
$$

化簡後可以得到：

$$
F = V_i + I - V_i \cdot e^{\displaystyle \frac{I}{V_i + I}} \tag{11}
$$

當 $V_i$ 跟 $F$ 確定時，只要求這個方程式的根就可以得到 $I$。意思是當手續費固定時，當我們累積的利息收入達到 $I$，就是要進行提領再存入操作的時機。

但是我們直接套用之前的公式是合理的嗎？因為之前的公式是假設手續費與資金量成正比，而現在則是讓手續費固定。如果 $V_i$ 遠大於 $F$，則我們可以認為這種近似是合理的。舉一個實際的例子，假設初始資金 $V_i$ 是 $100$ 美元，手續費 $F$ 是 $1$ 美元，代入上面的式子 $(11)$，得到此時 $I$ 等於 $14.5$ 美元，我們可以算出此時 $\beta$ 是 $0.991$，如果用這個 $\beta$ 計算初始資金 $V_i$ 的手續費，我們會得到手續費為 $0.87$ 美元，我們會發現 $0.87 \approx 1$，所以此時我們可以大致認為手續費不會變動。如果說在區塊鍊上，費用可能會在每個 gas $12$ 到 $20$ Gwei 之間波動，並且合約執行後實際的 gas used 也只能估算，在合約執行前也無法知道精確的數量。意思是說手續費的波動，已經大於將手續費假設為與資金量成正比造成的誤差。由於本來在鍊上的交易就有隨機性，所以你本來就不可能找到實際上的最佳解，因為你無法預測下一個區塊的手續費是多少，你只能預估。所以我們可以認為當 $V_i$ 足夠大時，這樣的近似就是合理的。

## 7. 計算 (Calculations)

現在我們來看實際的情況，假設我們在恆定乘積做市商，我們提供 $500$ 個 USDT 與 $500$ 個 USDC 的流動性。所以目前流動池的價值是 $1000$ 美元。這邊為了簡單起見，我們假設我們提供的資產它們之間交換的價格比值（在這裡永遠是 $1$ 個 USDT 換 $1$ 個 USDC）與整個流動池的總市值都不會變動。也就是 USDT 的數量永遠是 $500$ 個市值也是 $500$ 美元，USDC 也同理永遠是 $500$ 個共 $500$ 美元。既然它們對美元的市值不會變動，當然它們之間交換的價格也不會變動。並且收到的利息也都是穩定幣，所以收到的利息也不會因為幣價的波動而波動，而是近似於存款的利息，數量只會變多。所以我們可以把這樣的流動池對，認為它近似於我們一開始提到的有摩擦力的存款模型，這個流動池的市值就是前面說的初始資金 $V_i$，在現在的情況下就是 $1000$ 美元。

假設當我們要把賺到的利息再存入，假設要經過三個步驟，首先把利息提領出來，由於我們收到的手續費中 USDT 與 USDC 的比值，不一定跟我們的流動池中兩種資產的比值是相同的比例。所以我們還要交易把它們換成相同比例，最後再存入。總之不管要經過多少步驟，我們把所有會產生的成本都加起來，就是所需的手續費 $F$，假設在目前的狀況中所需的手續費（通常是該鍊的幣）換算成美金要 $20$ 美元。

我們代入上面的公式計算得到 $I$ 等於 $207$ 美元。可以看我建立的[Desmos頁面](https://www.desmos.com/calculator/aeull6siit)，我們把 $F$ 跟 $V_i$ 設定成我們要的數值，然後看兩個方程式的交點，就是我們要求的 $I$。

另一個方法是，移項後求下面這個方程式的根。

$$
V_i + I - V_i \cdot e^{\displaystyle \frac{I}{V_i + I}} - F = 0
$$

這是可以計算的，可以看我建立的[Desmos頁面](https://www.desmos.com/calculator/trcdgomtyn)，這裡橘色的線，就是我們的方程式，這個方程的根，就是我們方程式的解。你可以把 $F$ 跟 $V_i$ 設定成你需要的數值來幫你預測 $I$。

接著看另一個[Desmos頁面](https://www.desmos.com/calculator/dqropidfjb)，這裡 $x$ 軸是我們的 $F$，而 $y$ 軸是我們的 $I$。我們可以利用這個頁面觀察 $F$ 與 $I$ 之間的關係，我們會發現 $I$ 始終大於 $F$，這很合理如果收到的利息小於手續費，則我們的錢就會越換越少。而當 $F$ 趨近 $0$ 於時 $I$ 也趨近於 $0$，這代表回到古典沒有手續費的情況。

接著看 $\gamma$ 與 $\beta$ 的關係，當 $\beta$ 等於 $0.8$，代入之前的公式 $(10)$：

$$
\displaystyle\beta = \gamma e^{\displaystyle1 - \gamma }
$$

我們可以得到 $\gamma$ 等於 $0.472$。我們用一樣的方法求當 $\beta$ 等於 $0.9, 0.99, 0.999$ 的值得到：

```
   beta=0.9     gamma=0.608
   beta=0.99    gamma=0.865
   beta=0.999   gamma=0.956
```

可以看我建立的[Desmos頁面](https://www.desmos.com/calculator/5spwcqh4f7)，其中 $x$ 軸代表 $\beta$ 而 $y$ 軸代表 $\gamma$，觀察 $\beta$ 與 $\gamma$ 之間的關係。我們會發現當 $\beta$ 很接近 $1$ 時，斜率是趨近無窮大。從前面的計算也可以得知當 $\beta$趨近 $1$ 時 $\gamma$ 是陡峭的上升，而不是線性的趨近於 $1$。你看當 $\beta$ 是 $0.9$ 時 $\gamma$ 是 $0.608$ 而當 $\beta$ 是 $0.99$ 時 $\gamma$ 是 $0.865$，即使 $\beta$ 已經很接近 $1$ 了 $\gamma$ 距離 $1$ 還是有一段差距。也就是說即使 $\beta$ 很接近 $1$，當交易有摩擦時，那怕摩擦力很小，對實際利率的影響也是巨大的。或許這個結果也說明了為什麼傳統的金融業，在資金的體量不同時，資金利用的效率完全不同，比如若是有更多資金或更大的交易量，通常會獲得更好的條件，如：利率、手續費折扣、退佣等等。除了行銷的考量，或許也是因為考慮到了操作造成的成本對利率的影響，也就是本文所討論的主題。

## 8. 結論 (Conclusion)

在古典複利中由於沒有手續費，所以我們再存入操作的時間間隔要無限小，才能使收益最大。但是如果考慮了手續費則時間間隔不能過小，也不能過大。我們提出了一個考量手續費的複利模型，藉由假設手續費與資金量呈正比，我們得出了要使收益最佳化的解。我們發現這個解的結果是，當收益達到初始資金的一個比例時，要進行再存入的操作，與一開始和中間過程中的利率無關。並且證明它在改變時間單位與資金單位的情況下，也不會改變結果，這也側面驗證了我們假設的合理性。我們還討論了分割間隔對成長率的影響，最後發現當分割間隔是平均時，成長率最大。並且計算了手續費對利率的影響，得出了等效利率。由於在區塊鍊上的世界中，往往是收取固定手續費，所以我們拓展我們的模型到固定手續費的情況，並且討論了這種近似是否合理，並提供了網頁方便大家做計算。

-----

## 參考文獻 (References)


[^1]: **维基百科, s.v. "指数函数"** Wikipedia contributors (2023-06-15) <https://zh.wikipedia.org/w/index.php?title=%E6%8C%87%E6%95%B0%E5%87%BD%E6%95%B0&oldid=77692656>
[^2]: **维基百科, s.v. "几何平均数"** Wikipedia contributors (2023-04-14) <https://zh.wikipedia.org/w/index.php?title=%E5%87%A0%E4%BD%95%E5%B9%B3%E5%9D%87%E6%95%B0&oldid=76812687>
[^3]: **维基百科, s.v. "算术平均数"** Wikipedia contributors (2022-02-05) <https://zh.wikipedia.org/w/index.php?title=%E7%AE%97%E6%9C%AF%E5%B9%B3%E5%9D%87%E6%95%B0&oldid=70020320>
[^4]: **维基百科, s.v. "算术-几何平均值不等式"** Wikipedia contributors (2023-02-01) <https://zh.wikipedia.org/w/index.php?title=%E7%AE%97%E6%9C%AF-%E5%87%A0%E4%BD%95%E5%B9%B3%E5%9D%87%E5%80%BC%E4%B8%8D%E7%AD%89%E5%BC%8F&oldid=75780402>
[^5]: **Wikipedia, s.v. "Rate of return"** Wikipedia contributors (2023-08-06) <https://en.wikipedia.org/w/index.php?title=Rate_of_return&oldid=1169071015#Logarithmic_or_continuously_compounded_return>
