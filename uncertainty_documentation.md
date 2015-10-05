# Calculation of Uncertainties in Hiperpret

The uncertainties in Hiperpret are calculated from Student's T distribution.

The function get_mean_and_unc in hiperpret takes a list (array) as input and returns a mean and an uncertainty.

It calculates the following:
1. number of measurements (N)
2. sample mean (sm)
3. sample variance (sv = sum_i((X_i-<X>)^2)/(N-1))
4. estimated standard error (ese = sv/sqrt(N))
5. confidence interval (i = t.interval(confidence_level, N-1, sm, ese))[1]
6. uncertainty (unc = (i[1] - i[0])/2)

Since the population (read: actual) standard error is unknown, it must be estimated and this estimate will have an uncertainty. When such an uncertainty exists, the correct distribution to use to estimate the average and its uncertainty is Student's T distribution which is a broader distribution than the Gaussian distribution (for a Gaussian distribution where the variance is set to the sample variance).

The function get_mean_and_unc calculates the mean and the uncertainty for a given confidence level. This confidence level is defined as a variable in the top of Hiperpret and it is called "CONFIDENCE_LEVEL". The reported uncertainty needs to be subtracted and added to the mean in order to get the confidence interval. So given mean and unc, the confidence interval is: [mean - unc, mean + unc].

The measurements are made with a precision of 1 ms. This means that the uncertainty of one measurement can never go below 0.5 ms. The formula for the uncertainty of a sum where each measurement has uncertainty unc_i is: unc_sum = sqrt(sum_i=0^N-1(unc_i^2)), where N is the total number of measurements (which in this case is number of runs for the implementation). If all the unc_i are the same, unc_sum = sqrt(N)*unc. For an average, the uncertainty becomes: unc_av = unc_sum/N = unc/sqrt(N) = 0.5/sqrt(N). Since the uncertainty of the average can never go below this number, the get_mean_and_unc sets the estimated variance to this number if any sample variance below this number is observed.

The results of the confidence interval calculated by using Student's T distribution have been compared with the online calculator found at:
http://www.danielsoper.com/statcalc3/calc.aspx?id=96
The Python library function is documented at:
[1] http://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.t.html