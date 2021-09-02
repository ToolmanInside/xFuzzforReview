# xFuzz

![logo](./logo.png)

Cross-contract vulnerabilities are vulnerabilities where more than two contracts are involved by external calls. Cross-contract call itself is generally not vulnerable. However, it is prone to being leveraged by existing vulnerabilities and deceive current vulnerability detection technologies. For example, a cross-contract vulnerability is missed by existing detection tools is shown in the following code.

```javascript
	contract Trader {
		TokenSale tokenSale = new TokenSale(); // internal contract
	
		function combination() {
			tokenSale.buyTokensWithWei();
			tokenSale.buyTokens(beneficiary);
		}
	}
	
	contract TokenSale {
		TokenOnSale tokenOnSale; // external contract
	
		function set(address _add) {
			tokenOnSale = TokenOnSale(_add);
		}
	
		function buyTokens(address beneficiary) {
			if (starAllocationToTokenSale > 0) {
				tokenOnSale.mint(beneficiary, tokens);
			}
		}
	
		function buyTokensWithWei() onlyTrader {
			wallet.transfer(weiAmount);
		}
	}
```

[test](./test.md)