# Compound interest model in the presence of trading frictions

Author: Cheng-Yang Chang chengyangchang1@gmail.com 2023.9.19

[Check it out on GitHub](https://github.com/chengyangchang1/compound-interest-with-friction)



> **Abstract**: In a traditional centralised bank deposit, the bank will pay us interest based on the classical compound interest model. The classical compound interest model assumes that the handling fee is zero, and the interest that the bank gives us has already taken into account and deducted the necessary handling fee, so all we have to do is collect the interest without doing anything else. However, in the case of decentralised contracts, no one will provide such a service and we will have to do it manually. For example, if you lock a position on Uniswap, you will generate revenue every day, and you can unlock the position and then lock it again to inject liquidity, but both unlocking and injecting liquidity require a fee, and the newly injected assets will also generate additional revenue, so we have to calculate the optimal deposit cycle ourselves and when we should perform the operation to maximise revenue. In the case where the handling fee is proportional to the amount of funds, we find the period that optimises the return in this particular case. We then approximate this to the case where the fee is fixed and discuss whether such an approximation is reasonable. We use the formula for the real example and discuss the effect of the handling fee on the interest rate.

-----

## 1. Introduction


The classical model of compound interest is that we withdraw and re-deposit simple interest over and over again, and when there is no handling fee for re-deposits (no cost), we find that we should let the time between re-deposits approach zero, which will maximise revenue. We know that this is the definition of an exponential function, and that it is a model of compound interest growth.

However, this assumption of no handling fee does not necessarily correspond to the real situation. If we are in a centralised bank, the interest rate offered by the bank has already taken into account the handling fee, and the bank has already deducted the fees and profits they need to make, so we just need to leave it to collect the interest, and we don't have to do any operations. But in the case of a decentralised contract, no one will provide such a service, so we have to calculate the optimal deposit cycle ourselves and do it manually.

For example, a locked position on Uniswap generates daily revenue, which can be unlocked and re-injected into liquidity. However, both unlocking and re-depositing of liquidity are subject to a chain miner's fee, and the newly injected assets generate additional revenue, so when should we perform the operation (withdrawal and re-deposit) in order to maximise the revenue.

Since there is a handling fee for the withdrawal and re-deposit, we can't make the time interval for re-deposit as close to $0$ as in the classical case, because the shorter the time interval, the higher the handling fee, but the longer the time interval, the lower the compounding effect. So we should be able to find a suitable time interval to maximise revenue. If the handling fee is decreased to $0$, it degenerates to the classical compound interest model.

## 2. The classical model of compound interest without fees

We review the classical definition of the exponential function, and the formula for compound interest growth in unit time[^1] looks like this:

$$
(\displaystyle 1 + \displaystyle \frac{1}{\displaystyle n} )^{\displaystyle n}
$$

When there is no handling fee for re-deposits, we find that the revenue is greatest when $n$ tends to infinity, and although the interest rate is ostensibly $1$, it changes from $1$ to $e$ because of the constant compounding of interest inputs. Now we let the interest rate (simple interest) change from $1$ to $a$, and the formula for the growth of compound interest per unit of time becomes:

$$
(\displaystyle 1 + \displaystyle \frac{a}{\displaystyle n} )^{\displaystyle n}
$$

Let $n$ tends to infinity, we get:

$$
\lim_{n \to \infty} (\displaystyle 1 + \displaystyle \frac{a}{\displaystyle n} )^{\displaystyle n} = e^{\displaystyle a}
$$

## 3. The mechanism of compound interest when trading is not frictionless

In a deposit cycle, we call the funds deposited at the beginning the initial funds, which we call $V_i$. The interest income accumulated since the last deposit is called $I$. We add the initial deposit to the interest income to get the total funds which we call $V_f$ and we get $V_i + I = V_f$. Now let's assume that the interest rate is $a$, that is, if we leave it alone, after a unit of time, the interest income accumulated will be $\displaystyle V_i a$. Assuming that there is a handling fee of $F$ for re-deposit, we now define a new quantity $\beta$, which can be written as $\displaystyle\beta = \frac{V_f - F}{V_f} = \frac{V_i + I - F}{V_i + I}$. So $\beta$ can be considered as the ratio of the remaining funds after re-deposit to the total funds before deposit, so $\beta V_f$ is also the initial funds for the next cycle, that is, the total funds of the previous cycle multiplied by $\beta$ is the initial funds for the next cycle, so $\beta$ can be taken as the ratio of the remaining funds for each re-deposit. The closer $\beta$ is to $1$, the lower the handling fee is, and the closer $\beta$ is to $0$, the higher the handling fee is ($\beta < 1$, so $1 - \beta$ is the rate at which the handling fee is charged).

Now let the interest rate be $a$, and suppose that a unit of time is split $n$ times (where the time interval between splits is assumed to be uniform), so that the amount of money each time becomes:

$$
\left ( 1+\frac{a }{\displaystyle n} \right )
$$

But now, assuming that there is a fee for withdrawing and re-depositing the revenue, we make the re-deposited funds equal to the original funds multiplied by $\beta$. **Assuming that the handling fee for withdrawals and re-deposits is proportional to the current amount of funds**, since $F \propto V_f$, $\beta$ is independent of $V_f$, and we can see that $\beta$ is a constant value at this point. Then each time you make a deposit after the handling fee is charged, it becomes:

$$
\beta \left ( 1+\frac{a }{\displaystyle n} \right )
$$

Since this action is repeated $n$ times per unit of time, the total amount of money after a unit of time becomes:

$$
\left [ \beta \left ( 1+\frac{a }{\displaystyle n} \right ) \right ] ^{\displaystyle n} \tag{1}
$$

We can imagine that when there is a commission, if the operation is too frequent, the commission will erode the profitability and lead to a decrease in revenue, and if the operation period is too long, it will weaken the effect of compounding, which will also lead to a decrease in revenue. If we want to know what $n$ to maximise revenue. We can differentiate with respect to $n$ such that the extreme point is where the differential is equal to $0$. We let $\displaystyle \beta \left ( 1+\frac{a }{\displaystyle n} \right ) = g$, then the differential of the above equation $(1)$ becomes the differential of $g^{\displaystyle n}$. Since we are only asking for the value of $n$ when the differential is equal to $0$, and the logarithm function is an increasing function. So the $n$ that is equal to $0$ after differentiate by taking the logarithm is also the $n$ that is equal to $0$ after differentiate by the original function. So we can get:

$$
\begin{align*}
&\displaystyle \frac{\mathrm{d}}{\mathrm{d} n} \left ( g^{\displaystyle n} \right ) = 0 \\
\Rightarrow \ &\displaystyle \frac{\mathrm{d}}{\mathrm{d} n} \ln{\left ( g^{\displaystyle n} \right ) } = 0 \\
\Rightarrow \ &\displaystyle \ln{g} + n \frac{g^{\prime } }{\displaystyle g} = 0 \tag{2} \\
\end{align*}
$$

Substituting $\displaystyle \beta \left ( 1+\frac{a }{\displaystyle n} \right ) = g$ into $(2)$ and can be reduced to:

$$
\ln{\left [ \displaystyle \beta \left ( 1+\frac{a }{\displaystyle n} \right ) \right ]} = \frac{1}{\displaystyle \left ( 1+\frac{a }{\displaystyle n} \right )}\frac{a }{\displaystyle n} \tag{3}
$$

Let $\displaystyle \frac{\displaystyle a }{\displaystyle n} = A$, and substituting into the above equation $(3)$, we get:

$$
\ln{\left [ \beta \left ( 1 + A \right ) \right ]} = \frac{1}{\left ( 1 + A \right )}A \tag{4}
$$

Reducing the above equation, we get:

$$
\beta = \frac{e^{\frac{\displaystyle A}{\displaystyle 1 + A} } }{1 + A} \tag{5}
$$

We can see that $\beta$ is a function of $A$, and when $A$ is determined $\beta$ is also determined. Because $a$ must be greater than or equal to $0$, so $A$ must be greater than or equal to $0$. Conversely, when $\beta$ is determined, $A$ is also determined, which means that $A$ is also a function of $\beta$. So when $\beta$ is determined, $A$ can be found by finding the roots of this equation.

Let's look at what $A$ is, we know that $A = \displaystyle \frac{\displaystyle a }{\displaystyle n}$. We will find that $a$ is the interest rate per unit of time (simple interest). If we deposit $1$ dollar, after a unit of time we will have $a$ dollars of interest income, and if we deposit $V_i$ dollars, after a unit of time we will get $\displaystyle V_i a$ dollars of interest income. But we slice it into $n$ times in a unit of time, so $\displaystyle \frac{\displaystyle a }{\displaystyle n}$ is the income accumulated per unit of money since the last re-deposit. If the initial funds are $V_i$, then $V_i\displaystyle \frac{\displaystyle a }{\displaystyle n}$ is the income accumulated from the last re-deposit, which can be written as $V_i A$, and so the interest accumulated at the end of each cycle can be written as $I = V_i A$. We also know that when $\beta$ is determined, $A$ is also determined, so when $\beta$ is determined, $a$ is proportional to $n$, and the ratio between them is determined by $\beta$.

Why do we use $A$ instead of $a$ and $n$? Because interest rates fluctuate on-chain, so the $n$ calculated when interest rates fluctuate will also fluctuate, which in turn affects the optimal deposit period we get, and makes the deposit period fluctuate as well. However, we will find that $a$ always appears with $n$ (always divides), so $A$ is only related to $\beta$ and independent of $a$. Therefore, by using $A$, the effect of interest rate fluctuations can be ignored. Given $\beta$, $A$ can be calculated, and as long as the ratio of accumulated interest income to the original deposit reaches $A$, it is time to re-deposit.

為什麼我們不用 $a$ 跟 $n$ 而要用 $A$？因為在鍊上利率時常是波動的，所以在利率波動的情況下算出的 $n$ 也會波動，進而影響到我們得出的最佳存入的週期，使得存入週期也是波動的。但是我們會發現 $a$ 總是與 $n$ 一起出現（總是相除），所以 $A$ 只跟 $\beta$ 有關與 $a$ 無關。因此改用 $A$ 後就可以忽略利率波動的影響，只要給定 $\beta$ 後就可以求出 $A$，只要累積的利息收入與本來存入資金的比率到達 $A$，就是要再存入的時機。

We then look at the property of this solution, and we see that this solution gives the same result if we change the base unit of time, for example, if we replace the day with the week as the base unit. We know that $A = \displaystyle \frac{\displaystyle a }{\displaystyle n}$. Since we are splitting $n$ times per unit of time, $\displaystyle \frac{\displaystyle 1 }{\displaystyle n}$ is the optimal splitting period, and we get $\displaystyle \frac{\displaystyle 1}{\displaystyle n} = \frac{A}{a}$. Since $A$ is determined when $\beta$ is determined, $A$ divided by $a$ is the optimal split period. If we use day as the base unit, then $a$ is the interest received in a day, and if we use week as the base unit, then $a$ is the interest received in a week. Of course, if we use week as the base unit, $a$ will become $7$ times bigger, and $\displaystyle \frac{\displaystyle 1 }{\displaystyle n}$, which is the optimal split period, will also be $7$ times smaller, but don't forget that the base unit of time is changed from day to week, so the time that actually passes is unchanged. which means that the solution of this formula will not change the time interval of the optimal deposit cycle just because we change the base unit of time. This is very reasonable, because the base unit of time is set by human, and we should not get different results just because we use a different system of unit.

For the same reason, if we change the base unit of the funds, for example, if we change the base unit from dollars to cents, the result will be the same. First of all, $\beta$ is the ratio, let's say it's $0.9$, which means that each time we re-deposit, 90% of the funds will have remained, and when $\beta$ is determined $A$ is also determined, let's substitute $\beta = 0.9$ to arrive at the corresponding $A$ to be $0.644$. Since $A$ is the ratio of accumulated interest income to the initial capital, if the initial capital is $1$ dollar, the time to re-deposit is when the accumulated interest reaches $0.644$ dollars. If we use cents as the base unit instead, since $1$ dollar is $100$ cents and $A$ is $0.644$, the time to re-deposit is when the accumulated interest reaches $64.4$ cents. And $0.644$ dollars is equal to $64.4$ cents, which means that no matter how we change the base unit of funds, it won't affect the final result. This is similar to the previous example of the base unit of time, where the base unit of valuation is set by humans, and it is not reasonable to expect that changing the base unit of valuation will cause a different result.

同樣的道理，如果我們改變資金的單位，比如我們把單位從美元改成美分，得出的結果也是一樣的。因為首先 $\beta$ 是比例假如說是 $0.9$ 好了，代表每次再存入資金會剩下百分之九十，又由於 $\beta$ 決定時 $A$ 也決定了，我們把 $\beta = 0.9$ 代入得出對應的 $A$ 為 $0.644$。因為 $A$ 是累積的利息收入與初始資金的比例，如果初始資金是 $1$ 美元，則累積的利息達到 $0.644$ 美元就是再存入的時機。如果我們改用美分計價，由於 $1$ 美元是 $100$ 美分 $A$ 為 $0.644$ 所以累積利息要達到 $64.4$ 美分，才是我們再存入的時機。而 $0.644$ 美元與 $64.4$ 美分相等，也就是說不管我們怎麼改變計價的單位，也不會影響最後的結果。因為這跟前面時間單位的例子相似，計價的單位是人為設定的，沒道理我們改變計價單位就造成結果不同。

Let's go back and look at why we need to make the fee $F$ proportional to the total funds $V_f$, because that's how we can make $\beta$ a constant, so that $\beta$ is independent of $V_f$, and it follows that $\beta$ is independent of $n$. In this way, when we differentiate with respect to $n$, we can treat $\beta$ as a constant and get the answer we want. But is this a reasonable assumption? We have to check the properties of our solution. It must not change the result even if we change the base units of time and money, because the base units of time and money are set by human, and there is no reason why changing the base units would cause the result to be different, which we have just shown is the case. Moreover, we started with the assumption that the time interval of the splits is uniform, so we still have to prove that when the number of splits $n$ is fixed, an uniform split maximises the revenue, which will be done later in the following derivation. If our solution satisfies all of the conditions described above, we can at least say that it is the solution that leads to the fastest rate of growth under our assumption that the fee is proportional to the total amount of funds.

You can see the [Desmos page](https://www.desmos.com/calculator/9imklswzy5?lang=zh-TW) I created. The $x$ axis represents $n$, the purple line here represents the growth rate, the blue line represents the derivative of growth rate with respect to $n$, and the orange dotted line is the solution to equation $(3)$. We can see that the orange dotted line is a vertical line, which means that the value of $x$ of the orange line is the solution to Equation $(3)$. We can see that the blue line (which is the derivative of the growth rate with respect to $x$) has a root whose $x$ value is exactly the $x$ value of the orange dashed line. The purple line also happens to be the highest point, not the lowest. In other words, we have verified numerically that the equation $(3)$ that we derived earlier is indeed the formula that maximises the growth rate. You can change different $\beta$ and $a$ to verify this, and the relationship we just mentioned will hold for different $\beta$ and $a$.

可以看我建立的[Desmos頁面](https://www.desmos.com/calculator/9imklswzy5?lang=zh-TW)，其中的 $x$ 軸代表 $n$，這裡紫色的線代表成長率，藍色的線代表成長率對 $n$ 微分，而橘色的虛線就是方程式 $(3)$ 的解。我們可以看到橘色虛線是一條直線，也就是橘色線的 $x$ 的數值，就是公式 $(3)$ 的解。我們會發現藍色的線（也就是成長率對的 $x$ 微分）它的根的 $x$ 值正好是橘色虛線的 $x$ 值。再看此時的紫色線也正好是最高點而不是最低點。也就是說我們用數值的方法驗證了，我們前面求出的方程式 $(3)$，的確就是使成長率最大的公式。你可以改變不同的 $\beta$ 跟 $a$ 來驗證，剛剛說的關係在不同的 $\beta$ 跟 $a$ 下都會成立。

## 4. 使成長率最大的分割間隔 (The best split interval to maximise growth rate)

However, we have assumed that the time interval between each re-deposit is uniform. If the interval is not uniform, is it possible to get a higher growth rate? To find out how to maximise the revenue, let's look at the previous equation $(1)$. We know that after a unit of time the funds become $\displaystyle\left [ \beta \left ( 1+\frac{a }{\displaystyle n} \right ) \right ] ^{\displaystyle n}$, and we can separate it to get:

但是我們前面是假設，每次再存入的時間間隔是平均的，如果間隔不平均，有沒有可能使成長率更高？想要知道怎樣的間隔可以讓收益最大化，我們看前面的式子 $(1)$。我們知道過了單位時間後資金變為 $\displaystyle\left [ \beta \left ( 1+\frac{a }{\displaystyle n} \right ) \right ] ^{\displaystyle n}$，我們可以拆解它，得到：

$$
\left [ \beta \left ( 1+\frac{a }{\displaystyle n} \right ) \right ] ^{\displaystyle n} = \beta^{\displaystyle n} \left ( 1+\frac{a }{\displaystyle n} \right )^{\displaystyle n} \tag{6}
$$

We can see that the growth rate per unit of time has two parts, the first part is $\beta^{\displaystyle n}$ and the second part is $\displaystyle\left ( 1+\frac{a }{\displaystyle n} \right )^{\displaystyle n}$. Let's look at the latter part of $\displaystyle\left ( 1+\frac{a }{\displaystyle n} \right )^{\displaystyle n}$. We will find that this is actually $\displaystyle\left ( 1+\frac{a }{\displaystyle n} \right )$ multiplied by $n$ times, but what if $n$ is not a positive integer? We have already shown that the base unit of time is set by human and does not affect the result, so if $n$ is not a positive integer, we just need to find an appropriate base unit of time which makes $n$ an integer. So here we just assume that $n$ is a positive integer, which does not affect the result. Continuing with the derivation $\displaystyle\left ( 1+\frac{a }{\displaystyle n} \right )$ is the magnification of the total funds compared to the initial funds after each time of deposit and before withdrawals. A unit of time is split $n$ times, so the length of each time is $\displaystyle \frac{\displaystyle 1 }{\displaystyle n}$, and we let $\displaystyle \frac{\displaystyle 1 }{\displaystyle n}$ be a new physical quantity called $x$. Substituting $x$ into $\displaystyle\left ( 1+\frac{a }{\displaystyle n} \right )$ and it becomes $\displaystyle\left ( 1 + x a \right )$. Now if the time interval between splits is not uniform, we let the time interval for the $1$th deposit cycle be $x_1$, the $2$th interval be $x_2$, and so on up to the $n$th time be $x_n$. When the time interval is not uniform $\displaystyle\left ( 1+\frac{a }{\displaystyle n} \right )^{\displaystyle n}$ can be written as $\displaystyle\left ( 1 + x_1 a \right )\left ( 1 + x_2 a \right ) \cdots \left ( 1 + x_n a \right )$, and we substitute it into the growth rate formula $(6)$ above and the growth rate can be written as:

我們可以看到，單位時間的成長率由兩個部分構成，前半部分是 $\beta^{\displaystyle n}$ 後面是 $\displaystyle\left ( 1+\frac{a }{\displaystyle n} \right )^{\displaystyle n}$。我們看後面的 $\displaystyle\left ( 1+\frac{a }{\displaystyle n} \right )^{\displaystyle n}$ 這個部分，我們會發現這其實就是 $\displaystyle\left ( 1+\frac{a }{\displaystyle n} \right )$ 乘以 $n$ 次，那如果 $n$ 不是正整數怎麼辦？我們前面已經論證時間的單位是人為設定的，不會影響結果，所以如果 $n$ 不是正整數，我們只需要找一個恰當的時間單位，使得 $n$ 是整數。所以這裡我們只要假設 $n$ 是正整數就好，不會影響結果。繼續往下推導 $\displaystyle\left ( 1+\frac{a }{\displaystyle n} \right )$ 就是每次存入後到提領出來前總資金對比初始資金的倍率，單位時間分割 $n$ 次，所以每次的時間長度就是 $\displaystyle \frac{\displaystyle 1 }{\displaystyle n}$，我們令 $\displaystyle \frac{\displaystyle 1 }{\displaystyle n}$ 為一個新的物理量，稱它為 $x$。把 $x$ 代入 $\displaystyle\left ( 1+\frac{a }{\displaystyle n} \right )$ 就變為 $\displaystyle\left ( 1 + x a \right )$。現在如果分割間隔的時間不是平均的，我們令第 $1$ 次再存入的時間間隔是 $x_1$，第 $2$ 次的間隔是 $x_2$，依此類推到第 $n$ 次是 $x_n$。在時間分割不平均的情況下 $\displaystyle\left ( 1+\frac{a }{\displaystyle n} \right )^{\displaystyle n}$ 就可以寫為 $\displaystyle\left ( 1 + x_1 a \right )\left ( 1 + x_2 a \right ) \cdots \left ( 1 + x_n a \right )$，當時間間隔不平均時我們把它代入上面的成長率公式 $(6)$，成長率就可以寫為：

$$
\displaystyle\beta^{\displaystyle n}\left ( 1 + x_1 a \right )\left ( 1 + x_2 a \right ) \cdots \left ( 1 + x_n a \right ) \tag{7}
$$

我們可以改用 $\displaystyle\left ( 1 + x_1 a \right )\left ( 1 + x_2 a \right ) \cdots \left ( 1 + x_n a \right )$ 的**幾何平均數(Geometric mean)**[^2]來取代它，我們令幾何平均數為 $G$，因為 $G^{\displaystyle n} = \displaystyle\left ( 1 + x_1 a \right )\left ( 1 + x_2 a \right ) \cdots \left ( 1 + x_n a \right )$。把 $G^{\displaystyle n}$ 代入上面的式子 $(7)$ 我們可以把成長率改寫為：

$$
\displaystyle\beta^{\displaystyle n} G^{\displaystyle n} \tag{8}
$$

我們會發現考慮手續費後的成長率，正好是古典分割 $n$ 次的複利成長公式乘上 $\beta^{\displaystyle n}$。但是當分割次數是固定的情況下，我們會發現不論分割間隔如何改變，都不會影響到分割的總次數 $n$，所以 $\beta^{\displaystyle n}$ 不會改變。如果我們希望成長率是極大值，由於 $n$ 是定值不會改變，所以只要 $G$ 越大，成長率也就越高。也就是說使 $G$ 最大的時間間隔分布，就是使成長率最大的時間間隔分布。我們看看**算術平均數(Arithmetic mean)**[^3] $\displaystyle\left ( 1 + x_1 a \right )\left ( 1 + x_2 a \right ) \cdots \left ( 1 + x_n a \right )$ 的算術平均數是 $\displaystyle\frac{n + \left ( x_1 + x_2 + \cdots + x_n \right ) a}{n}$，但是 $\left ( x_1 + x_2 + \cdots + x_n \right )$ 是 $1$，因為我們是把單位時間分割 $n$ 份，所以不管如何分割，這些時間間隔加起來，就是一開始的總時間也就是單位時間也就是 $1$，所以它們加起來一定是 $1$，我們把它化簡後得到 $\displaystyle\left ( 1 + \frac{a}{n} \right )$，我們會發現不論分割間隔為何，由於 $n$ 是固定的，所以算術平均數總是一樣的。我們知道**算幾不等式**[^4]，算術平均數總是大於等於幾何平均數，只有數列中每一個數相等時它們才會相等，也就是說當 $x_1 = x_2 = \cdots = x_n$ 時 $\displaystyle G = \left ( 1 + \frac{a}{n} \right )$，此時 $G$ 大於等於其他所有可能的幾何平均數，所以當數列中每一個數都相等時的 $G$，就是 $G$ 最大的可能。所以我們證明使 $G$ 最大的時間間隔分布為平均分布，此時 $\displaystyle G = \left ( 1 + \frac{a}{n} \right )$，代入前面的公式 $(8)$，此時成長率寫為：

$$
\beta^{\displaystyle n} \left ( 1+\frac{a }{\displaystyle n} \right )^{\displaystyle n}
$$

我們會發現又回到我們最一開始的公式，所以我們證明了當分割次數 $n$ 固定時，使成長率最高的分割是平均分割。前面又證明了當時間間隔平均分割時，使成長率最高的公式，可以用它求出使成長率最高的 $n$。可以得出如果讓時間間隔與分割次數都可以自由改變，我們前面求的公式依然是讓成長率最高的公式，因為我們的公式可以同時滿足這兩個條件。

## 5. 等效利率 (Equivalent interest rate)

現在我們了解到了，當交易有摩擦時，我們並不能像傳統的複利一樣，無限短的提領再存入，而是必須在利息收入到達一定量的時候再操作，才能到達最佳的成長效果。也就是說這種交易的摩擦會造成我們資金利用能力的下降，也就是我們實際上收到的利率會小於交易無摩擦時的利率。我們想知道這樣的損失會造成多大的影響。

回顧古典複利，我們知道：

$$
\lim_{n \to \infty} \left ( \displaystyle 1 + \displaystyle \frac{a}{\displaystyle n}  \right )^{\displaystyle n} = e^{\displaystyle a}
$$

意思是說經過單位時間，資金會成長為原來的 $e^{\displaystyle a}$ 倍，其中 $a$ 我們稱為**對數報酬率(logarithmic rate of return)**[^5]。現在我們想知道當當交易有摩擦時，如果我們嚴格依照最佳循環週期來操作，則在利率不變維持 $a$ 的情況下，我們等效的對數報酬變為多少。

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

我們引入一個新的參數，我們令 $\displaystyle\gamma = \frac{b}{a}$，然後將上面的式子化簡後得到：

$$
\gamma = \frac{1}{(1 + A)}
$$

我們會發現因為 $A$ 一定大於 $0$，所以 $\gamma$ 一定小於 $1$。並且 $\gamma$ 跟 $a$ 無關只跟 $\beta$（或 $A$）有關。也就是說摩擦力會造成我們實際的收益的下降，並且對數報酬率的下降，正好等於與原始資金與加入利息收益的總資金的比值。

並且移項後我們可以得到 $\displaystyle(1 + A) = \frac{1}{\gamma }$，與 $\displaystyle A = \frac{1}{\gamma } - 1$，將它們代入上面的方程式 $(5)$ 化簡後得到：

$$
\displaystyle\beta = \gamma e^{\displaystyle1 - \gamma } \tag{9}
$$

## 6. 擴展到手續費固定時的狀況 (Extended to the situation where the handling fee is fixed)

但是在交易上更常見的狀況是，鍊上交易收取的是一個不看資金大小的固定手續費，而不是百分比的手續費，因此我們要改寫我們的公式。在這裡我們直接假設前面推導出的公式（假設手續費與資金量成正比），可以套用到手續費固定時的情況，下面會討論這種假設是否合理。我們回來看我們之前得到的式子 $(4)$：

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
F = V_i + I - V_i \cdot e^{\displaystyle \frac{I}{V_i + I}} \tag{10}
$$

當 $V_i$ 跟 $F$ 確定時，只要求這個方程式的根就可以得到 $I$。意思是當手續費固定時，當我們累積的利息收入達到 $I$，就是要進行提領再存入操作的時機。

但是我們直接套用之前的公式是合理的嗎？因為之前的公式是假設手續費與資金量成正比，而現在是讓手續費固定。如果 $V_i$ 遠大於 $F$，則我們可以認為這種近似是合理的。舉一個實際的例子，假設初始資金 $V_i$ 是 $100$ 美元，手續費 $F$ 是 $1$ 美元，代入上面的式子 $(10)$，得到此時 $I$ 等於 $14.5$ 美元，我們可以算出此時 $\beta$ 是 $0.991$，如果用這個 $\beta$ 計算初始資金 $V_i$ 的手續費，我們會得到手續費為 $0.87$ 美元，我們會發現 $0.87 \approx 1$，所以此時我們可以近似的認為手續費不會變動。如果說在區塊鍊上，費用可能會在每個 gas $12$ 到 $20$ Gwei 之間波動，並且合約執行後實際的 gas used 也只能估算，在合約執行前也無法知道精確的數量。意思是說手續費的波動，已經大於將手續費假設為與資金量成正比造成的誤差。由於本來在鍊上的交易就有隨機性，所以你本來就不可能找到實際上的最佳解，因為你無法預測下一個區塊的手續費是多少，你只能預估，所以這樣的近似是合理的。

一般來說會直接在鍊上做交易的人，都是資金量遠大於手續費才會直接在鍊上做交易，這樣才會比較划算。如果是資金量小的人，他們會在交易所使用中心化的服務。如果資金只有 $100$ 美元，可是每次在鍊上交易要 $20$ 美元，顯然他會去中心化的交易所，接受交易所大約千分之五的交易手續費。可是你看如果資金有 $10,000$ 美元，千分之五的手續費是 $50$ 美元，顯然他會直接在鍊上做交易。

也就是說，如果你硬要計算手續費 $F$ 是固定時的最佳 $I$，首先這個本身很難計算，其次也是沒有意義的。因為會直接在鍊上做交易的，本身就是資金量遠大於手續費的人。而鍊上交易手續費的波動，其實影響遠大於假設手續費與資金量成正比時，用初始資金與總資金所計算出的手續費差異。

## 7. 計算 (Calculations)

現在我們來看實際的情況，假設我們在恆定乘積做市商，我們提供 $500$ 個 USDT 與 $500$ 個 USDC 的流動性。所以目前流動池的價值是 $1000$ 美元。這邊為了簡單起見，我們假設我們提供的資產它們之間交換的價格比值（在這裡永遠是 $1$ 個 USDT 換 $1$ 個 USDC）與整個流動池的總市值都不會變動。也就是 USDT 的數量永遠是 $500$ 個市值也是 $500$ 美元，USDC 也同理永遠是 $500$ 個共 $500$ 美元。既然它們對美元的市值不會變動，當然它們之間交換的價格也不會變動。並且收到的利息也都是穩定幣，所以收到的利息也不會因為幣價的波動而波動，而是近似於存款的利息，數量只會變多。所以我們可以把這樣的流動池對，認為它近似於我們一開始提到的有摩擦力的存款模型，這個流動池的市值就是前面說的初始資金 $V_i$，在現在的情況下就是 $1000$ 美元。

假設當我們要把賺到的利息再存入，假設要經過三個步驟，首先把利息提領出來，由於我們收到的手續費中 USDT 與 USDC 的比值，不一定跟我們的流動池中兩種資產的比值是相同的比例。所以我們還要交易把它們換成相同比例，最後再存入。總之不管要經過多少步驟，我們把所有會產生的成本都加起來，就是所需的手續費 $F$，假設在目前的狀況中所需的手續費（通常是該鍊的幣）換算成美金要 $20$ 美元。

我們代入上面的公式計算得到 $I$ 等於 $207$ 美元。可以看我建立的[Desmos頁面](https://www.desmos.com/calculator/aeull6siit)，我們把 $F$ 跟 $V_i$ 設定成我們要的數值，然後看兩個方程式的交點，就是我們要求的 $I$。

或是移項後求這個方程式的根等於 $0$。

$$
V_i + I - V_i \cdot e^{\displaystyle \frac{I}{V_i + I}} - F = 0
$$

這是可以計算的，可以看我建立的[Desmos頁面](https://www.desmos.com/calculator/trcdgomtyn)，這裡橘色的線，就是我們的方程式，這個方程的根，就是我們方程式的解。你可以把 $F$ 跟 $V_i$ 設定成你需要的數值來幫你預測 $I$。

接著看另一個[Desmos頁面](https://www.desmos.com/calculator/dqropidfjb)，這裡 $x$ 軸是我們的 $F$，而 $y$ 軸是我們的 $I$。我們可以利用這個頁面觀察 $F$ 與 $I$ 之間的關係，我們會發現 $I$ 始終大於 $F$，這很合理如果收到的利息小於手續費，則我們的錢就會越換越少。而當 $F$ 趨近 $0$ 於時 $I$ 也趨近於 $0$，這代表回到古典沒有手續費的情況。

接著看 $\gamma$ 與 $\beta$ 的關係，當 $\beta$ 等於 $0.8$，代入之前的公式 $(9)$：

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

在古典複利中由於沒有手續費，所以我們再存入操作的時間間隔要無限小，才能使收益最大。但是如果考慮了手續費則時間間隔不能過小，也不能過大。我們提出了一個考量手續費的複利模型，藉由假設手續費與資金量呈正比，我們得出了想要最佳化收益的解。我們發現這個解是收益達到原始資金的一個比例時，要進行再存入的操作，與一開始與中間過程中的利率無關。並且證明它在改變時間單位與資金單位的情況下，也不會改變結果，這也側面驗證了我們假設的合理性。我們還討論了分割間隔對成長率的影響，最後發現當分割間隔是平均時，成長率最大。並且計算了手續費對利率的影響，得出了等效利率。由於在區塊鍊上的世界中，往往是收取固定手續費，所以我們拓展我們的模型到固定手續費的情況，並且討論了這種近似是否合理，並提供了網頁方便大家做計算。

-----

## 參考文獻 (References)


[^1]: **维基百科, s.v. "指数函数"** Wikipedia contributors (2023-06-15) <https://zh.wikipedia.org/w/index.php?title=%E6%8C%87%E6%95%B0%E5%87%BD%E6%95%B0&oldid=77692656>
[^2]: **维基百科, s.v. "几何平均数"** Wikipedia contributors (2023-04-14) <https://zh.wikipedia.org/w/index.php?title=%E5%87%A0%E4%BD%95%E5%B9%B3%E5%9D%87%E6%95%B0&oldid=76812687>
[^3]: **维基百科, s.v. "算术平均数"** Wikipedia contributors (2022-02-05) <https://zh.wikipedia.org/w/index.php?title=%E7%AE%97%E6%9C%AF%E5%B9%B3%E5%9D%87%E6%95%B0&oldid=70020320>
[^4]: **维基百科, s.v. "算术-几何平均值不等式"** Wikipedia contributors (2023-02-01) <https://zh.wikipedia.org/w/index.php?title=%E7%AE%97%E6%9C%AF-%E5%87%A0%E4%BD%95%E5%B9%B3%E5%9D%87%E5%80%BC%E4%B8%8D%E7%AD%89%E5%BC%8F&oldid=75780402>
[^5]: **Wikipedia, s.v. "Rate of return"** Wikipedia contributors (2023-08-06) <https://en.wikipedia.org/w/index.php?title=Rate_of_return&oldid=1169071015#Logarithmic_or_continuously_compounded_return>
