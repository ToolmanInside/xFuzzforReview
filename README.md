# xFuzz

<img src="https://www.imageoss.com/images/2021/09/02/logo921a77fb5a4ded3f.png"  height="300" width="305">

Cross-contract vulnerabilities are vulnerabilities where more than two contracts are involved by external calls. Cross-contract call itself is generally not vulnerable. However, it is prone to being leveraged by existing vulnerabilities and deceive current vulnerability detection technologies. For example, a cross-contract vulnerability is missed by existing detection tools is shown in the following code.

```javascript
 1 contract Trader {
 2     TokenSale tokenSale = new TokenSale(); // internal contract
 3 
 4     function combination() {
 5         tokenSale.buyTokensWithWei();
 6         tokenSale.buyTokens(beneficiary);
 7     }
 8 }
 9 
10 contract TokenSale {
11     TokenOnSale tokenOnSale; // external contract
12 
13     function set(address _add) {
14         tokenOnSale = TokenOnSale(_add);
15     }
16
17     function buyTokens(address beneficiary) {
18         if (starAllocationToTokenSale > 0) {
19             tokenOnSale.mint(beneficiary, tokens);
20         }
21     }
22
23     function buyTokensWithWei() onlyTrader {
24         wallet.transfer(weiAmount);
25     }
26 }
```

The code at **line 6** directs to an external contract *TokenSale* and invokes an uncontroled external call at **line 19**, which can be leveraged to form a cross-contract vulnerability.

These vulnerabilities are firstly reported by [Clairvoyance](https://ieeexplore.ieee.org/document/9270398). However, this tool only supports detecting cross-contract reentrancy vulnerabilities. Considering other overlooked cross-conract threats by compounding cross-contract calls with existing vulnerabilities, we extend their work by supporting three most dangerous cross-contract vulnerabilities.

We present **xFuzz**, a machine learning guided cross-contract fuzzing framework.

We train a machine leaning model to provide suggestions on deciding a function is whether vulnerable or not. To further effectively guide fuzzers, we propose to combine model predictions and static analysis to guide fuzzers in three ways:

* model predictions help locate vulnerable functions
* identify exploitable paths which are related to internal callers
* identify exploitable paths which are related to external callers

In this website, we will keep updating suppliment materials.

[test](./test.md)

[Dataset Open](./dataset/dataset-doc.md)