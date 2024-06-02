let fetchedQuery = '';

document.getElementById('loading').style.display = 'block';
document.getElementById('tabView').style.display = 'none';

// Get query input value
const query = document.getElementById('queryInput').value;

// API URL
const apiUrl = 'http://127.0.0.1:8001/ask_question';

// Fetch data from the API
fetch(apiUrl, {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json'
    },
    body: JSON.stringify({ question: query })  // Update the key to match the expected field
})
.then(response => response.json())
.then(data => {
    fetchedQuery = data.query;

    // Clear existing table headers and rows
    const table = document.getElementById('dataTables-example');
    const thead = table.querySelector('thead');
    const tbody = table.querySelector('tbody');

    thead.innerHTML = '';
    tbody.innerHTML = '';

    // Check if data.result is not empty and is an array
    if (Array.isArray(data.result) && data.result.length > 0) {
        // Create table headers dynamically
        const headers = Object.keys(data.result[0]);
        const headerRow = document.createElement('tr');
        headers.forEach(header => {
            const th = document.createElement('th');
            th.textContent = header;
            headerRow.appendChild(th);
        });
        thead.appendChild(headerRow);

        // Populate the table with new data
        data.result.forEach(item => {
            const row = document.createElement('tr');
            headers.forEach(header => {
                const td = document.createElement('td');
                td.textContent = item[header];
                row.appendChild(td);
            });
            tbody.appendChild(row);
        });
    } else {
        // If no data found, display a message in a single row
        const row = document.createElement('tr');
        row.innerHTML = `<td colspan="1">No data found</td>`;
        tbody.appendChild(row);
    }

    // Hide loading indicator and show table
    document.getElementById('loading').style.display = 'none';
    document.getElementById('tabView').style.display = 'block';
    document.getElementById('showQueryButton').style.display = 'inline-block';
})
.catch(error => {
    console.error('Error fetching data:', error);
    alert('Failed to fetch data. Please try again later.');

    // Hide loading indicator
    document.getElementById('loading').style.display = 'none';
});

document.getElementById('showQueryButton').addEventListener('click', () => {
    document.getElementById('queryModalBody').textContent = fetchedQuery;
    $('#queryModal').modal('show');
});
