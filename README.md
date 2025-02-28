<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Simulador de Investimentos</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background-color: #f9fafb;
      margin: 0;
      padding: 0;
      display: flex;
      flex-direction: column;
      min-height: 100vh;
    }
    .container {
      width: 100%;
      max-width: 1200px;
      margin: 0 auto;
      padding: 20px;
      flex: 1;
    }
    .header {
      text-align: center;
      margin-bottom: 40px;
    }
    .header h1 {
      font-size: 2rem;
      color: #1f2937;
    }
    .header p {
      color: #6b7280;
    }
    .tabs {
      display: flex;
      justify-content: center;
      margin-bottom: 20px;
    }
    .tabs button {
      padding: 10px 20px;
      margin: 0 5px;
      border: none;
      border-radius: 9999px;
      background-color: #fff;
      color: #374151;
      cursor: pointer;
      transition: background-color 0.3s ease;
    }
    .tabs button.active {
      background-color: #3b82f6;
      color: #fff;
    }
    .card {
      background-color: #fff;
      border-radius: 12px;
      box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
      padding: 20px;
      margin-bottom: 20px;
    }
    .card h2 {
      font-size: 1.5rem;
      color: #1f2937;
      margin-bottom: 20px;
    }
    .card label {
      display: block;
      font-size: 0.875rem;
      color: #374151;
      margin-bottom: 5px;
    }
    .card input,
    .card select {
      width: 100%;
      padding: 12px;
      border: 1px solid #d1d5db;
      border-radius: 8px;
      margin-bottom: 15px;
      font-size: 1rem;
    }
    .card button {
      width: 100%;
      padding: 14px;
      background-color: #3b82f6;
      color: #fff;
      border: none;
      border-radius: 8px;
      cursor: pointer;
      font-size: 1rem;
      transition: background-color 0.3s ease;
    }
    .card button:hover {
      background-color: #2563eb;
    }
    .table-container {
      overflow-x: auto;
    }
    table {
      width: 100%;
      border-collapse: collapse;
      background-color: #fff;
    }
    th, td {
      padding: 12px;
      text-align: left;
      border-bottom: 1px solid #e5e7eb;
    }
    th {
      background-color: #f3f4f6;
      font-weight: bold;
    }
    .summary-grid {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 20px;
      margin-bottom: 20px;
    }
    .summary-card {
      background-color: #fff;
      border-radius: 12px;
      box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
      padding: 20px;
      border-top: 4px solid;
    }
    .summary-card h3 {
      font-size: 1.125rem;
      color: #374151;
      margin-bottom: 10px;
    }
    .summary-card p {
      font-size: 1.875rem;
      color: #1f2937;
      font-weight: bold;
    }
    .income-card {
      background: linear-gradient(135deg, #10b981, #06b6d4);
      color: #fff;
      border-radius: 12px;
      padding: 20px;
      text-align: center;
    }
    .income-card h3 {
      font-size: 1.25rem;
      margin-bottom: 10px;
    }
    .income-card p {
      font-size: 2.25rem;
      font-weight: bold;
      margin-bottom: 10px;
    }
    .income-card small {
      font-size: 0.875rem;
      opacity: 0.8;
    }
    .hidden {
      display: none;
    }
    footer {
      background: linear-gradient(135deg, #10b981, #06b6d4);
      color: #fff;
      text-align: center;
      padding: 20px;
      margin-top: auto;
    }
    footer p {
      margin: 0;
      font-size: 1rem;
    }
  </style>
</head>
<body>
  <div class="container">
    <!-- Cabeçalho -->
    <div class="header">
      <h1>Simulador de Investimentos</h1>
      <p>Calcule como seu dinheiro crescerá ao longo do tempo</p>
    </div>

    <!-- Navegação -->
    <div class="tabs">
      <button id="tab-input" class="active" onclick="showTab('input')">Simulação</button>
      <button id="tab-table" onclick="showTab('table')">Tabela</button>
      <button id="tab-summary" onclick="showTab('summary')">Resumo</button>
    </div>

    <!-- Tela de entrada -->
    <div id="input" class="card">
      <h2>Simulação de Investimento</h2>
      <form id="investment-form">
        <label for="monthlyContribution">Aporte Mensal (R$)</label>
        <input type="number" id="monthlyContribution" value="1500" min="1" required />

        <div class="grid">
          <div>
            <label for="timeValue">Tempo</label>
            <input type="number" id="timeValue" value="10" min="1" required />
          </div>
          <div>
            <label for="timeUnit">Unidade</label>
            <select id="timeUnit">
              <option value="anos">Anos</option>
              <option value="meses">Meses</option>
            </select>
          </div>
        </div>

        <label for="annualRate">Taxa de Juros Anual (%)</label>
        <input type="number" id="annualRate" value="13" step="0.01" min="0" required />

        <button type="button" onclick="calculateInvestment()">Simular Investimento</button>
      </form>
    </div>

    <!-- Tabela de Resultados -->
    <div id="table" class="card hidden">
      <h2>Tabela de Investimentos</h2>
      <div class="table-container">
        <table id="results-table">
          <thead>
            <tr>
              <th>Ano</th>
              <th>Aporte Anual</th>
              <th>Investimento Acumulado</th>
              <th>Juros do Ano</th>
              <th>Juros Acumulados</th>
              <th>Patrimônio Total</th>
            </tr>
          </thead>
          <tbody>
            <!-- Dados da tabela serão preenchidos aqui -->
          </tbody>
        </table>
      </div>
    </div>

    <!-- Resumo -->
    <div id="summary" class="card hidden">
      <h2>Resumo do Investimento</h2>
      <div class="summary-grid">
        <div class="summary-card" style="border-top-color: #3b82f6;">
          <h3>Valor Investido</h3>
          <p id="total-invested">R$ 0.00</p>
        </div>
        <div class="summary-card" style="border-top-color: #10b981;">
          <h3>Juros Obtidos</h3>
          <p id="total-interest">R$ 0.00</p>
        </div>
        <div class="summary-card" style="border-top-color: #f59e0b;">
          <h3>Patrimônio Total</h3>
          <p id="total-balance">R$ 0.00</p>
        </div>
      </div>
      <div class="income-card">
        <h3>Rendimento Mensal Estimado (1%)</h3>
        <p id="monthly-income">R$ 0.00</p>
        <small>Valor mensal que você poderia receber mantendo o patrimônio aplicado a 1% ao mês</small>
      </div>
    </div>
  </div>

  <!-- Footer -->
  <footer>
    <p>Código desenvolvido por Vitor Tamos - IA's</p>
  </footer>

  <script>
    // Função para alternar entre as abas
    function showTab(tabName) {
      document.querySelectorAll('.card').forEach(card => card.classList.add('hidden'));
      document.getElementById(tabName).classList.remove('hidden');

      document.querySelectorAll('.tabs button').forEach(button => button.classList.remove('active'));
      document.getElementById(`tab-${tabName}`).classList.add('active');
    }

    // Função para formatar números no padrão brasileiro
    function formatCurrency(value) {
      return value.toLocaleString('pt-BR', {
        minimumFractionDigits: 2,
        maximumFractionDigits: 2
      });
    }

    // Função para calcular o investimento
    function calculateInvestment() {
      // Obter valores dos inputs
      const monthlyContribution = parseFloat(document.getElementById('monthlyContribution').value);
      const timeValue = parseFloat(document.getElementById('timeValue').value);
      const timeUnit = document.getElementById('timeUnit').value;
      const annualRate = parseFloat(document.getElementById('annualRate').value);

      // Converter inputs para cálculos
      const monthlyRate = annualRate / 100 / 12;
      const totalMonths = timeUnit === 'anos' ? timeValue * 12 : timeValue;

      let totalInvested = 0;
      let totalBalance = 0;
      const tableBody = document.querySelector('#results-table tbody');
      tableBody.innerHTML = ''; // Limpar tabela anterior

      // Calcular mês a mês
      for (let month = 1; month <= totalMonths; month++) {
        totalInvested += monthlyContribution;
        const monthlyInterest = totalBalance * monthlyRate;
        totalBalance += monthlyContribution + monthlyInterest;

        // Adicionar linha na tabela a cada 12 meses ou no final
        if (month % 12 === 0 || month === totalMonths) {
          const year = Math.ceil(month / 12);
          const yearlyInterest = monthlyInterest * 12; // Aproximação simples

          const row = document.createElement('tr');
          row.innerHTML = `
            <td>${year}</td>
            <td>R$ ${formatCurrency(monthlyContribution * 12)}</td>
            <td>R$ ${formatCurrency(totalInvested)}</td>
            <td>R$ ${formatCurrency(yearlyInterest)}</td>
            <td>R$ ${formatCurrency(totalBalance - totalInvested)}</td>
            <td>R$ ${formatCurrency(totalBalance)}</td>
          `;
          tableBody.appendChild(row);
        }
      }

      // Atualizar resumo
      document.getElementById('total-invested').textContent = `R$ ${formatCurrency(totalInvested)}`;
      document.getElementById('total-interest').textContent = `R$ ${formatCurrency(totalBalance - totalInvested)}`;
      document.getElementById('total-balance').textContent = `R$ ${formatCurrency(totalBalance)}`;
      document.getElementById('monthly-income').textContent = `R$ ${formatCurrency(totalBalance * 0.01)}`;

      // Mostrar a tabela
      showTab('table');
    }
  </script>
</body>
</html>
