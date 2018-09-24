
# Jackson API Documentation
## Base URL https://pro-uat.retireup.com/jackson

## /access `POST`
Exchanges an API key for a short-lived Bearer token.

BODY
``` 
{
  //API key that grants access to API host from caller's IP address
  key: "R20R07649282R48R5"
}
```

EXAMPLE
```
curl -d '{"key":"R20R07649282R48R5"}' -H "Content-Type: application/json" -X POST https://pro-uat.retireup.com/jackson/access
```

RESPONSE
```
{"ok":true,"token":"eyJhbGciOi...buR4uHKM0Be0"}
```

Then, for all other API calls, include the Authorization header with the Bearer token 
```
curl -H 'Authorization: Bearer eyJhbGciOi...buR4uHKM0Be0' https://pro-uat.retireup.com/jackson/plan/###
```

## /plan `POST`
Creates a plan and generates a plan outcome.  Responds with id of the plan outcome.  Use this id to retreive the outcome via GET.

BODY
``` 
{
  //Age of the person.
  //Defaults to 50 if non-number or nothing entered.
  age: 50,
  
  //Current Annual Salary. Drives Social Security Estimate
  //Defaults to 60k if non-number or nothing entered
  salary: 100000
  
  //Annual Retirement Income Need.  Inflated by 2.5% yearly.
  //Defaults to 60k if non-number or nothing entered
  income: 80000
  
  //Amount of total non qualified savings.  Investment defaulted to 50/50 - SPY/AGG
  //Defaults to 100k if non-number or nothing entered
  nqSavings: 500000,
  
  //Amount of total qualified savings. Investment defaulted to 50/50 - SPY/AGG
  //Defaults to 500k if non-number or nothing entered
  qSavings: 500000
  
  //Non Qual Annuity Percentage 0 - 100
  //Percentage of Non Qual savings moved to Annuity
  //Defaults to 0 if non-number or nothing entered. 
  //All input rounded to nearest integer and constrained to 0-100
  nqAnnuity: 50
  
  //Qual Annuity Percentage 0 - 100
  //Percentage of Qual savings moved to Annuity
  //Defaults to 0 if non-number or nothing entered. 
  //All input rounded to nearest integer and constrained to 0-100
  qAnnuity: 50
  
  //Retirement Age
  //Defaults to 65 if non-number or nothing entered
  retire: 65,
  
  //Annuity used.
  // 1 - PAII with Lifeguard Freedom Flex with 7% bonus and Income Max; 80/20 asset allocation
  // 2 - PAII with Lifeguard Freedom Flex with 5% bonus and Income Max; 80/20 asset allocation
  annuity: 1
}
```

EXAMPLE
```
curl -d '{"age":"50", "qSavings":"700000", "retire": "65"}' -H "Content-Type: application/json" -X POST https://pro-uat.retireup.com/jackson/plan
```

## /plan/:id `GET`
Retrieves a plan outcome.

EXAMPLE
```
curl -H "Content-Type: application/json" https://pro-uat.retireup.com/jackson/plan/###
```

RESPONSE
```
{
  input: {}, //input plan parameters
  result: 
    {

      //Plan Probability 0-99
      success: 50,

      //Income Stability.  This represents the income stability ratio of the entire plan on the median run.
      isr: 88

      //Legacy.  Amount of money passed on after the plan on the median run
      legacy: 55468,
      
      //detailed results from the median run
      median: {
        age:[] //age of the person
        annuity:[] //income from annuity
        nonAnnuity:[] //income from non annuity sources (social security, IRA, savings)
        short:[] //amount short of income need
      }

    }
  ]
}
```
