# applepay-notion-automation
Track ApplePay expenses in Notion using iOS Automation

# 💸 ApplePay to Notion: Automated Expense Tracker

This automation logs your ApplePay transactions (merchant + amount) directly into a Notion database. It's built using Apple's iOS Automations, Notion API, and text manipulation to handle different currency formatting.

## 🧩 What It Does

- Triggered by ApplePay transactions via iOS Shortcuts Automation
- Sends the transaction info as a new row to a Notion Expense Tracker database

## 🔧 Tools Used

- **iOS Shortcuts Automation** 
- **Notion API** – you’ll need an integration token (guide follows below)

## 🛡 Security and Customization

- DO NOT share your real Notion token or database ID.
- Screenshots in this repo are anonymized.
- This repo shows a basic set up. You might want to customize it by adding more parameters (like Expense category, Date, Account, etc.)

## 🚀 How To Use This

1. **Set up your Notion database**:
   - Make sure it has at least “Name" and "Amount" columns. You can create more columns to customize it to your needs. 
   - Later in the process you'll need the database ID: on the full page of your database, click on menu (three dots at the upper right-hand corner) > Copy link > paste it somewhere (you need to edit the link)
     
      <img src="https://github.com/lenka-ja/applepay-notion-automation/blob/main/N_databaseID1.png" width=30% height=30%>
     
   - From the link delete anything before the slash (including the slash) and after the question mark (incl. the question mark) - you'll end up with alphanumeric string of 32 characters
     
       <img src="https://github.com/lenka-ja/applepay-notion-automation/blob/main/N_databaseID2.png" width=70% height=70%>

2. **Create a Notion integration**:
   - [https://www.notion.so/profile/integrations](https://www.notion.so/profile/integrations)
     
   -  <img src="https://github.com/lenka-ja/applepay-notion-automation/blob/main/API_NewIntegration.png" width=50% height=50%>

   - Create integration name and connect it to your workspace and Save.
   - Define settings and get Internal Integration Secret (do not share this with anyone) - you'll need it later
     
     <img src="https://github.com/lenka-ja/applepay-notion-automation/blob/main/API_IntegrationSecret.png" width=70% height=70%>

3. **Connect your Notion database with your Integration**:
   - On the full page of your database, click on menu > Connections and search for the name of Integration you just created

     <img src="https://github.com/lenka-ja/applepay-notion-automation/blob/main/N_Connection.png" width=50% height=50%>

4. **Create the iOS Automation** manually:

a) Trigger = Transaction

   <img src="https://github.com/lenka-ja/applepay-notion-automation/blob/main/1_Trigger.jpg" width=30% height=30%>

b) Choose the payment card(s) you want to receive the expense from

c) In the next step click on New Blank Automation

d) On scripting tab search for "Get Variable" 

e) Tap on Variable and choose "Shortcut Input" from the menu

   <img src="https://github.com/lenka-ja/applepay-notion-automation/blob/main/2_GetVariable.jpg" width=30% height=30%>

f) Add another action: "Get numbers from" - it should fill with "Shortcut Input" again

g) Add "Round numbers" - I chose to round the numbers to Ones place (to prevent issues with the different formats between transaction and Notion database)

   <img src="https://github.com/lenka-ja/applepay-notion-automation/blob/main/3_RoundNumbers.jpg" width=30% height=30%>

h) Add "Get text from Input", click on "Input" and choose "Select Variable" - search for "Shortcut Input" and choose "Merchant" from the menu

   <img src="https://github.com/lenka-ja/applepay-notion-automation/blob/main/4_Merchant.jpg" width=30% height=30%>

i) Add "URL" and fill: https://api.notion.com/v1/pages

j) Add "Get contents of URL"
Method = POST
Headers:
- Authorization: Bearer [your Integration Secret]
- Notion-Version: 2022-06-28

  Request Body (JSON):
```json
{
  "parent": {
    "type": "database_id",
    "database_id": "DATABASE_ID"
   },
  "properties": {
    "Name": {
      "title": [
        {
          "text": {
            "content": "Merchant"
          }
        }
      ]
    },
    "Amount": {
      "number": "Rounded Number"  
    }
  }
}
```

- Replace the "Merchant" with "Shortcut Input" and search for "Merchant"
- "Rounded Number" is the variable you got from transaction's "Amount"
This block should look like this when done:

   <img src="https://github.com/lenka-ja/applepay-notion-automation/blob/main/5_GetContentsOfURL2.jpg" width=30% height=30%>


4. **Test a transaction** – Try using ApplePay and check your Notion!

## 🧠 Why I Built This

I wanted a real-time, low-effort and no-input-required way to track my personal spending.
