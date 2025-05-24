# Expense Tracker (ReactJS)
## Date: 24/05/2025

## AIM
To develop a simple Expense Tracker application using React that allows users to manage their personal finances by adding, viewing, and deleting income and expense transactions, while dynamically calculating the current balance, total income, and total expenses.

## ALGORITHM
### STEP 1: Initialize the Project
Create a new React app using:

npx create-react-app expense-tracker
or

npm create vite@latest expense-tracker --template react

Open the project in a code editor like VS Code.

### Step 2: Setup State
Define a state variable to store transactions:

Example: const [transactions, setTransactions] = useState([])

Define state variables for the form inputs:

const [text, setText] = useState("")

const [amount, setAmount] = useState("")

### Step 3: Add a New Transaction
Create a form with two inputs:

Text input for description

Number input for amount

On form submit:

Validate input

Create a new transaction object

Add the object to the transactions array using setTransactions.

### Step 4: Display Transaction List

Use map() to render each transaction in a list.

Conditionally style each item based on amount > 0 (income) or amount < 0 (expense).

Add a delete button next to each transaction.

### Step 5: Calculate and Display Summary

Use reduce() to calculate:

Total Balance: sum of all amounts

Total Income: sum of all positive amounts

Total Expenses: sum of all negative amounts

Display these values at the top of the UI.

### Step 6: Delete a Transaction

When delete is clicked:

Use filter() to remove the transaction from the array by id.

Update the state using setTransactions.

### Step 7: Style the Application

Use basic CSS to style:

Balance summary

Income/expense totals

Form inputs

Transaction list (with color coding)

## PROGRAM
## APP.js
```
import { useState } from 'react';
import './App.css';

function App() {
  const [transactions, setTransactions] = useState([]);
  const [description, setDescription] = useState('');
  const [amount, setAmount] = useState('');

  const addTransaction = (e) => {
    e.preventDefault();
    if (!description || !amount) return;
    
    const newTransaction = {
      id: Date.now(),
      description,
      amount: parseFloat(amount)
    };
    
    setTransactions([...transactions, newTransaction]);
    setDescription('');
    setAmount('');
  };

  const deleteTransaction = (id) => {
    setTransactions(transactions.filter(transaction => transaction.id !== id));
  };

  const balance = transactions.reduce((total, transaction) => total + transaction.amount, 0);
  const income = transactions
    .filter(transaction => transaction.amount > 0)
    .reduce((total, transaction) => total + transaction.amount, 0);
  const expenses = transactions
    .filter(transaction => transaction.amount < 0)
    .reduce((total, transaction) => total + transaction.amount, 0);

  return (
    <div className="container">
      <h1>Expense Tracker</h1>
      
      <div className="balance-box">
        <h2>Your Balance</h2>
        <h3>${balance.toFixed(2)}</h3>
      </div>
      
      <div className="summary">
        <div className="income">
          <h4>Income</h4>
          <p className="money plus">+${income.toFixed(2)}</p>
        </div>
        <div className="expense">
          <h4>Expense</h4>
          <p className="money minus">-${Math.abs(expenses).toFixed(2)}</p>
        </div>
      </div>
      
      <h3>History</h3>
      <div className="transaction-list">
        {transactions.length === 0 ? (
          <p>No transactions yet</p>
        ) : (
          <ul>
            {transactions.map(transaction => (
              <li key={transaction.id} className={transaction.amount > 0 ? 'plus' : 'minus'}>
                <span>{transaction.description}</span>
                <span>${Math.abs(transaction.amount).toFixed(2)}</span>
                <button 
                  onClick={() => deleteTransaction(transaction.id)}
                  className="delete-btn"
                >
                  ×
                </button>
              </li>
            ))}
          </ul>
        )}
      </div>
      
      <h3>Add New Transaction</h3>
      <form onSubmit={addTransaction}>
        <div className="form-control">
          <label htmlFor="description">Description</label>
          <input
            type="text"
            id="description"
            value={description}
            onChange={(e) => setDescription(e.target.value)}
            placeholder="Enter description..."
            required
          />
        </div>
        <div className="form-control">
          <label htmlFor="amount">Amount</label>
          <p>(negative - expense, positive - income)</p>
          <input
            type="number"
            id="amount"
            value={amount}
            onChange={(e) => setAmount(e.target.value)}
            placeholder="Enter amount..."
            step="0.01"
            required
          />
        </div>
        <button type="submit" className="btn">Add Transaction</button>
      </form>
    </div>
  );
}

export default App;
```
## APP.css
```
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
  font-family: 'Arial', sans-serif;
}

.container {
  max-width: 500px;
  margin: 30px auto;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 5px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

h1 {
  text-align: center;
  margin-bottom: 20px;
  color: #333;
}

.balance-box {
  text-align: center;
  margin-bottom: 20px;
}

.balance-box h3 {
  font-size: 24px;
}

.summary {
  display: flex;
  justify-content: space-between;
  margin-bottom: 20px;
  padding: 15px;
  background-color: #f8f9fa;
  border-radius: 5px;
}

.summary div {
  text-align: center;
  flex: 1;
}

.summary h4 {
  margin-bottom: 5px;
  font-size: 16px;
}

.money {
  font-size: 18px;
  font-weight: bold;
}

.plus {
  color: #2ecc71;
}

.minus {
  color: #e74c3c;
}

.transaction-list ul {
  list-style-type: none;
  margin-bottom: 20px;
}

.transaction-list li {
  background-color: #fff;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
  padding: 10px;
  margin-bottom: 10px;
  display: flex;
  justify-content: space-between;
  position: relative;
  border-right: 4px solid;
}

.transaction-list li.plus {
  border-color: #2ecc71;
}

.transaction-list li.minus {
  border-color: #e74c3c;
}

.delete-btn {
  background-color: #e74c3c;
  color: white;
  border: none;
  border-radius: 3px;
  padding: 2px 5px;
  cursor: pointer;
  font-size: 16px;
  line-height: 1;
  position: absolute;
  left: -20px;
  opacity: 0;
  transition: opacity 0.3s ease;
}

.transaction-list li:hover .delete-btn {
  opacity: 1;
}

form {
  margin-top: 20px;
}

.form-control {
  margin-bottom: 15px;
}

.form-control label {
  display: block;
  margin-bottom: 5px;
  font-weight: bold;
}

.form-control input {
  width: 100%;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 16px;
}

.btn {
  width: 100%;
  padding: 10px;
  background-color: #3498db;
  color: white;
  border: none;
  border-radius: 4px;
  font-size: 16px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.btn:hover {
  background-color: #2980b9;
}

@media (max-width: 600px) {
  .container {
    margin: 10px;
    padding: 15px;
  }
  
  .summary {
    flex-direction: column;
  }
  
  .summary div {
    margin-bottom: 10px;
  }
  
  .summary div:last-child {
    margin-bottom: 0;
  }
}
```
## OUTPUT
![alt text](<Screenshot (32).png>)
![alt text](<Screenshot (31).png>)
![alt text](<Screenshot (33).png>)
## RESULT
A fully functional React-based Expense Tracker application was successfully developed. 
