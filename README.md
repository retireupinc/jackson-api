
# Jackson API Documentation
## Base URL https://pro-uat.retireup.com/jackson

## /plan `POST`
Creates a plan and generates a plan outcome.  Responds with id of the plan outcome.  Use this id to retreive the outcome via GET.

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
  // 1) all cash
  // 2) 80% AGG / 20% SPY
  // 3) 40% AGG / 60% SPY
  // 4) 20% AGG / 80% SPY
  // 5) 0% AGG / 100% SPY 
  strategy: 2,
  
  //Retirement Age
  retire: 65,
  
  //Annuity parameters used.  Created via /annuity POST.  If not entered defaults to annuity with 7% compound rollup and 5% withdrawal.
  annuity: ###
}
```

EXAMPLE
```
curl -d '{"age":"50", "savings":"700000", "strategy":"1", "retire": "65"}' -H "Content-Type: application/json" -X POST https://pro-uat.retireup.com/jackson/plan
```

## /plan/:id `GET`
Retrieves a plan outcome with runs at 0%, 25%, 50%, 75% invested in the annuity.

Response BODY
``` 
input: {}, //input plan parameters
results: [
  {
    //Percentage of savings invested in annnuity
    moved: 50

    //Plan Probability 0-99
    success: 50,

    //Income Stability.  This represents the income stability ratio of the entire plan on the median run.
    isr: 88

    //Legacy.  Amount of money passed on after the plan on the median run
    legacy: 55468,

  }
]
```

EXAMPLE
```
curl https://pro-uat.retireup.com/jackson/plan/###
```

## /annuity POST
Creates annuity for use in plan with parameters created.  Responds with id of the annuity which can be used as input to a plan.

BODY
``` 
{
  //Description
  description: "Jackson Perspective Advisory II®",
  
  //Living Benefit Rider Gauranteed Credit
  credit: 5
  
  //Living Benefit Rider Credit Interest Type (compound, simple)
  interest: "compound"
  
  //Living Benefit Rider withdrawal percent
  withdraw: 7,
  
  //Rider Fee
  riderFee: 0.9,
  
  //M & E Fee
  meFee: 0,
  
}
```
