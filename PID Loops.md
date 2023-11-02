### Proportional Integral Derivative
- Closed Loop Control: Recursive Calling (Update a specific variable between a specified range or equality)
- Specific to [analog signals] : represented by an infinite value of doubles
###### Requirements
- Actuator
- Feedback
###### Variables
1. Setpoint Value: The value in which the actuator should be
2. Process Value: The point where the actuator actually measures
3. Control Value: Value that is placed to the PV to achieve SV
4. PID Loop Coefficients: $K_p K_i K_d$
![[Pasted image 20231019202422.png]]
###### Loop Coefficients
- Proportional Gain (P): Reaction
	- Instantaneous Response to the Error
	$$P=(SP-PV)\cdot K_p$$
- Integral Gain (I): Cumulative Changes
	- Error Over Time: Based on the magnitude of the response of the P term
	- Contributes to dampening the strength of loops response to the continued error
		$$
\int_{o}^{t} (PV-SP)\, dt
$$
	- Gets the area of the values under the curve of PV-SP for a specified value t (time)
- Derivative Gain (D): 
	- Tuning Parameter: Influences the response of the control system to changes in the error signals
	- Higher $K_d$: Stronger Response; prone to increased noise sensitivity
	- Lower $K_d$: Less sensitive to the rate of change; may result in slower response times
	$$D = K_d\frac{e(t)}{e} \text{ where e is the error}$$
	- The derivative $\frac{de(t)}{dt}$ represents the derivative of the error; speed of rate of change for a specified time value
- CV Output: $\sum_2$
	- $CV=P+I+D$ 
#### Mathematics of PID Loops
$$u(t) = K_pe(t)+K_i\int_0^te(t)dt+K_d\frac{de(t)}{dt}$$
- Output = Proportional + Integral + Derivative
- Proportional
	- Error: Setpoint - Process Value
	- Output = $K\times Error$, where K is called the Gain
	- The error is the dependent on the gain
		- Oscillation can occur
		- Tuning the gain is the best approach
	- Offset: Steady-State Error
		- Usage for the Integral portion of the PID Loop
- Proportional + Integral
	$$u(t)=K_p+K_i\int e(t)dt$$
	- Integral
		- Cumulative Error
		- the Integral can also be seen as a summation rather than the usual interpretation of integral
		$$u(t)=K_p\times e(t)+K_i\sum e(t)$$
	- As of the current, there are to K gains
	- Typical: One gain and a time constant for the integral
		$$u(t)=K\times e(t)+\sum \frac{K}{\tau_i}e(t)$$
		- The $\tau_i$ is only applied to the integral part of the controller
			- i.e. sec/repeat
###### Pseudocode:
- Error: Setpoint - Process value
- Reset (Integral) Register) = Reset + K/tau_i * Error
	- As times goes on: the proportional component oscillates
	- Over time the reset register continues to rise as long the process variable is below the setpoint
	- The reset component would increase enough to reach the desired setpoint
- Output = K * Error + Reset
- Units of Parameters
	- Gain (K): Dimensionless
	- $\tau_i$: Seconds/repeat (Integral Time Constant)
		- When the error is not changing:
			- The error will stay the same and the proportional output would be constant
			- Every second/repeat; the initial proportional kick would be done
				- The usage of the proportional is used for controllers for kicks in large changes of input
- Proportional + Derivative
	$$u(t)=K_p\times e(t)+K_d\frac{de(t)}{dt}$$
	- Derivative of the error function
	$$u(t) = K\times \left[e(t)+\frac{1}{\tau_d}\frac{de(t)}{dt}\right]$$
	- It is how fast the error is changing
	$$u(t) = K\times e(t)+\frac{K}{\tau_i}\Delta e(t)$$
	- where $\Delta e$ is the Error - LastError; how fast the error is changing
	- The Derivative term is used to know when the process term is changing too fast
		- Stops overshooting the setpoint
	- Is a the reduction value that results to a value close to the setpoint but not exceeding it; reduced as the change on the delta is reduced
	- Dampening the overshoot that might gain unstable
#### Review

	