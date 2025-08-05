# Funding

## How is funding calculateed?
differs for different exchanges, but its generally interest rate + premium

at every funding time, in theory, price should fall by exactly the funding.  If not, we can trade at `time - epsilon` and `time + epsilon` in the correct direction to "arb" the funding.

assuming constant (interest rate/ borrow rate?), price should increase linearly by exactly the funding amount