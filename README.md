# Expense-Tracker
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Expense Tracker</title>
  <!-- Bootstrap CSS -->
  <link href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css" rel="stylesheet">
  <style>
    body {
      background-color: #f8f9fa;
      padding-top: 50px;
    }
  </style>
</head>
<body>

  <div class="container">
    <h1 class="text-center mb-4">Expense Tracker</h1>
    
    <!-- Form to add new expense -->
    <form id="expenseForm" class="mb-4">
      <div class="form-group">
        <label for="expenseName">Expense Name</label>
        <input type="text" class="form-control" id="expenseName" required>
      </div>
      <div class="form-group">
        <label for="expenseAmount">Amount</label>
        <input type="number" class="form-control" id="expenseAmount" required>
      </div>
      <button type="submit" class="btn btn-primary">Add Expense</button>
    </form>

    <!-- Table to display expenses -->
    <table id="expenseTable" class="table table-striped">
      <thead>
        <tr>
          <th>Expense Name</th>
          <th>Amount</th>
          <th>Action</th>
        </tr>
      </thead>
      <tbody>
        <!-- Expense data will be displayed here -->
      </tbody>
    </table>
  </div>

  <!-- Bootstrap JS -->
  <script src="https://code.jquery.com/jquery-3.5.1.slim.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/@popperjs/core@2.5.4/dist/umd/popper.min.js"></script>
  <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/js/bootstrap.min.js"></script>

  <script>
    document.addEventListener('DOMContentLoaded', function() {
      // Function to fetch expenses from localStorage and display them
      function displayExpenses() {
        const expenses = JSON.parse(localStorage.getItem('expenses')) || [];

        const expenseTableBody = document.querySelector('#expenseTable tbody');
        expenseTableBody.innerHTML = '';

        expenses.forEach(function(expense, index) {
          const tr = document.createElement('tr');
          tr.innerHTML = `
            <td>${expense.name}</td>
            <td>$${expense.amount}</td>
            <td>
              <button class="btn btn-sm btn-warning edit-btn" data-index="${index}">Edit</button>
              <button class="btn btn-sm btn-danger delete-btn" data-index="${index}">Delete</button>
            </td>
          `;
          expenseTableBody.appendChild(tr);
        });
      }

      displayExpenses();

      // Add expense form submission
      document.querySelector('#expenseForm').addEventListener('submit', function(event) {
        event.preventDefault();

        const name = document.querySelector('#expenseName').value;
        const amount = parseFloat(document.querySelector('#expenseAmount').value);

        if (name.trim() === '' || isNaN(amount) || amount <= 0) {
          alert('Please enter valid expense details.');
          return;
        }

        const newExpense = {
          name,
          amount
        };

        let expenses = JSON.parse(localStorage.getItem('expenses')) || [];
        expenses.push(newExpense);
        localStorage.setItem('expenses', JSON.stringify(expenses));

        displayExpenses();

        // Clear form fields
        document.querySelector('#expenseName').value = '';
        document.querySelector('#expenseAmount').value = '';
      });

      // Delete and Edit expense buttons
      document.querySelector('#expenseTable').addEventListener('click', function(event) {
        if (event.target.classList.contains('delete-btn')) {
          const index = event.target.dataset.index;
          let expenses = JSON.parse(localStorage.getItem('expenses')) || [];
          expenses.splice(index, 1);
          localStorage.setItem('expenses', JSON.stringify(expenses));
          displayExpenses();
        } else if (event.target.classList.contains('edit-btn')) {
          const index = event.target.dataset.index;
          let expenses = JSON.parse(localStorage.getItem('expenses')) || [];
          const expenseToEdit = expenses[index];
          document.querySelector('#expenseName').value = expenseToEdit.name;
          document.querySelector('#expenseAmount').value = expenseToEdit.amount;
          expenses.splice(index, 1);
          localStorage.setItem('expenses', JSON.stringify(expenses));
          displayExpenses();
        }
      });
    });
  </script>
</body>
</html>
