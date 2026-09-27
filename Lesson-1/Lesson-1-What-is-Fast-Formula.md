# Oracle Fusion HCM Fast Formula

## Lesson 1: What is Fast Formula?

**Author:** Praveen Irulappan

**LinkedIn:** https://www.linkedin.com/in/praveen7595/

---

### Learning Level
🟢 Beginner

### What You Will Learn

- What Fast Formula is
- How Fast Formula works
- Inputs
- DBIs
- Contexts
- Functions
- Local variables
- Return values
- Formula Types
- Where to create Fast Formulas
- Basic Fast Formula structure
- First Payroll example

  
LESSON 1: What is Fast Formula?
Let's start from absolute zero.
1. Forget the syntax for now
Imagine your client tells you:
"Employees working more than 8 hours should receive overtime pay."
You need to convert this business requirement into something Oracle can execute.
The business rule is:
IF hours worked > 8
THEN
    overtime = hours worked - 8
ELSE
    overtime = 0
Then suppose the hourly rate is ₹500.
For an employee who worked 10 hours:
Regular hours = 8
Overtime hours = 10 - 8
               = 2

Overtime amount = 2 × ₹500
                = ₹1,000
Fast Formula is the rule engine that allows Oracle HCM to perform this kind of calculation or decision.
2. What does Fast Formula actually do?
Think of Fast Formula as:
                 ORACLE HCM
                     │
                     ▼
             ┌─────────────────┐
             │   Fast Formula  │
             │                 │
Input Data ─►│ Business Logic  │─► Output
             │                 │
             └─────────────────┘
For example:
Hours Worked = 10
Hourly Rate  = 500
                  │
                  ▼
            FAST FORMULA
                  │
                  ▼
        Overtime = 1,000
The formula itself doesn't randomly know everything about the employee.
It gets information through specific mechanisms:
1. Inputs
Values passed into the formula.
Example:
IV_HOURS
IV_RATE
2. Database Items, or DBIs
Oracle-provided read-only variables that expose application data to the formula.
For example, depending on the formula type and available DBIs, a formula may access employee-related information without writing SQL directly. Oracle describes DBIs as read-only variables used to retrieve application data without needing to know the underlying data model.
3. Contexts
The environment in which the formula is running.
For example:
PERSON_ID
PAYROLL_ID
EFFECTIVE_DATE
Oracle describes contexts as execution values that help determine which employee, payroll, date, etc. the formula is operating against. They also act like SQL bind values when DBIs retrieve data.
4. Functions
Built-in operations such as:
ROUND()
TO_CHAR()
ADD_DAYS()
GET_CONTEXT()
GET_VALUE_SET()
5. Local variables
Temporary variables that you create yourself.
Example:
L_OVERTIME_HOURS
L_OVERTIME_AMOUNT
6. Return values
The result that the formula sends back.
3. Your first Fast Formula
Let's write the simplest possible formula.
Requirement:
Calculate 10% of an amount.
INPUTS ARE IV_AMOUNT (NUMBER)

L_RESULT = IV_AMOUNT * 0.10

RETURN L_RESULT
Suppose:
IV_AMOUNT = 10,000
Then:
L_RESULT = 10,000 × 0.10
         = 1,000
So:
RETURN L_RESULT
returns:
1,000
4. Understand every line
Line 1
INPUTS ARE IV_AMOUNT (NUMBER)
We're telling Oracle:
"This formula expects a number called IV_AMOUNT."
Think of it like a function parameter in programming.
Python:
def calculate(amount):
Fast Formula:
INPUTS ARE IV_AMOUNT (NUMBER)
Line 2
L_RESULT = IV_AMOUNT * 0.10
We're creating a local variable:
L_RESULT
and calculating:
IV_AMOUNT × 10%
The L_ prefix isn't mandatory, but it's an excellent naming convention.
I recommend:
IV_ = Input Variable
L_  = Local Variable
O_  = Output Variable
For example:
IV_SALARY
L_BONUS
O_AMOUNT
This makes formulas much easier to read.
Line 3
RETURN L_RESULT
This sends the result back to the process calling the formula.
So mentally:
INPUT
  ↓
IV_AMOUNT
  ↓
CALCULATION
  ↓
L_RESULT
  ↓
RETURN
  ↓
Oracle Payroll / HCM process
5. The most important concept: Fast Formula isn't just "code"
This is where many beginners go wrong.
They think:
"I need to learn Fast Formula syntax."
Syntax is only one part.
The real skill is understanding how Oracle supplies data to the formula.
Consider:
L_SALARY = ???
You immediately need to ask:
Where does salary come from?
Possibilities include:
Input?
DBI?
Context?
Value set?
Another formula?
Balance?
Element entry?
That's why professional Fast Formula development starts with the execution contract, not with typing code.
6. Your first mental model
Memorize this:
                FAST FORMULA
                     │
       ┌─────────────┼─────────────┐
       │             │             │
       ▼             ▼             ▼
    INPUTS         DBIs        CONTEXTS
       │             │             │
       └─────────────┼─────────────┘
                     ▼
                  LOGIC
                     │
       ┌─────────────┼─────────────┐
       ▼             ▼             ▼
   Variables      Functions      Conditions
                     │
                     ▼
                 RETURN
