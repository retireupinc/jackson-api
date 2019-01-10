
# Jackson API Documentation
## Base URL https://pro-uat.retireup.com/jackson

## /access `POST`
Exchanges an API key for a short-lived Bearer token.

BODY
``` 
{
  //API key that grants access to API host from caller's IP address
  key: "ABCD123"
}
```

EXAMPLE
```
curl -d '{"key":"ABCD123"}' -H "Content-Type: application/json" -X POST https://pro-uat.retireup.com/jackson/access
```

RESPONSE
```
{"ok":true,"token":"efghi...jklmno"}
```

Then, for all other API calls, include the Authorization header with the Bearer token 
```
curl -H 'Authorization: Bearer efghi...jklmno' https://pro-uat.retireup.com/jackson/plan/###
```

## /plan `POST`
Creates a plan and generates a plan outcome.  Responds with id of the plan outcome, as well as the outcome.  Use the id if you like to retreive the outcome via GET.  Or simply use the outcome returned from this POST

BODY
``` 
{
  //Age of the person.
  //Defaults to 55 if non-number or nothing entered.
  //Only Valid values are 45,55,65.  Everything else throws 422.
  age: 55,
  
  //Current Annual Salary. Drives Social Security Estimate
  //Defaults to 60k if non-number or nothing entered
  //Only Valid values are 0-200,000.  Everything else throws 422.
  salary: 100000
  
  //Annual Retirement Income Need.  Inflated by 2.25% yearly.
  //Defaults to 60k if non-number or nothing entered
  //Only Valid values are 30k - 120k.  Everything else throws 422.
  income: 80000
  
  //Amount of total non qualified savings.  Investment defaulted to 50/50 - SPY/AGG
  //Defaults to 100k if non-number or nothing entered
  //Only Valid values are 0 - 1.5M.  Everything else throws 422.
  nqSavings: 500000,
  
  //Amount of total qualified savings. Investment defaulted to 50/50 - SPY/AGG
  //Defaults to 500k if non-number or nothing entered
  //Only Valid values are 0 - 1.5M.  Everything else throws 422.
  qSavings: 500000
  
  //Non Qual Annuity Percentage 0 - 100
  //Percentage of Non Qual savings moved to Annuity
  //Defaults to 0 if non-number or nothing entered. 
  //Only Valid values are 0 - 100.  Everything else throws 422.
  nqAnnuity: 50
  
  //Qual Annuity Percentage 0 - 100
  //Percentage of Qual savings moved to Annuity
  //Defaults to 0 if non-number or nothing entered. 
  //Only Valid values are 0 - 100.  Everything else throws 422.
  qAnnuity: 50
  
  //Retirement Age
  //Always defaults to 65 no matter what is entered.
  retire: 65,
  
  //Annuity used.
  // 1 - PAII with Lifeguard Freedom Flex with 7% bonus and Income Max; 80/20 asset allocation
  //Always defaults to 1 no matter what is entered.  #2 no longer used.
  annuity: 1
}
```

EXAMPLE
```
curl -d '{"age":"50", "qSavings":"700000", "retire": "65"}' -H "Content-Type: application/json" -X POST https://pro-uat.retireup.com/jackson/plan
```

RESPONSE
```
{
  input: [{},{}] //input plan parameters
  result: 
  [
    {

      //Plan Probability 0-99
      success: 50,
      
      //detailed results from the median run
      median: {
      
        //Income Stability.  This represents the income stability ratio of the entire plan on the median run.
        isr: 88

        //Legacy.  Amount of money passed on after the plan on the median run
        legacy: 55468,

        age:[] //age of the person
        annuity:[] //income from annuity
        nonAnnuity:[] //income from non annuity sources (social security, IRA, savings)
        short:[] //amount short of income need
      },
      
     //detailed results from the bad timing run, 10% quantile of the monte carlo runs
      low: {
      
        //Income Stability.  This represents the income stability ratio of the entire plan on the low run.
        isr: 88

        //Legacy.  Amount of money passed on after the plan on the low run
        legacy: 55468,

        age:[] //age of the person
        annuity:[] //income from annuity
        nonAnnuity:[] //income from non annuity sources (social security, IRA, savings)
        short:[] //amount short of income need
      }

    },
    {//second result running api with nqAnnuity = 0 and qAnnuity = 0}
    ]
}
```
