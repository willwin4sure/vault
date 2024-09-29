You've heard this from [[Capital Markets|14.01]]: the way to price money in the future is via their ==expected discounted value==: we take an expectation over uncertainty, but we also need to discount by risk-free interest rate. Bonds from uncertain sources (corporate instead of, say, the US Treasury), have an extra risk premium and are cheaper.

## Stocks in Terms of Dividends

Stocks are different than bonds in two important ways:

* They pay ==dividends== over time to stockholders.
* They do not have a fixed terminal date.

A fundamental evaluation would equate the price of the stock with its sum of expected discounted dividends (in a modern world of meme stocks, the price could easily be much higher, of course).

In nominal terms, we can write this as
$$
\$Q_{t}=\frac{\$ D_{t+1}^{e}}{(1+i_{1t}+x^{s})}+\frac{\$D_{t+2}^{e}}{(1+i_{1t}+x^{s})(1+i_{1t+1}^{e}+x^{s})}+\cdots.
$$
We can also express it in real terms by removing the dollar signs and replacing $i$ with $r$.

## IS-LM with Expectations

The basic IS-LM model is short-sighted, but in reality the future matters! Even if you have no income in the present, you still might consume if you have savings or expect large profits in the future. In particular, consumption is a function of total wealth in addition to disposable income.

Here's a picture of what happens to the IS curve:

![[is_lm_expectations.png|center|384]]

One important feature is that the IS curve is a lot steeper: changes to current income have a lot less of an impact compared to interest rates. This is because you are a lot more driven by long term expected changes for the future. 

---

**Next:** [[Facts of Growth]]