This diagram will eventually become second nature.
7. Where do we actually write Fast Formula?
In the Fusion HCM application, the exact navigation depends somewhat on the formula area and product flow.
A common current path is:
My Client Groups → Fast Formulas
For Payroll-specific work, you may also encounter:
My Client Groups → Payroll → Fast Formulas
Then you generally select:
Create Fast Formula
        ↓
Formula Name
        ↓
Formula Type
        ↓
Description
        ↓
Effective Start Date
        ↓
Formula Text
        ↓
Compile
The critical point is:
Formula Type comes BEFORE code.
Why?
Because the formula type determines what the formula is allowed to do and which contexts, inputs, DBIs and return requirements are available.
Oracle's documentation also emphasizes that formula usage corresponds to specific formula types with their own input/output requirements.
8. Formula Type is extremely important
Suppose your client says:
"Create a payroll calculation formula."
Don't immediately start writing:
INPUTS ARE ...
First ask:
What formula type?
For example, Oracle Payroll can use Fast Formula for things such as:
Payroll calculations
Element processing
Skip rules
Validation
Proration
Data loading
Other payroll processing decisions
Oracle explicitly documents these types of Payroll uses for Fast Formula.
The formula type is effectively the contract between Oracle and your formula.
9. Context: your first exposure
Now let's introduce one of the most important concepts.
Suppose Oracle is processing:
Employee: Praveen
Person ID: 12345
Payroll: Monthly Payroll
Effective Date: 30-Sep-2026
How does the formula know:
"I'm currently processing Praveen"?
Through the execution context.
For example:
PERSON_ID
Oracle provides the context.
You can retrieve it using:
L_PERSON_ID = GET_CONTEXT(PERSON_ID, -1)
Conceptually:
Oracle Payroll
      │
      │ PERSON_ID = 12345
      ▼
Fast Formula
      │
      ▼
GET_CONTEXT(PERSON_ID, -1)
      │
      ▼
L_PERSON_ID = 12345
Oracle documents GET_CONTEXT as the mechanism for retrieving a context value, with a default value if the context isn't set.
10. Context vs Input
This distinction is interview gold.
Input
Something explicitly passed to your formula.
INPUTS ARE IV_HOURS (NUMBER)
Think:
"Here is some data. Please calculate using it."
Context
Information describing the execution environment.
PERSON_ID
PAYROLL_ID
EFFECTIVE_DATE
Think:
"Who am I processing, and under what circumstances?"
DBI
Oracle-provided application data.
Think:
"Give me this piece of HCM information."
So:
INPUT
= data passed into formula

CONTEXT
= environment in which formula runs

DBI
= application data exposed to formula
Do not mix these three concepts.
11. A real-world example
Imagine this requirement:
"Pay a bonus of 10% if the employee's salary is greater than ₹50,000."
We need:
Salary
      ↓
Is salary > 50,000?
      ↓
YES ──────────► Bonus = Salary × 10%
      │
NO
      ↓
Bonus = 0
Formula logic:
INPUTS ARE IV_SALARY (NUMBER)

L_BONUS = 0

IF IV_SALARY > 50000 THEN
(
  L_BONUS = IV_SALARY * 0.10
)

RETURN L_BONUS
Notice something important.
We didn't write:
SELECT salary
FROM employee
That's not how you should think about Fast Formula.
Instead, Oracle exposes the relevant information through supported formula inputs, DBIs, contexts and functions.
12. Your first interview question
Q: What is Fast Formula?
A strong interview answer:
Fast Formula is Oracle Fusion HCM's rule and calculation engine used to perform business logic such as payroll calculations, validations, eligibility decisions, proration and other HCM processing. A formula receives data through inputs, contexts and database items, processes that data using variables, conditions and functions, and returns the required result to the calling HCM process.
That's much better than:
"Fast Formula is used for calculations."
The second answer is technically true, but too shallow for an experienced Oracle HCM consultant.
13. One very important modern rule
Your original notes contain a lot of historical material. We are not going to memorize everything in them blindly.
For example, modern Oracle guidance emphasizes:
Use inputs where practical instead of unnecessarily fetching DBIs.
Don't access DBIs until they're actually needed.
Use ALIAS appropriately.
Avoid unnecessary CHANGE_CONTEXTS.
Exit loops early where possible.
Keep formulas short and efficient.
These performance principles will become much more important when we reach advanced Payroll formulas.
🎯 Lesson 1 assignment
Don't write a complicated Payroll formula yet.
Write this formula yourself:
Requirement
An employee receives a bonus equal to 5% of their salary if salary is greater than ₹40,000. Otherwise, the bonus is zero.
You need to produce:
INPUTS ARE ...

...

RETURN ...
Then answer these 5 questions:
Q1. What is the input?
Q2. What is the local variable?
Q3. What condition are we checking?
Q4. What calculation happens when the condition is TRUE?
Q5. What happens when it is FALSE?
Send me your formula, even if you think it's wrong.
