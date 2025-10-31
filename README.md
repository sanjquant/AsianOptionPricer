
*** Asian Options - Simple Monte Carlo Price ***



 ## Goal
			1. Model Assumptions for Asian Option Pricing using Monte Carlo model.
			2. Input parameters used in the model and the expected output
			3. How to Run (example)
			4. known limitations in the model.

1. Model Assumptions for Asian Option Pricing using Monte Carlo model: 
			a. Risk neutral GBM with constant parameters: dS/D = (r-q)*dt + σ *dW
			b. Discrete and equally spaced fixings: N points over (0,T], arithmetic average A used in payoff
			c. Payoffs : 
					 -> Fixed-strike: calls (A-K), puts (K-A)
					 -> Floating-strike: calls (S_T - A)
			d. Variance reduction: anithetic variates (with defaults on) and optional geometric-average control variate using anlystic E
			e. Season handling: observed fixing are treated as constants
			f. Rates and dividend/carry are continuously compounded.

2. Input parameters used in the model and the expected output:
			**Input**
			a. S0: spot price
			b. K: strike (None for floating-strike)
			c. r: risk-free rate (continuous compounding)
			d. q: dividend/carry yield (continuous)
			e. sigma: volatility (constant)
			f. T: time to maturity (years)
			g. N: number of equally spaced fixings (default 12)
			h. option type: "call" or "put"
			i. strike_type: "fixed" or "floating"
			j. n_paths: Monte Carlo paths (default 200,000)
			k. seed: RNG seed
			l. antithetic: True/False (default True)
			m. observed_fixings: a dict of known fixings, either by index {index: price}, or by time {t: price}, where 't' is a year fraction in (0,T].
			n. use_geometric_cv: enable geometric control variate (default True)
			
			**Output value**: dict with  
				price  : estimated PV per unit  
				stderr : Monte Carlo standard error  
				ci95   : 95% confidence interval -> [lo, hi]  
				diagnostics: {antithetic, geometric_cv, cv_rho}

3. How to Run (example)
			# Open python notebook in Google colab:
			Python usage:
			### python
			from asian_option_pricer import asian_price_mc

			# Example 1: Unseasoned fixed-strike call
			resA = asian_price_mc(spot=100, strike=100, r=0.03, q=0.00, sigma=0.20, T=0.5, N=12,option_type="call", strike_type="fixed", paths=200_000, seed=7)
			print(resA)

			#  Example 2: Seasoned floating-strike put)
			obs = [98.2, 99.0, 101.3, 100.7]  # observed fixings
			resB =  asian_price_mc(spot=100, r=0.02, q=0.01, sigma=0.25, T=1.0, N=24, option_type="put", paths=250_000, seed=11, observed_fixings=obs)
			print(resB)

			# Example 3: N=1 sanity vs Black–Scholes (fixed-strike)

			resC =  asian_price_mc(spot=100, strike=100, r=0.01, q=0.00, sigma=0.18, T=1.0, N=1, option_type="call", strike_type="fixed", paths=300_000, seed=99)
			bs = black_scholes_call(100, 100, 0.01, 0.00, 0.18, 1.0)
			print("=== Vanilla Cross-Check ===")
			print(f"MC={resC.price:.6f} vs BS={bs:.6f} (diff={resC.price - bs:.6f})")


	
4. known limitations and constains in the model.
			a. Equal spacing only no business-day calendars or holidays.
			b. Constant r, q, σ and no smiles/surfaces or term structures.
			c. GBM only and no local/stochastic vol, jumps, or discrete dividends.
			d. Geometric CV (control variate) are just estimates but is not a substitute for full analytics.
   

	
        
        
	
 	

        
	





    
