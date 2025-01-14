<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Controle de Associados</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
        }
        form {
            margin-bottom: 20px;
            padding: 15px;
            border: 1px solid #ccc;
            border-radius: 5px;
        }
        label {
            display: block;
            margin: 10px 0 5px;
        }
        input, select, button {
            padding: 8px;
            width: 100%;
            margin-bottom: 10px;
            border: 1px solid #ccc;
            border-radius: 5px;
        }
        button {
            background-color: #4CAF50;
            color: white;
            cursor: pointer;
        }
        button:hover {
            background-color: #45a049;
        }
        table {
            width: 100%;
            border-collapse: collapse;
        }
        th, td {
            border: 1px solid #ccc;
            padding: 8px;
            text-align: left;
        }
    </style>
</head>
<body>

<h1>Controle de Associados</h1>

<!-- Formulário de Cadastro -->
<h2>Cadastro de Ativação</h2>
<form id="activationForm">
    <label for="name">Nome:</label>
    <input type="text" id="name" required>
    
    <label for="date">Data:</label>
    <input type="date" id="date" required>
    
    <label for="cpf">CPF:</label>
    <input type="text" id="cpf" required>
    
    <label for="plate">Placa:</label>
    <input type="text" id="plate" required>
    
    <label for="consultant">Consultor:</label>
    <input type="text" id="consultant" required>
    
    <label for="activationType">Tipo de Ativação:</label>
    <select id="activationType" required>
        <option value="nova">Nova</option>
        <option value="troca">Troca de Titularidade</option>
    </select>
    
    <div id="oldOwnerField" style="display: none;">
        <label for="oldOwner">Nome do Antigo Associado:</label>
        <input type="text" id="oldOwner">
    </div>
    
    <button type="button" onclick="registerActivation()">Cadastrar Ativação</button>
</form>

<!-- Formulário de Cancelamento -->
<h2>Cancelamento</h2>
<form id="cancellationForm">
    <label for="cancelName">Nome:</label>
    <input type="text" id="cancelName" required>
    
    <label for="cancelDate">Data:</label>
    <input type="date" id="cancelDate" required>
    
    <label for="cancelCpf">CPF:</label>
    <input type="text" id="cancelCpf" required>
    
    <label for="cancelPlate">Placa:</label>
    <input type="text" id="cancelPlate" required>
    
    <label for="cancelConsultant">Consultor:</label>
    <input type="text" id="cancelConsultant" required>
    
    <button type="button" onclick="registerCancellation()">Cadastrar Cancelamento</button>
</form>

<!-- Relatórios -->
<h2>Relatório Mensal</h2>
<button onclick="generateReport()">Gerar Relatório por Consultor</button>
<table id="reportTable" style="display: none;">
    <thead>
        <tr>
            <th>Consultor</th>
            <th>Ativações</th>
            <th>Cancelamentos</th>
        </tr>
    </thead>
    <tbody></tbody>
</table>

<script>
    const activations = [];
    const cancellations = [];

    // Exibir campo para troca de titularidade
    document.getElementById('activationType').addEventListener('change', function () {
        const oldOwnerField = document.getElementById('oldOwnerField');
        oldOwnerField.style.display = this.value === 'troca' ? 'block' : 'none';
    });

    // Cadastro de Ativação
    function registerActivation() {
        const name = document.getElementById('name').value;
        const date = document.getElementById('date').value;
        const cpf = document.getElementById('cpf').value;
        const plate = document.getElementById('plate').value;
        const consultant = document.getElementById('consultant').value;
        const activationType = document.getElementById('activationType').value;
        const oldOwner = document.getElementById('oldOwner').value;

        const activation = { name, date, cpf, plate, consultant, activationType, oldOwner };
        activations.push(activation);

        alert('Ativação cadastrada com sucesso!');
        document.getElementById('activationForm').reset();
    }

    // Cadastro de Cancelamento
    function registerCancellation() {
        const name = document.getElementById('cancelName').value;
        const date = document.getElementById('cancelDate').value;
        const cpf = document.getElementById('cancelCpf').value;
        const plate = document.getElementById('cancelPlate').value;
        const consultant = document.getElementById('cancelConsultant').value;

        const cancellation = { name, date, cpf, plate, consultant };
        cancellations.push(cancellation);

        alert('Cancelamento cadastrado com sucesso!');
        document.getElementById('cancellationForm').reset();
    }

    // Gerar Relatório
    function generateReport() {
        const reportTable = document.getElementById('reportTable');
        const tbody = reportTable.querySelector('tbody');
        tbody.innerHTML = '';

        const consultants = {};

        activations.forEach(({ consultant }) => {
            consultants[consultant] = consultants[consultant] || { activations: 0, cancellations: 0 };
            consultants[consultant].activations++;
        });

        cancellations.forEach(({ consultant }) => {
            consultants[consultant] = consultants[consultant] || { activations: 0, cancellations: 0 };
            consultants[consultant].cancellations++;
        });

        for (const [consultant, data] of Object.entries(consultants)) {
            const row = `<tr>
                <td>${consultant}</td>
                <td>${data.activations}</td>
                <td>${data.cancellations}</td>
            </tr>`;
            tbody.innerHTML += row;
        }

        reportTable.style.display = 'table';
    }
</script>

</body>
</html>
