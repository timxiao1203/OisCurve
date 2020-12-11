# Ois Curve

Overview

	Overnight index swaps (OIS) curves became the market standard for discounting collateralized cashflows. The reason often given for using the OIS rate as the discount rate is that it is derived from the fed funds rate and the fed funds rate is the interest rate usually paid on collateral. As such the fed funds rate and OIS rate are the relevant funding rates for collateralized transactions.
	Many banks now consider that overnight indexed swap (OIS) rates should be used for discounting when collateralized portfolios are valued and that LIBOR should be used for discounting when portfolios are not collateralized.

	Summary

	OIS Discounting  Introduction
	OISCurve Construction and Bootstrapping Overview
	Interpolation
	Optimization

OIS Discounting Introduction
	In the past, a classic yield curve, such as 3 month LIBOR was used for both discounting cashflows and projecting forward rates. 
	However, this classic viewpoint is too simplistic. It does not take into account the relative credit risk between lending money forward over a short period of time at interbank rates, versus the risk involved in short-term funding of those loans.
	Prior to the 2007 financial crisis, market practitioners considered the LIBOR curve as a proxy for the risk-free curve and used it for discounting cashflows.
	During the financial crisis, the spread between three month US LIBOR and three month US treasury rate—increased dramatically. As a result, the LIBOR curve can not be regarded as risk free anymore.
	Market practitioners started to use a new valuation methodology referred to as dual curve discounting, overnight index swaps (OIS) discounting or CSA discounting. 
	OIS curves became the market standard for discounting collateralized cashflows. This curve represents the market expectations of the Federal Reserve daily target for the overnight lending rate. 
	The reason often given for using the OIS rate as the discount rate is that it is derived from the fed funds rate and the fed funds rate is the interest rate usually paid on collateral. As such the fed funds rate and OIS rate are the relevant funding rates for collateralized transactions.
	Many banks now consider that OIS rates should be used for discounting when collateralized portfolios are valued and that LIBOR should be used for discounting when portfolios are not collateralized.

OIS Curve Construction Overview

	The most liquid instruments that can be used to build OIS curve are Fed Fund Futures and OIS swaps that pay at the daily compounded Fed Fund rate.
	However, Fed Fund Futures are currently only liquid up to two years and OIS swaps up to ten years.
	Beyond ten years, the most liquid instruments are Fed Fund versus 3M LIBOR basis swaps, which are liquid up to thirty years. 
	The problem is that to price these basis swaps one needs both the OIS curve, to project the Fed Fund rate, and the LIBOR curve, to project the LIBOR rate. 
	In the past one could have generated the Libor curve separately, by using the single curve for both forward projection and discounting. 
	However, in the modern market Libor swaps are quoted using OIS discounting. This means that in order to generate a forward Libor curve from Libor swap quotes one must first have the OIS curve already constructed so that one knows how to discount the cashflows. So neither the OIS curve nor the Libor curve can be built without the other. The two curves must be generated simultaneously.
	The central tenet of curve construction under OIS discounting is to bootstrap multiple curves simultaneously.
	One needs two term structure inputs: a term structure of OIS instruments and a term structure of LIBOR/swaps.
	This method proceeds as follows:
•	From the underlying instruments, determine which define a point on the OIS curve and which define a point on the Libor curve.
•	All missing values have to be interpolated using linear interpolation on rates.
•	Create these two sets of unknown curve points and make an initial guess for their values. 
•	Price all of the given instruments using the initial guess of the two curves. 
•	Compare the prices with the market quotes and adjust the initial guess accordingly. This would be performed by the same numerical optimization routine as is currently used in FinPricing, the Levenberg-Marquardt algorithm. The only difference is the greater number of points that will be perturbed in the algorithm. 
•	Repeat the pricing and adjustment until the error reaches acceptable levels.

Interpolation

	Most popular interpolation algorithms in curve bootstrapping are linear, log-linear and cubic spline.
	The selected interpolation rule can be applied to either zero rates or discount factors.
	Some critics argue that some of these simple interpolations cannot generate smooth forward rates and the others may be able to produce smooth forward rates but fail to match the market quotes.		
	Also they cannot guarantee the continuity and positivity of forward rates.
	The monotone convex interpolation is more rigorous. It meets the following essential criteria:
•	Replicate the quotes of all input underlying instruments.
•	Guarantee the positivity of the implied forward rates
•	Produce smooth forward curves.
	Although the monotone convex interpolation rule sounds almost perfectly, it is not very popular with practitioners.

Optimization

	As described above, the bootstrapping process needs to solve a yield using a root finding algorithm. 
	In other words, it needs an optimization solution to match the prices of curve-generated instruments to their market quotes.
	FinPricing employs the Levenberg-Marquardt algorithm for root finding, which is very common in curve construction.
	Another popular algorithm is the Excel Solver, especially in Excel application.



You can find more details at
https://finpricing.com/lib/IrCurveIntroduction.html

