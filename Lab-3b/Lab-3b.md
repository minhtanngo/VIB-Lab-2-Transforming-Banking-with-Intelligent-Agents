
# 🧑‍💼 AskHR: Automate HR tasks with Agentic AI (Lab 3b: Adding tools to the agent)

What we did in Lab-3a was to build a super simple agent that can answer questions based on a knowledge base. In this lab, we will enhance the agent to make it more powerful, by adding more tools.

## Step by step instructions to enhance the HR Agent with additional tools:

1. First, open up the **`hr.yaml`** file within this folder  (provided to you by the instructor), and add your initials to each of the "summary" of each path. For example "/create-expense-claim" summary should be updated from "Create an expense claim" to "NN_Create an expense claim"

<img width="1000" alt="image" src="../new_imgs/hr_b_step1.png">

2. Scroll down to the **Toolset** section. Click on **Add tool +**:

<img width="1000" alt="image" src="../imgs/hr_step8.png">

3. Select **Import**:

<img width="1000" alt="image" src="../imgs/step13.png">

4. Drag and drop or click to upload the **edited** **hr.yaml** file, then click on **Next**:

<img width="1000" alt="image" src="../imgs/hr_step10.png">    

5. Select all the operations and click on **Done**:

<img width="1000" alt="image" src="../new_imgs/hr_b_step5.png">

6. Next we are going to implement a simple example of a "flow". A tool flow is simply a way of chaining together multiple tools or agents. Click on **Add tool +**:

<img width="1000" alt="image" src="../new_imgs/hr_b_step6.png">

7. Next, click "Create a new flow"

<img width="1000" alt="image" src="../new_imgs/hr_b_step7.png">

8. In the flow builder, click on the arrow between Start and End, and search for the "[Your initials]_Get emails for a user" and add it. Do the same for "[Your initials]_Get corporate card transactions for a user" skill, as shown in the screenshots below:

<img width="1000" alt="image" src="../new_imgs/hr_b_step8_1.png">
<img width="1000" alt="image" src="../new_imgs/hr_b_step8_2.png">
<img width="1000" alt="image" src="../new_imgs/hr_b_step8_3.png">

9. Then, click the pencil on the top left of your screen (next to "Untitled")

<img width="1000" alt="image" src="../new_imgs/hr_b_step9.png">

10. Fill in the details as shown, then click "Save Changes". Please be sure to replace occurrences of [Your Initials] with your own initials in both the **Name** and **
\]**.

<img width="1000" alt="image" src="../new_imgs/hr_b_step10.png">

*Name*
[Your initials]_Reconcile Receipts With Card Transactions

*Description*
```
This tool flow enables an employee to reconcile their corporate credit card transactions with trip receipts that they received via email. It returns a table that links any receipts from their emails to corporate card transactions by matching the receipts to card transactions, and lists any unmatched receipts as well.

When you use the NN_Get_emails_for_a_user tool and NN_Get_corporate_card_transactions_for_a_user tool, set the "name" field in the request body as the employee_name input of this tool flow.

The output MUST be in TABLE FORMAT (not bullet points), and you should only return ONE table with the following columns: 
- Transaction ID (exists if there is a matching corporate card transaction for an emailed receipt, set to "N/A" otherwise)
- Supplier
- Date of Transaction
- Transaction Amount
- Receipt File Link (Retrieved From Emails & Uploaded to Box)
- Transaction Type
- Payment Method (card or cash). 

For cases where you successfully matched the corporate card transactions with receipts from emails, you MUST ALWAYS include the Transaction ID(s) read from the corporate card transaction(s). However, note that there may certain receipts from email which do not have corresponding corporate card transactions (+ Transaction IDs), indicating they were not corporate card transactions, and were paid in cash. Therefore, alongside the receipts that do have a matched transaction (and Transaction ID), you MUST STILL include the receipts that DO NOT HAVE a Transaction ID in the table, and leave the Transaction ID as "N/A", while setting the payment method to 'cash'
```
*Inputs*
- name: 
    - Description: Employee name for using as filter in the tools
    - Datatype: string
    - Required: on

*Outputs*
- expense_report: 
    - Description: 
    ```
    Table with the following columns: 
    - Transaction ID (exists if there is a matching corporate card transaction for an emailed receipt, set to "N/A" otherwise)
    - Supplier
    - Date of Transaction
    - Transaction Amount
    - Receipt File Link (Retrieved From Emails & Uploaded to Box)
    - Transaction Type
    - Payment Method (card or cash). 
    ```
    - Datatype: string
    - Required: on

10. Click "Done" on the top right of the flow builder

11. Check in your list of tools that the new tool flow AND an auto-generated tool called "get_flow_status" have been added. If not, please click Add Tool > Add from Local Instance > select and add the "get_flow_status" tool.


12. Scroll down to the **Behavior** section. Insert the instructions below into the **Instructions** field

```
Use your knowledge base to answer general questions about employee benefits. For any questions related to benefits/incentives that are unrelated to healthcare, refer to your KNOWLEDGE.

Use the tools to get or update user specific information.

When user asks to show profile data or check time off balance or update title/address or request time off for the very first time,  first ask the user for their name,  then invoke the tool and then use the same name in the whole session without asking for the name again.

When user request for time off for the very first time,  You must ensure that you have name, leave start date and leave end date before using the tool. If you do not have the information, clarify with the users. Convert the dates to YYYY-MM-DD format

For multiple requests, always use the same name throughout the session.

Always reply in a friendly and complete sentence.

Do not ever call tools, unless you are sure of the correct input parameters. If you are not sure, or have incomplete information (such as a missing user/employee name, request it from the user first)

Call the Reconcile Expenses For a Trip tool flow when an employee requests to reconcile their corporate credit card transactions with trip receipts

For the following tools, please ensure that you know the employee name and if you do not, request the name (do not assume):
- get_emails
- get_corporate_card_transactions
- user_profile_details
- update_address
- update_title
- request_time_off
- time_off_balance
- Reconcile Expenses For a Trip
```
<img width="1000" alt="image" src="../new_imgs/hr_b_step11.png">

13. Scroll up to the **Description** section. Update the description as follows:
```
You are an agent who handles employee HR queries.  You provide short and crisp responses, keeping the output to 200 words or less.  You can help users check their profile data, retrieve latest time off balance, update title or address, retrieve their emails, retrieve their corporate card transactions, request time off, and more.
```
<img width="1000" alt="image" src="../new_imgs/hr_b_step12.png">

14. Refresh the page, then test your agent in the preview chat on the right side by asking the following questions and validating the responses.  They should look similar to what is shown in the screenshots below.

**Note**: if you are prompted for your name, say "Victoria Baker".

```
1. What is the pet policy?

2. Show me my profile data.

3. What is my time off balance?

4. Request time off

6. Reconcile receipts with card transactions for my recent trip
```

- *IF the response to the last question is "the tool flow is still running", then you can wait a few seconds and then type 'show results'*

<img width="1000" alt="image" src="../imgs/hr_step13.png">

<img width="1000" alt="image" src="../new_imgs/hr_b_step13_1.png">

<img width="1000" alt="image" src="../new_imgs/hr_b_step13_4.png">

<img width="1000" alt="image" src="../new_imgs/hr_b_step13_6.png">