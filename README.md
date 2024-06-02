document.getElementById('showQueryButton').addEventListener('click', () => {
    document.getElementById('queryModalBody').textContent = fetchedQuery;
    $('#queryModal').modal('show');
});
