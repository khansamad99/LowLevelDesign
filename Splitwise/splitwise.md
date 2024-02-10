Step 1: Flow

            Add Friends
- Add------>f1,f2,f3,f4->Expense->Split

            Add Groups
- Add------>g1,g2,g3,g4->Expense->Split between(f1,f2....)


Step 2 : Requirements

- Add Friends
- Add/Manage Groups
- Add/Manage Friends Inside Groups
- Manage Expense inside a group/outside a group
- Split Expense Capablities
        Clear from Interviewer
            - Split Equally
            - Split Unequally
            - Percentage Split
- Balance Sheet -> To View Transactions

Step 3: Objects Identification(Classes)

- Splitwise(Driver Object)
- User
- Group
- Expense
- Split
- BalanceSheet

Classes and UML Diagram

    EXPENSES                                                                
 - String _id                                                           
 - String desc
 - int amount
 - USER paid_by
 - SplitType split_type -> enum SplitType {equal, unequal, percentage}
 - List <SPLIT>

    ExpenseController
- CreateExpense()
    - SplitFactory<FACTORY DESIGN PATTERN>
    - Interface -> validateReq() , computeAmount()

    SPLIT
- User user
- double amount
- int percentage

    USER
- String user_id
- String name
- UserExpenseSheet

    UserController
- List<USER>

    GROUP
- String _id
- String name
- List<USER>
- List<EXPENSE>
- ExpenseController

    GroupController
- List<GROUP>

    UserExpenseBalanceSheet
- map<USER,BALANCE>
- double totalExpense
- double totalPayment
- double totalOWN
- doubel totalGetBack

    BALANCE
- double amountOwn
- double amountGetback

    BalanceSheetController
- ExpenseController
// Business LOGIC


UML Diagram LINK : 