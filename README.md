
***Asian Option Pricing Model****



 ## Goal
	# Model Assumptions.
        # Inputs & Parameters (asian_price_mc)
	# How to Run (example)

1. Model Assumptions : 
             $ Risk neutral GBM with constant parameters: dS/D = (r-q)*dt + σ *dW
             $ Discrete and equally spaced fixings: N points over (0,T], arithmetic average A used in payoff
             $ Payoffs : 
                     -> Fixed-strike: calls (A-K), puts (K-A)
                     -> Floating-strike: calls (S_T - A)
             $ Variance reduction: anithetic variates (with defaults on) and optional geometric-average control variate using anlystic E
             $ Season handling: observed fixing are treated as constants
 	     $ Rates and dividend/carry are continuously compounded.

2. Inputs & Parameters (asian_price_mc)
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
expected output: 	
Returns a dict with: price,  stderr,  ci95 [lo, hi],  diagnostics {antithetic, geometric_cv, cv_rho}.

3. How to Run (example)
        # Open python notebook in Google colab:
	run the code by calling as shown below  : 
            
	res = asian_price_mc(
    		 S0=100, K=100, r=0.03, q=0.01, sigma=0.22,
  		  T=1.0, N=12, call_put="call", strike_type="fixed",
   		 n_paths=200_000, seed=123, antithetic=True,
    		observed_fixings=None, use_geometric_cv=True)
       
      print(res["price"], res["stderr"], res["ci95"])

4. Known Limitation:
	1. Equal spacing onlym no business-day calendars or holidays.
	2. Constant r, q, σ and no smiles/surfaces or term structures.
	3. GBM  only and no local/stochastic vol, jumps, or discrete dividends.
	4. Geometric CV (control variate) are just estimates but is not a substitute for full analytics.
   

	
        
        
	
 	

        
	





    
