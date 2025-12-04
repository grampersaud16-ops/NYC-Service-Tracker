<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>FIRE RESPONSE INC SERVICE CALLS</title>
<style>
  body { 
    font-family: Arial, sans-serif; 
    background-color: pink; 
    margin: 0; 
    padding: 20px; 
  }
  h1 { 
    text-align: center; 
    color: #fff; 
    background-color: red; 
    padding: 15px; 
    border-radius: 10px; 
  }
  .tracker-container {
    display: flex; 
    justify-content: space-between; 
    flex-wrap: wrap; 
    margin-top: 20px;
  }
  .borough {
    width: 23%; 
    background: #fff; 
    border-radius: 10px; 
    padding: 10px; 
    margin-bottom: 20px;
    box-shadow: 2px 2px 8px rgba(0,0,0,0.2);
  }
  .borough h2 {
    text-align: center; 
    margin-top: 0;
  }
  table {
    width: 100%;
    border-collapse: collapse;
    margin-bottom: 10px;
  }
  th, td {
    border: 1px solid #ccc;
    padding: 5px;
    text-align: left;
  }
  select, input, textarea {
    width: 100%;
    box-sizing: border-box;
    margin-bottom: 5px;
  }
  .status-open { background-color: red; color: white; }
  .status-completed { background-color: green; color: white; }
  .status-pending { background-color: orange; color: white; }
  .status-to-return { background-color: purple; color: white; }
  button { padding: 5px; width: 100%; margin-bottom: 10px; background-color: red; color: white; border: none; border-radius: 5px; cursor: pointer; }
</style>
</head>
<body>

<h1>FIRE RESPONSE INC SERVICE CALLS</h1>

<div class="tracker-container" id="tracker-container">
  <div class="borough" id="Queens"><h2>Queens</h2></div>
  <div class="borough" id="Brooklyn"><h2>Brooklyn</h2></div>
  <div class="borough" id="Manhattan"><h2>Manhattan</h2></div>
  <div class="borough" id="Bronx"><h2>Bronx</h2></div>
</div>

<script>
let jobs = JSON.parse(localStorage.getItem('nycJobs')) || {Queens: [], Brooklyn: [], Manhattan: [], Bronx: []};

function saveJobs() {
  localStorage.setItem('nycJobs', JSON.stringify(jobs));
}

function createBoroughSection(boroughDiv, boroughName) {
  const form = document.createElement('div');
  form.innerHTML = `
    <input type="text" placeholder="Job site" class="job-site">
    <input type="text" placeholder="Trouble/issues" class="job-trouble">
    <select class="job-status">
      <option value="open">Open</option>
      <option value="completed">Completed</option>
      <option value="pending">Pending</option>
      <option value="to-return">To Return</option>
    </select>
    <textarea placeholder="Notes/outcomes" class="job-notes"></textarea>
    <button>Add Job</button>
    <button class="sort-btn">Sort by Status</button>
  `;
  boroughDiv.appendChild(form);

  const table = document.createElement('table');
  table.innerHTML = `
    <thead>
      <tr>
        <th>Job Site</th>
        <th>Trouble/Issues</th>
        <th>Status</th>
        <th>Notes</th>
      </tr>
    </thead>
    <tbody class="job-table-body"></tbody>
  `;
  boroughDiv.appendChild(table);
  const tbody = table.querySelector('.job-table-body');

  form.querySelector('button').addEventListener('click', () => {
    const site = form.querySelector('.job-site').value.trim();
    const trouble = form.querySelector('.job-trouble').value.trim();
    const status = form.querySelector('.job-status').value;
    const notes = form.querySelector('.job-notes').value.trim();
    if (!site || !trouble) return alert('Please enter Job Site and Trouble/Issues');

    const job = {site, trouble, status, notes};
    jobs[boroughName].push(job);
    saveJobs();
    renderJobRow(tbody, job, boroughName);
    form.querySelector('.job-site').value = '';
    form.querySelector('.job-trouble').value = '';
    form.querySelector('.job-notes').value = '';
  });

  form.querySelector('.sort-btn').addEventListener('click', () => {
    jobs[boroughName].sort((a,b) => {
      const order = {open:1, pending:2, 'to-return':3, completed:4};
      return order[a.status] - order[b.status];
    });
    renderAllJobs(tbody, boroughName);
  });

  jobs[boroughName].forEach(job => renderJobRow(tbody, job, boroughName));
}

function renderJobRow(tbody, job, boroughName) {
  const row = document.createElement('tr');

  const siteCell = document.createElement('td');
  const siteInput = document.createElement('input');
  siteInput.value = job.site;
  siteInput.addEventListener('input', () => { job.site = siteInput.value; saveJobs(); });
  siteCell.appendChild(siteInput);

  const troubleCell = document.createElement('td');
  const troubleInput = document.createElement('input');
  troubleInput.value = job.trouble;
  troubleInput.addEventListener('input', () => { job.trouble = troubleInput.value; saveJobs(); });
  troubleCell.appendChild(troubleInput);

  const statusCell = document.createElement('td');
  const statusSelect = document.createElement('select');
  ['open','completed','pending','to-return'].forEach(s => {
    const opt = document.createElement('option');
    opt.value = s;
    opt.textContent = s.charAt(0).toUpperCase() + s.slice(1);
    if (job.status === s) opt.selected = true;
    statusSelect.appendChild(opt);
  });
  const statusLabel = document.createElement('span');
  statusLabel.textContent = job.status.toUpperCase();
  statusLabel.className = 'status-' + job.status;
  statusSelect.addEventListener('change', () => {
    job.status = statusSelect.value;
    statusLabel.textContent = job.status.toUpperCase();
    statusLabel.className = 'status-' + job.status;
    saveJobs();
  });
  statusCell.appendChild(statusSelect);
  statusCell.appendChild(statusLabel);

  const notesCell = document.createElement('td');
  const notesInput = document.createElement('textarea');
  notesInput.value = job.notes;
  notesInput.addEventListener('input', () => { job.notes = notesInput.value; saveJobs(); });
  notesCell.appendChild(notesInput);

  row.appendChild(siteCell);
  row.appendChild(troubleCell);
  row.appendChild(statusCell);
  row.appendChild(notesCell);

  tbody.appendChild(row);
}

function renderAllJobs(tbody, boroughName) {
  tbody.innerHTML = '';
  jobs[boroughName].forEach(job => renderJobRow(tbody, job, boroughName));
}

['Queens','Brooklyn','Manhattan','Bronx'].forEach(name => {
  const div = document.getElementById(name);
  createBoroughSection(div, name);
});
</script>

</body>
</html>
