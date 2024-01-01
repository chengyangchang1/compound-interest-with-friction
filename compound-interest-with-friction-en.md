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

Why do we use $A$ instead of $a$ and $n$? Because interest rates fluctuate on-chain. So if the interest rate fluctuates, the calculated $n$ will also fluctuate, which in turn affects the optimal deposit period we get, and makes the deposit period fluctuate as well. However, we will find that $a$ always appears with $n$ (always divides), so $A$ is only related to $\beta$ and independent of $a$. Therefore, by using $A$, the effect of interest rate fluctuations can be ignored. Given $\beta$, $A$ can be calculated, and as long as the ratio of accumulated interest income to the original deposit reaches $A$, it is time to re-deposit.

We then look at the property of this solution, and we see that this solution gives the same result if we change the base unit of time, for example, if we replace the day with the week as the base unit. We know that $A = \displaystyle \frac{\displaystyle a }{\displaystyle n}$. Since we are splitting $n$ times per unit of time, $\displaystyle \frac{\displaystyle 1 }{\displaystyle n}$ is the optimal splitting period, and we get $\displaystyle \frac{\displaystyle 1}{\displaystyle n} = \frac{A}{a}$. Since $A$ is determined when $\beta$ is determined, $A$ divided by $a$ is the optimal split period. If we use day as the base unit, then $a$ is the interest received in a day, and if we use week as the base unit, then $a$ is the interest received in a week. Of course, if we use week as the base unit, $a$ will become $7$ times bigger, and $\displaystyle \frac{\displaystyle 1 }{\displaystyle n}$, which is the optimal split period, will also be $7$ times smaller, but don't forget that the base unit of time is changed from day to week, so the time that actually passes is unchanged. which means that the solution of this formula will not change the time interval of the optimal deposit cycle just because we change the base unit of time. This is very reasonable, because the base unit of time is set by human, and we should not get different results just because we use a different system of unit.

For the same reason, if we change the base unit of the funds, for example, if we change the base unit from dollars to cents, the result will be the same. First of all, $\beta$ is the ratio, let's say it's $0.9$, which means that each time we re-deposit, 90% of the funds will have remained, and when $\beta$ is determined $A$ is also determined, let's substitute $\beta = 0.9$ to arrive at the corresponding $A$ to be $0.644$. Since $A$ is the ratio of accumulated interest income to the initial capital, if the initial capital is $1$ dollar, the time to re-deposit is when the accumulated interest reaches $0.644$ dollars. If we use cents as the base unit instead, since $1$ dollar is $100$ cents and $A$ is $0.644$, the time to re-deposit is when the accumulated interest reaches $64.4$ cents. And $0.644$ dollars is equal to $64.4$ cents, which means that no matter how we change the base unit of funds, it won't affect the final result. This is similar to the previous example of the base unit of time, where the base unit of valuation is set by humans, and it is not reasonable to expect that changing the base unit of valuation will cause a different result.

Let's go back and look at why we need to make the fee $F$ proportional to the total funds $V_f$, because that's how we can make $\beta$ a constant, so that $\beta$ is independent of $V_f$, and it follows that $\beta$ is independent of $n$. In this way, when we differentiate with respect to $n$, we can treat $\beta$ as a constant and get the answer we want. But is this a reasonable assumption? We have to check the properties of our solution. It must not change the result even if we change the base units of time and money, because the base units of time and money are set by human, and there is no reason why changing the base units would cause the result to be different, which we have just shown is the case. Moreover, we started with the assumption that the time interval of the splits is uniform, so we still have to prove that when the number of splits $n$ is fixed, an uniform split maximises the revenue, which will be done later in the following derivation. If our solution satisfies all of the conditions described above, we can at least say that it is the solution that leads to the fastest rate of growth under our assumption that the fee is proportional to the total amount of funds.

