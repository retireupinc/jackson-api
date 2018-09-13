
# Jackson API Documentation
## Base URL https://pro-uat.retireup.com/jackson

## /plan `POST`
Creates a plan and generates a plan outcome.  Responds with id of the plan outcome.

BODY
``` 
{
  //Age of the person
  age: 50,
  
  //Current Annual Salary.  Defaults to 60k if not entered.
  salary: 100000
  
  //Amount of total savings.  The calculation assumes this money is in a qualified account, like an IRA or 401k
  savings: 500000,
  
  //Investment Strategy
  // 0) all cash
  // 1) 80% AGG / 20% SPY
  // 2) 60% AGG / 40% SPY
  // 3) 40% AGG / 60% SPY
  // 4) 20% AGG / 80% SPY
  // 5) 0% AGG / 100% SPY 
  strategy: 3,
  
  //Retirement Age
  retire: 65,
  
  //Annuity parameters used.  Created via /annuity POST.  If not entered defaults to annuity with 7% compound rollup and 5% withdrawal.
  annuity: ###
}
```

EXAMPLE
```
curl -d '{"age":"50", "savings":"700000", "strategy":"1", "retire": "65", annuity}' -H "Content-Type: application/json" -X POST https://pro-uat.retireup.com/jackson/plan
```

## /plan/:id `GET`
Retrieves a plan outcome with breaks at 0%, 25%, 50%, 75% invested in an annuity.

Response BODY
``` 
{
  //Invested amount in annnuity
  annuity: 50
  
  //Plan Probability 0-99
  probability: 50,
  
  //Income Stability.  This represents the income stability ratio of the entire plan on the median run.
  ISR: 88
  
  //First Year Income Stability.  Income stability ratio on the first year in retirement for the median run.
  ISR1: 90
  
  //Legacy.  Amount of money passed on after the plan on the median run
  legacy: 500000,
  
}
```