You can see the [Desmos page](https://www.desmos.com/calculator/9imklswzy5?lang=zh-TW) I created. The $x$ axis represents $n$, the purple line here represents the growth rate, the blue line represents the derivative of growth rate with respect to $n$, and the orange dotted line is the solution to equation $(3)$. We can see that the orange dotted line is a vertical line, which means that the $x$ value of the orange line is the solution to Equation $(3)$. We can see that the blue line (which is the derivative of the growth rate with respect to $x$) has a root whose $x$ value is exactly the $x$ value of the orange dashed line. The purple line also happens to be the highest point, not the lowest. In other words, we have verified numerically that the equation $(3)$ that we derived earlier is indeed the formula that maximises the growth rate. You can change different $\beta$ and $a$ to verify this, and the relationship we just mentioned will hold for different $\beta$ and $a$.

## 4. 使成長率最大的分割間隔 (The best split interval to maximise growth rate)

However, we have assumed that the time interval between each re-deposit is uniform. If the interval is not uniform, is it possible to get a higher growth rate? To find out how to maximise the revenue, let's look at the previous equation $(1)$. We know that after a unit of time the funds become $\displaystyle\left [ \beta \left ( 1+\frac{a }{\displaystyle n} \right ) \right ] ^{\displaystyle n}$, and we can separate it to get:

$$
\left [ \beta \left ( 1+\frac{a }{\displaystyle n} \right ) \right ] ^{\displaystyle n} = \beta^{\displaystyle n} \left ( 1+\frac{a }{\displaystyle n} \right )^{\displaystyle n} \tag{6}
$$

We can see that the growth rate per unit of time has two parts, the first part is $\beta^{\displaystyle n}$ and the second part is $\displaystyle\left ( 1+\frac{a }{\displaystyle n} \right )^{\displaystyle n}$. Let's look at the second part $\displaystyle\left ( 1+\frac{a }{\displaystyle n} \right )^{\displaystyle n}$. We will find that this is actually $\displaystyle\left ( 1+\frac{a }{\displaystyle n} \right )$ multiplied by $n$ times, but what if $n$ is not a positive integer? We have already shown that the base unit of time is set by human and does not affect the result, so if $n$ is not a positive integer, we just need to find an appropriate base unit of time which makes $n$ an integer. So here we just assume that $n$ is a positive integer, which does not affect the result. Continuing with the derivation $\displaystyle\left ( 1+\frac{a }{\displaystyle n} \right )$ is the magnification of the total funds compared to the initial funds after each time of deposit and before each withdrawal. A unit of time is split $n$ times, so the length of each time is $\displaystyle \frac{\displaystyle 1 }{\displaystyle n}$, and we let $\displaystyle \frac{\displaystyle 1 }{\displaystyle n}$ be a new physical quantity called $x$. Substituting $x$ into $\displaystyle\left ( 1+\frac{a }{\displaystyle n} \right )$ and it becomes $\displaystyle\left ( 1 + x a \right )$. Now if the time interval between splits is not uniform, we let the time interval of the 1st time deposit cycle be $x_1$, the 2nd time be $x_2$, and so on up to the nth time be $x_n$. When the time interval is not uniform $\displaystyle\left ( 1+\frac{a }{\displaystyle n} \right )^{\displaystyle n}$ can be written as $\displaystyle\left ( 1 + x_1 a \right )\left ( 1 + x_2 a \right ) \cdots \left ( 1 + x_n a \right )$, and we substitute it into the growth rate formula $(6)$ above and the growth rate can be written as:

$$
\displaystyle\beta^{\displaystyle n}\left ( 1 + x_1 a \right )\left ( 1 + x_2 a \right ) \cdots \left ( 1 + x_n a \right ) \tag{7}
$$

Instead of $\displaystyle\left ( 1 + x_1 a \right )\left ( 1 + x_2 a \right ) \cdots \left ( 1 + x_n a \right )$, we can replace it with the **Geometric mean**[^2] of itself, and we let the geometric mean be $G$. because $G^{\displaystyle n} = \displaystyle\left ( 1 + x_1 a \right )\left ( 1 + x_2 a \right ) \cdots \left ( 1 + x_n a \right )$. Substituting $G^{\displaystyle n}$ into the above equation $(7)$, we can rewrite the growth rate as:

$$
\displaystyle\beta^{\displaystyle n} G^{\displaystyle n} \tag{8}
$$

We find that the growth rate after taking the commission into account is exactly the classical compound interest growth formula for $n$ splits multiplied by $\beta^{\displaystyle n}$. However, when the number of splits is fixed, we find that no matter how the time interval between splits changes, it does not affect the total number of splits $n$, so $\beta^{\displaystyle n}$ does not change. If we want to maximise the growth rate, since $n$ is a fixed number that does not change, so the larger $G$ is, the higher the growth rate will be. That is, the time interval to maximise $G$ is the time interval to maximise the growth rate. Let's look at the **Arithmetic mean**[^3]. The arithmetic mean of $\displaystyle\left ( 1 + x_1 a \right )\left ( 1 + x_2 a \right ) \cdots \left ( 1 + x_n a \right )$ is:

我們會發現考慮手續費後的成長率，正好是古典分割 $n$ 次的複利成長公式乘上 $\beta^{\displaystyle n}$。但是當分割次數是固定的情況下，我們會發現不論分割間隔如何改變，都不會影響到分割的總次數 $n$，所以 $\beta^{\displaystyle n}$ 不會改變。如果我們希望成長率是極大值，由於 $n$ 是定值不會改變，所以只要 $G$ 越大，成長率也就越高。也就是說使 $G$ 最大的時間間隔分布，就是使成長率最大的時間間隔分布。我們看看**算術平均數(Arithmetic mean)**[^3] $\displaystyle\left ( 1 + x_1 a \right )\left ( 1 + x_2 a \right ) \cdots \left ( 1 + x_n a \right )$ 的算術平均數是：

$$
\displaystyle\frac{n + \left ( x_1 + x_2 + \cdots + x_n \right ) a}{n} \tag{9}
$$

But $\left ( x_1 + x_2 + \cdots + x_n \right ) = 1$, because a unit of time is split into $n$ parts, so no matter how it is split, the sum of all these time intervals is a unit of time, which is $1$. Substituting it into equation $(9)$ and reducing it to get $\displaystyle\left ( 1 + \frac{a}{n} \right )$, since $n$ is a fixed number, we find that the arithmetic mean is always the same value, no matter how we split it. Let's look at the **AM–GM inequality**[^4], the arithmetic mean is always greater than or equal to the geometric mean, and they are equal only if every number in the series is equal. That is, when $x_1 = x_2 = \cdots = x_n$, $G$ is greater than or equal to all other possible geometric mean, that is, at this point $G$ is maximum. So we show that the time interval distribution that maximises $G$ is the uniform distribution, at this point $\displaystyle G = \left ( 1 + \frac{a}{n} \right )$, and substituting into the previous equation $(8)$, the growth rate at this point is written as:

$$
\beta^{\displaystyle n} \left ( 1+\frac{a }{\displaystyle n} \right )^{\displaystyle n}
$$

We will find that it returns to the formula we started with, so we proved that when the number of splits $n$ is fixed, the split that maximises the growth rate is the uniform split. And earlier, we also derived the formula for $n$ that maximises the growth rate when the time interval between splits is uniform. Therefore, it can be concluded that if the time interval and the number of splits are allowed to change freely, the formula we found earlier is still the formula that maximises the growth rate, because our formula can satisfy both conditions at the same time.

## 5. Equivalent interest rate

We now realise that when there exists trading friction, we can't make the cycle of withdrawals and re-deposits infinitely short, as we do in classical compounding. Instead, we must wait until accumulated interest income reaches a certain amount in order to maximise growth. That is, the friction of the transaction will cause a reduction in our ability to utilise the funds, which means that the interest rate we actually receive will be lower than it would have been when the transaction was frictionless. We would like to know how big the impact of such a loss would be.

Reviewing classical compound interest, we know that:

$$
\lim_{n \to \infty} \left ( \displaystyle 1 + \displaystyle \frac{a}{\displaystyle n}  \right )^{\displaystyle n} = e^{\displaystyle a}
$$

This means that after a unit of time, money grows by $e^{\displaystyle a}$ times compared to the initial, where $a$ is what we call the **logarithmic rate of return**[^5]. Now, if we keep the interest rate fixed at $a$, and we strictly follow the optimal deposit cycle when there is a trading friction, we want to know what our equivalent logarithmic rate of return becomes.

As we already know, if we take into account the commission, after a unit of time the funds will grow by $\displaystyle\left [ \beta \left ( 1+\frac{\displaystyle a }{\displaystyle n} \right ) \right ] ^{\displaystyle n}$ times compared to the initial. And let it be equal to $e^{\displaystyle b}$, where $b$ is the equivalent logarithmic rate of return:

$$
\left [ \beta \left ( 1+\frac{a }{\displaystyle n} \right ) \right ] ^{\displaystyle n} = e^{\displaystyle b}
$$

We take the logarithm of both sides, and reduce it to get:

$$
n\ln{\left [ \beta \left ( 1+\frac{a }{\displaystyle n} \right ) \right ]} = b
$$

We also know that the $n$ that makes the optimal deposit cycle is the solution of $\displaystyle\ln{\left [ \displaystyle \beta \left ( 1+\frac{a }{\displaystyle n} \right ) \right ]} = \frac{1}{\displaystyle \left ( 1+\frac{a }{\displaystyle n} \right )}\frac{a }{\displaystyle n}$. Substituting into the above equation and reduce it to get:

$$
b = \frac{1}{\left ( 1 + A \right ) } a
$$

We introduce a new quantity, we let $\displaystyle\gamma = \frac{b}{a}$, and then we substitute it into the above equation, and reduce it to get:

$$
\gamma = \frac{1}{(1 + A)}
$$

We will find that since $A$ must be greater than $0$, $\gamma$ must be less than $1$. And $\gamma$ is independent of $a$ and only related to $\beta$ (or $A$). That is, friction will cause our actual revenue to decrease, and the decrease in the logarithmic rate of return will be equal to the ratio of the initial funds to the total funds before re-deposit.

And after moving the terms, we get $\displaystyle(1 + A) = \frac{1}{\gamma }$, and $\displaystyle A = \frac{1}{\gamma } - 1$, substituting them into the above equation $(5)$ and reduce it to get:

$$
\displaystyle\beta = \gamma e^{\displaystyle1 - \gamma } \tag{10}
$$

## 6. Extended to the situation where the handling fee is fixed

However, it is more common for on-chain transactions to charge a fixed handling fee that does not depend on the amount of total funds, rather than a percentage of total funds, so we have to rewrite our formula. Here we assume that the formula we derived earlier (assuming that the handling fee is proportional to the amount of total funds) can be applied to the situation when the handling fee is fixed, and we will discuss whether this assumption is reasonable below. Let's come back to the formula $(4)$ we got earlier:

$$
\ln{\left [ \beta \left ( 1 + A \right ) \right ]} = \frac{1}{\left ( 1 + A \right )}A \tag{4}
$$

First, let's look at what $\beta$ is. We know that $\beta$ is the ratio of the remaining funds after re-deposit to the total funds. From the above definition, we know that $\displaystyle\beta = \frac{V_f - F}{V_f} = \frac{V_i + I - F}{V_i + I}$. And $A$ is the ratio of accumulated interest income to initial capital, which can be written as $\displaystyle A = \frac{I}{V_i}$. Then $\left ( 1 + A \right )$ can be written as:

$$
\left ( 1 + A \right ) = 1 + \frac{I}{V_i} = \frac{V_f}{V_i}
$$

We substitute $\beta$ and $A$ into the above equation $(4)$ and reduce it to get:

$$
\begin{align*}
&\ln{\left [ \beta \left ( 1 + A \right ) \right ]} = \frac{1}{\left ( 1 + A \right )}A \\
\Rightarrow \ &\ln{\left [ \frac{V_f - F}{V_f} \left ( \frac{V_f}{V_i} \right ) \right ] } = \frac{V_i}{V_f} \frac{I}{V_i} \\
\Rightarrow \ &\ln{\left ( \frac{V_f - F}{V_i} \right ) } = \frac{I}{V_f} \\
\end{align*}
$$

We substitute $V_f = V_i + I$ into the above equation and get the relationship between $F$ and $I$:

$$
\ln{\left ( \frac{V_i + I - F}{V_i} \right ) } = \frac{I}{V_i + I}
$$

Reduce it to get:

$$
F = V_i + I - V_i \cdot e^{\displaystyle \frac{I}{V_i + I}} \tag{11}
$$

When $V_i$ and $F$ are determined, $I$ can be found by finding the root of this equation. It means that when the handling fee is fixed, when our accumulated interest income reaches $I$, it is time to withdraw and re-deposit.

But is it reasonable for us to apply the previous formula directly? This is because the previous formula assumes that the handling fee is proportional to the amount of funds, whereas now it lets the handling fee be fixed. If $V_i$ is much greater than $F$, then we can consider this approximation reasonable. For example, assuming that the initial funding $V_i$ is $100$ dollars and the handling fee $F$ is $1$ dollar, and substitute them into the above equation $(11)$, we will get that $I$ is equal to $14.5$ dollars, and we can calculate that $\beta$ is $0.991$ at this point. If we use this $\beta$ to calculate the handling fee for the initial funds $V_i$, we will get that the handling fee is $0.87$ dollars, and we will find that $0.87 \approx 1$, so we can think that the handling fee is roughly not change at this point. In the on-chain market, the fee may fluctuate between $12$ and $20$ Gwei per gas, and the actual gas used after the contract is executed can only be estimated, and there is no way to know the exact amount until the contract is executed. This means that the fluctuation of the commission is greater than the error caused by assuming that the commission is proportional to the amount of funds. Since trading on-chain is inherently random, it is impossible to find the actual optimal solution because you cannot predict what the fee will be in the next block, you can only estimate it. So we can think that such an approximation is reasonable when $V_i$ is large enough.

但是我們直接套用之前的公式是合理的嗎？因為之前的公式是假設手續費與資金量成正比，而現在是讓手續費固定。如果 $V_i$ 遠大於 $F$，則我們可以認為這種近似是合理的。舉一個實際的例子，假設初始資金 $V_i$ 是 $100$ 美元，手續費 $F$ 是 $1$ 美元，代入上面的式子 $(11)$，得到此時 $I$ 等於 $14.5$ 美元，我們可以算出此時 $\beta$ 是 $0.991$，如果用這個 $\beta$ 計算初始資金 $V_i$ 的手續費，我們會得到手續費為 $0.87$ 美元，我們會發現 $0.87 \approx 1$，所以此時我們可以近似的認為手續費不會變動。如果說在區塊鍊上，費用可能會在每個 gas $12$ 到 $20$ Gwei 之間波動，並且合約執行後實際的 gas used 也只能估算，在合約執行前也無法知道精確的數量。意思是說手續費的波動，已經大於將手續費假設為與資金量成正比造成的誤差。由於本來在鍊上的交易就有隨機性，所以你本來就不可能找到實際上的最佳解，因為你無法預測下一個區塊的手續費是多少，你只能預估，所以這樣的近似是合理的。

## 7. Calculations

Now let's look at the real situation, assume we are providing $500$ USDT and $500$ USDC for liquidity at a Constant Product Market Maker . So the current value of the liquidity pool is $1000$ dollars. For simplicity, we assume that the ratio of the prices of the assets we provide for exchange between them (always $1$ USDT for $1$ USDC in this case) does not change, as well as the total market value of the entire liquid pool. That is, the number of USDT is always $500$ USDT and the market value is also $500$ dollars, and the same goes for USDC, which is always $500$ USDC and has a total market value of $500$ dollars. Since the market value of them does not change, the exchange price between USDT and USDC does not change either. Moreover, the interest received is in stablecoin, so the interest received will not fluctuate due to currency fluctuations, but is similar to the interest received on deposits, and the amount will only increase. So we can think of such a pool as approximating the frictional deposit model we mentioned at the beginning, and the market value of this pool is the initial capital $V_i$, which in this case is $1000$ dollars.

現在我們來看實際的情況，假設我們在恆定乘積做市商，我們提供 $500$ 個 USDT 與 $500$ 個 USDC 的流動性。所以目前流動池的價值是 $1000$ 美元。這邊為了簡單起見，我們假設我們提供的資產它們之間交換的價格比值（在這裡永遠是 $1$ 個 USDT 換 $1$ 個 USDC）與整個流動池的總市值都不會變動。也就是 USDT 的數量永遠是 $500$ 個市值也是 $500$ 美元，USDC 也同理永遠是 $500$ 個共 $500$ 美元。既然它們對美元的市值不會變動，當然它們之間交換的價格也不會變動。並且收到的利息也都是穩定幣，所以收到的利息也不會因為幣價的波動而波動，而是近似於存款的利息，數量只會變多。所以我們可以把這樣的流動池對，認為它近似於我們一開始提到的有摩擦力的存款模型，這個流動池的市值就是前面說的初始資金 $V_i$，在現在的情況下就是 $1000$ 美元。

Let's assume that when we want to re-deposit the interest earned, we have to go through a three-step process. Firstly, we have to withdraw the interest, and since the ratio of USDT to USDC in the interest we received is not necessarily the same as the ratio of the two assets in our liquid pool. So we have to trade and convert them to the same ratio and then re-deposit them. In summary, no matter how many steps it takes, we add up all of the costs that would be incurred, and that is the required fee of $F$, assuming that the required fee (usually in the currency of the chain) is converted to $20$ dollars in the current situation.

We substitute it into the above equation to get $I$ equal to $207$ dollars. You can look at the [Desmos page](https://www.desmos.com/calculator/aeull6siit) I created, we set $F$ and $V_i$ to the values we want, and then look at the intersection point of these two equations, which is the $I$ we are asking for.

Another way is to find the roots of the following equation after moving the terms.

$$
V_i + I - V_i \cdot e^{\displaystyle \frac{I}{V_i + I}} - F = 0
$$

This can be calculated by looking at the [Desmos page](https://www.desmos.com/calculator/trcdgomtyn) that I created, where the orange line is our equation, and the root of this equation is the solution to our equation. You can set $F$ and $V_i$ to whatever values you need to help you predict $I$.

Moving on to another [Desmos page](https://www.desmos.com/calculator/dqropidfjb), here the $x$ axis is our $F$ and the $y$ axis is our $I$. We can use this page to look at the relationship between $F$ and $I$, and we'll see that $I$ is always greater than $F$, which makes sense because if the interest we received is less than the handling fee, we're getting less and less for our money. As $F$ tends to $0$, $I$ also tends to $0$, which means that we return to the classical situation where there is no handling fee.

We review the relationship between $\gamma$ and $\beta$, and let $\beta$ be equal to $0.8$ at this point, and substitute it into the previous equation $(10)$:

$$
\displaystyle\beta = \gamma e^{\displaystyle1 - \gamma }
$$

We get $\gamma$ is equal to $0.472$. Then we use the same method to find the value of $\gamma$ when $\beta$ is equal to $0.9, 0.99, 0.999$, and we get:

```
   beta=0.9     gamma=0.608
   beta=0.99    gamma=0.865
   beta=0.999   gamma=0.956
```

You can look at the [Desmos page](https://www.desmos.com/calculator/5spwcqh4f7) I created, where the $x$ axis represents $\beta$ and the $y$ axis represents $\gamma$, and look at the relationship between $\beta$ and $\gamma$. We will find that when $\beta$ is very close to $1$, the slope tends to infinity. From the previous calculations, we can also see that as $\beta$ approaches $1$, $\gamma$ increases steeply rather than linearly. You can see that when $\beta$ is $0.9$ the $\gamma$ is $0.608$, and when $\beta$ is $0.99$ the $\gamma$ is $0.865$, even though $\beta$ is very close to $1$, there is still a big difference between $\gamma$ and $1$. That is, when there is friction in the transaction, even if $\beta$ is very close to $1$ and the friction is very small, the impact on the actual interest rate is still huge. Perhaps this result also illustrates why the efficiency of capital utilisation in the traditional financial industry is quite different when the volume of capital is different, e.g. if there is more capital or a larger transaction volume, you usually get better terms such as interest rates, fee discounts, rebates and so on. In addition to considerations of marketing, perhaps it is also due to the consideration of the impact of the costs caused by the operation on the interest rate, which is the subject of discussion in this article.

## 8. Conclusion

In classical compound interest model, since there is no commission, the time interval between re-deposits must be infinitely small in order to maximise revenue. However, if we take the handling fee into account, the time interval cannot be too small or too large. We propose a compound interest model that takes the handling fee into account. By assuming that the handling fee is proportional to the total amount of funds, we get a solution that optimises the revenue. We find that this solution results in the need for us to re-deposit when the revenue reaches a proportion of the initial funds. It is independent of the interest rate at the beginning and in the intermediate process. It is also shown that the result does not change even if the base unit of time and the money are changed. We also discussed the effect of split intervals on the growth rate, and found that the growth rate is maximised when the intervals of split are uniform. We also calculated the impact of commission on the interest rate and got the equivalent interest rate. Since in the on-chain market, a fixed fee is often charged, we extend our model to the fixed fee case and discuss whether this approximation is reasonable, and provide a web page for anyone to do the calculations.

-----

## References


[^1]: **维基百科, s.v. "指数函数"** Wikipedia contributors (2023-06-15) <https://zh.wikipedia.org/w/index.php?title=%E6%8C%87%E6%95%B0%E5%87%BD%E6%95%B0&oldid=77692656>
[^2]: **维基百科, s.v. "几何平均数"** Wikipedia contributors (2023-04-14) <https://zh.wikipedia.org/w/index.php?title=%E5%87%A0%E4%BD%95%E5%B9%B3%E5%9D%87%E6%95%B0&oldid=76812687>
[^3]: **维基百科, s.v. "算术平均数"** Wikipedia contributors (2022-02-05) <https://zh.wikipedia.org/w/index.php?title=%E7%AE%97%E6%9C%AF%E5%B9%B3%E5%9D%87%E6%95%B0&oldid=70020320>
[^4]: **维基百科, s.v. "算术-几何平均值不等式"** Wikipedia contributors (2023-02-01) <https://zh.wikipedia.org/w/index.php?title=%E7%AE%97%E6%9C%AF-%E5%87%A0%E4%BD%95%E5%B9%B3%E5%9D%87%E5%80%BC%E4%B8%8D%E7%AD%89%E5%BC%8F&oldid=75780402>
[^5]: **Wikipedia, s.v. "Rate of return"** Wikipedia contributors (2023-08-06) <https://en.wikipedia.org/w/index.php?title=Rate_of_return&oldid=1169071015#Logarithmic_or_continuously_compounded_return>
