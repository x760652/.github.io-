<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>借款管理系統</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            line-height: 1.6;
            margin: 0;
            padding: 20px;
            background-color: #f5f5f5;
            color: #333;
        }
        
        .container {
            max-width: 1400px;
            margin: 0 auto;
            background: white;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
        }
        
        h1 {
            text-align: center;
            color: #2c3e50;
            margin-bottom: 30px;
        }
        
        .form-section {
            margin-bottom: 30px;
            padding: 20px;
            background: #f8f9fa;
            border-radius: 8px;
        }
        
        .form-row {
            display: grid;
            grid-template-columns: repeat(6, 1fr) auto;
            gap: 10px;
            margin-bottom: 15px;
            align-items: center;
        }
        
        .form-row input, .form-row select {
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 4px;
        }
        
        button {
            background: #3498db;
            color: white;
            border: none;
            padding: 10px 15px;
            border-radius: 4px;
            cursor: pointer;
            transition: background 0.3s;
            margin: 2px;
        }
        
        button:hover {
            background: #2980b9;
        }
        
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
        }
        
        th, td {
            padding: 12px 15px;
            text-align: left;
            border-bottom: 1px solid #ddd;
        }
        
        th {
            background-color: #f8f9fa;
            font-weight: bold;
            position: sticky;
            top: 0;
        }
        
        tr:hover {
            background-color: #f5f5f5;
        }
        
        .due-soon {
            background-color: #fff3cd;
        }
        
        .overdue {
            background-color: #f8d7da;
        }
        
        .paid {
            background-color: #d4edda;
        }
        
        .extend-input {
            width: 60px;
            padding: 5px;
            border: 1px solid #ddd;
            border-radius: 4px;
        }
        
        .payment-input {
            width: 80px;
            padding: 5px;
            border: 1px solid #ddd;
            border-radius: 4px;
        }
        
        .received-input {
            width: 100px;
            padding: 5px;
            border: 1px solid #ddd;
            border-radius: 4px;
        }
        
        .extend-btn {
            background: #ffc107;
            color: #212529;
        }
        
        .extend-btn:hover {
            background: #e0a800;
        }
        
        .pay-btn {
            background: #28a745;
        }
        
        .pay-btn:hover {
            background: #218838;
        }
        
        .restore-btn {
            background: #6c757d;
        }
        
        .restore-btn:hover {
            background: #5a6268;
        }
        
        .delete-btn {
            background: #e74c3c;
        }
        
        .delete-btn:hover {
            background: #c0392b;
        }
        
        .stats-section {
            margin-bottom: 30px;
            padding: 20px;
            background: #e9ecef;
            border-radius: 8px;
        }
        
        .stats-grid {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 20px;
        }
        
        .stat-card {
            background: white;
            padding: 15px;
            border-radius: 8px;
            box-shadow: 0 0 5px rgba(0,0,0,0.1);
        }
        
        .stat-card h3 {
            margin-top: 0;
            color: #6c757d;
            font-size: 16px;
        }
        
        .stat-card p {
            font-size: 24px;
            font-weight: bold;
            margin-bottom: 0;
            color: #2c3e50;
        }
        
        .action-buttons {
            display: flex;
            flex-wrap: wrap;
            gap: 5px;
        }
        
        .period-selector {
            margin-bottom: 20px;
            display: flex;
            gap: 10px;
            align-items: center;
        }
        
        .period-selector select {
            padding: 8px 12px;
            border-radius: 4px;
            border: 1px solid #ddd;
        }
        
        .tab-buttons {
            display: flex;
            margin-bottom: 20px;
            border-bottom: 1px solid #ddd;
        }
        
        .tab-button {
            padding: 10px 20px;
            background: #f8f9fa;
            border: none;
            border-radius: 4px 4px 0 0;
            cursor: pointer;
            margin-right: 5px;
        }
        
        .tab-button.active {
            background: #3498db;
            color: white;
        }
        
        @media (max-width: 768px) {
            .form-row, .stats-grid {
                grid-template-columns: 1fr;
            }
            
            .action-buttons {
                flex-direction: column;
            }
            
            .form-row {
                grid-template-columns: 1fr;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>借款管理系統</h1>
        
        <div class="tab-buttons">
            <button class="tab-button active" onclick="showTab('current')">當前借款</button>
            <button class="tab-button" onclick="showTab('monthly')">月度統計</button>
            <button class="tab-button" onclick="showTab('yearly')">年度統計</button>
        </div>
        
        <div id="current-tab">
            <div class="stats-section">
                <h2>統計概覽</h2>
                <div class="stats-grid">
                    <div class="stat-card">
                        <h3>總借款金額</h3>
                        <p id="total-loan-amount">$0</p>
                    </div>
                    <div class="stat-card">
                        <h3>總實收金額</h3>
                        <p id="total-received-amount">$0</p>
                    </div>
                    <div class="stat-card">
                        <h3>即將到期借款</h3>
                        <p id="due-soon-count">0</p>
                    </div>
                    <div class="stat-card">
                        <h3>已過期借款</h3>
                        <p id="overdue-count">0</p>
                    </div>
                </div>
            </div>
            
            <div class="form-section">
                <h2>新增借款記錄</h2>
                <form id="loan-form">
                    <div class="form-row">
                        <input type="text" id="loan-id" placeholder="編號" required>
                        <input type="text" id="customer-name" placeholder="客戶名稱" required>
                        <input type="tel" id="customer-phone" placeholder="客戶電話">
                        <input type="number" id="loan-amount" placeholder="借款金額" min="0" required>
                        <input type="number" id="received-amount" placeholder="實收金額" min="0" step="0.01" value="0">
                        <input type="date" id="due-date" required>
                        <button type="submit">添加</button>
                    </div>
                </form>
            </div>
            
            <div class="loans-section">
                <h2>借款列表</h2>
                <table id="loans-table">
                    <thead>
                        <tr>
                            <th>編號</th>
                            <th>客戶名稱</th>
                            <th>客戶電話</th>
                            <th>借款金額</th>
                            <th>實收金額</th>
                            <th>到期日</th>
                            <th>剩餘天數</th>
                            <th>展延操作</th>
                            <th>狀態</th>
                            <th>操作</th>
                        </tr>
                    </thead>
                    <tbody id="loans-body">
                        <!-- 借款記錄將在這裡動態添加 -->
                    </tbody>
                </table>
            </div>
        </div>
        
        <div id="monthly-tab" style="display: none;">
            <div class="period-selector">
                <select id="month-select" onchange="loadMonthlyStats()">
                    <!-- 月份選項將動態生成 -->
                </select>
                <select id="year-select" onchange="loadMonthlyStats()">
                    <!-- 年份選項將動態生成 -->
                </select>
            </div>
            <div class="stats-section">
                <h2>月度統計</h2>
                <div class="stats-grid">
                    <div class="stat-card">
                        <h3>總借款金額</h3>
                        <p id="monthly-total-loan">$0</p>
                    </div>
                    <div class="stat-card">
                        <h3>總實收金額</h3>
                        <p id="monthly-total-received">$0</p>
                    </div>
                    <div class="stat-card">
                        <h3>借款筆數</h3>
                        <p id="monthly-loan-count">0</p>
                    </div>
                    <div class="stat-card">
                        <h3>已清償筆數</h3>
                        <p id="monthly-paid-count">0</p>
                    </div>
                </div>
            </div>
            <table id="monthly-table">
                <thead>
                    <tr>
                        <th>編號</th>
                        <th>客戶名稱</th>
                        <th>客戶電話</th>
                        <th>借款金額</th>
                        <th>實收金額</th>
                        <th>到期日</th>
                        <th>狀態</th>
                    </tr>
                </thead>
                <tbody id="monthly-body">
                    <!-- 月度借款記錄將在這裡動態添加 -->
                </tbody>
            </table>
        </div>
        
        <div id="yearly-tab" style="display: none;">
            <div class="period-selector">
                <select id="year-select-yearly" onchange="loadYearlyStats()">
                    <!-- 年份選項將動態生成 -->
                </select>
            </div>
            <div class="stats-section">
                <h2>年度統計</h2>
                <div class="stats-grid">
                    <div class="stat-card">
                        <h3>總借款金額</h3>
                        <p id="yearly-total-loan">$0</p>
                    </div>
                    <div class="stat-card">
                        <h3>總實收金額</h3>
                        <p id="yearly-total-received">$0</p>
                    </div>
                    <div class="stat-card">
                        <h3>借款筆數</h3>
                        <p id="yearly-loan-count">0</p>
                    </div>
                    <div class="stat-card">
                        <h3>已清償筆數</h3>
                        <p id="yearly-paid-count">0</p>
                    </div>
                </div>
            </div>
            <table id="yearly-table">
                <thead>
                    <tr>
                        <th>月份</th>
                        <th>借款金額</th>
                        <th>實收金額</th>
                        <th>借款筆數</th>
                        <th>已清償筆數</th>
                    </tr>
                </thead>
                <tbody id="yearly-body">
                    <!-- 年度統計將在這裡動態添加 -->
                </tbody>
            </table>
        </div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            // 從本地存儲加載數據
            let loans = JSON.parse(localStorage.getItem('loans')) || [];
            let history = JSON.parse(localStorage.getItem('loanHistory')) || [];
            let monthlyStats = JSON.parse(localStorage.getItem('monthlyStats')) || {};
            let yearlyStats = JSON.parse(localStorage.getItem('yearlyStats')) || {};
            
            // 設置默認到期日為30天後
            const defaultDueDate = new Date();
            defaultDueDate.setDate(defaultDueDate.getDate() + 30);
            document.getElementById('due-date').valueAsDate = defaultDueDate;
            
            // 初始化UI
            updateLoansTable();
            updateStats();
            initPeriodSelectors();
            
            // 表單提交事件
            document.getElementById('loan-form').addEventListener('submit', function(e) {
                e.preventDefault();
                
                // 獲取表單數據
                const loanId = document.getElementById('loan-id').value;
                const customerName = document.getElementById('customer-name').value;
                const customerPhone = document.getElementById('customer-phone').value;
                const loanAmount = parseFloat(document.getElementById('loan-amount').value);
                const receivedAmount = parseFloat(document.getElementById('received-amount').value) || 0;
                const dueDate = document.getElementById('due-date').value;
                
                // 驗證數據
                if (!loanId || !customerName || isNaN(loanAmount) || !dueDate) {
                    alert('請填寫所有必填欄位');
                    return;
                }
                
                // 創建新借款記錄
                const loan = {
                    id: Date.now(),
                    loanId,
                    customerName,
                    customerPhone,
                    loanAmount,
                    receivedAmount,
                    dueDate,
                    extendedDays: 0,
                    status: 'active', // active, paid
                    history: [],
                    createdDate: new Date().toISOString()
                };
                
                // 添加到借款數組
                loans.push(loan);
                
                // 更新統計數據
                updateAllStats(loan, 'add');
                
                // 保存到本地存儲
                saveAllData();
                
                // 更新UI
                updateLoansTable();
                updateStats();
                
                // 重置表單並設置默認到期日
                this.reset();
                document.getElementById('due-date').valueAsDate = defaultDueDate;
            });
            
            // 處理展延、清償、恢復和刪除操作
            document.getElementById('loans-body').addEventListener('click', function(e) {
                const row = e.target.closest('tr');
                if (!row) return;
                
                const id = parseInt(row.getAttribute('data-id'));
                const loan = loans.find(l => l.id === id);
                if (!loan) return;
                
                if (e.target.classList.contains('extend-btn')) {
                    const extendDays = parseInt(row.querySelector('.extend-input').value) || 30; // 默認30天
                    const paymentAmount = parseFloat(row.querySelector('.payment-input').value) || 0;
                    
                    if (extendDays > 0) {
                        // 記錄歷史狀態
                        addToHistory(loan, 'extend', extendDays);
                        
                        // 更新實收金額
                        loan.receivedAmount += paymentAmount;
                        
                        // 展延天數
                        loan.extendedDays += extendDays;
                        
                        // 更新統計數據
                        updateAllStats(loan, 'extend');
                        
                        saveAllData();
                        updateLoansTable();
                        updateStats();
                    }
                }
                
                if (e.target.classList.contains('pay-btn')) {
                    // 記錄歷史狀態
                    addToHistory(loan, 'pay');
                    
                    loan.status = 'paid';
                    
                    // 更新統計數據
                    updateAllStats(loan, 'pay');
                    
                    saveAllData();
                    updateLoansTable();
                    updateStats();
                }
                
                if (e.target.classList.contains('restore-btn')) {
                    // 恢復到上一個狀態
                    restoreFromHistory(loan);
                    
                    // 重新計算所有統計數據
                    recalculateAllStats();
                    
                    saveAllData();
                    updateLoansTable();
                    updateStats();
                }
                
                if (e.target.classList.contains('delete-btn')) {
                    if (confirm('確定要刪除此借款記錄嗎？')) {
                        // 記錄歷史狀態以便可能的恢復
                        addToHistory(loan, 'delete');
                        
                        loans = loans.filter(l => l.id !== id);
                        
                        // 更新統計數據
                        updateAllStats(loan, 'delete');
                        
                        saveAllData();
                        updateLoansTable();
                        updateStats();
                    }
                }
            });
            
            // 處理實收金額編輯
            document.getElementById('loans-body').addEventListener('change', function(e) {
                if (e.target.classList.contains('received-input')) {
                    const row = e.target.closest('tr');
                    if (!row) return;
                    
                    const id = parseInt(row.getAttribute('data-id'));
                    const loan = loans.find(l => l.id === id);
                    if (!loan) return;
                    
                    const newAmount = parseFloat(e.target.value) || 0;
                    
                    // 記錄歷史狀態
                    addToHistory(loan, 'edit_received', newAmount - loan.receivedAmount);
                    
                    // 更新實收金額
                    loan.receivedAmount = newAmount;
                    
                    // 更新統計數據
                    updateAllStats(loan, 'edit_received');
                    
                    saveAllData();
                    updateStats();
                }
            });
            
            // 更新所有相關統計數據
            function updateAllStats(loan, action) {
                const date = new Date(loan.createdDate || new Date());
                const year = date.getFullYear();
                const month = date.getMonth() + 1;
                const yearMonth = `${year}-${month.toString().padStart(2, '0')}`;
                
                // 確保月度統計存在
                if (!monthlyStats[yearMonth]) {
                    monthlyStats[yearMonth] = {
                        totalLoan: 0,
                        totalReceived: 0,
                        loanCount: 0,
                        paidCount: 0,
                        loans: []
                    };
                }
                
                // 根據操作類型更新統計
                switch(action) {
                    case 'add':
                        monthlyStats[yearMonth].totalLoan += loan.loanAmount;
                        monthlyStats[yearMonth].totalReceived += loan.receivedAmount || 0;
                        monthlyStats[yearMonth].loanCount++;
                        if (loan.status === 'paid') monthlyStats[yearMonth].paidCount++;
                        monthlyStats[yearMonth].loans.push(loan);
                        break;
                        
                    case 'extend':
                    case 'edit_received':
                        // 重新計算實收金額總和
                        monthlyStats[yearMonth].totalReceived = monthlyStats[yearMonth].loans.reduce((sum, l) => {
                            if (l.id === loan.id) {
                                return sum + loan.receivedAmount;
                            }
                            return sum + (l.receivedAmount || 0);
                        }, 0);
                        break;
                        
                    case 'pay':
                        // 更新已清償筆數
                        monthlyStats[yearMonth].paidCount = monthlyStats[yearMonth].loans.filter(l => l.status === 'paid').length;
                        break;
                        
                    case 'delete':
                        monthlyStats[yearMonth].loans = monthlyStats[yearMonth].loans.filter(l => l.id !== loan.id);
                        monthlyStats[yearMonth].totalLoan = monthlyStats[yearMonth].loans.reduce((sum, l) => sum + l.loanAmount, 0);
                        monthlyStats[yearMonth].totalReceived = monthlyStats[yearMonth].loans.reduce((sum, l) => sum + (l.receivedAmount || 0), 0);
                        monthlyStats[yearMonth].loanCount = monthlyStats[yearMonth].loans.length;
                        monthlyStats[yearMonth].paidCount = monthlyStats[yearMonth].loans.filter(l => l.status === 'paid').length;
                        break;
                }
                
                // 重新計算年度統計
                updateYearlyStatsFromMonthly();
            }
            
            // 從月度統計更新年度統計
            function updateYearlyStatsFromMonthly() {
                yearlyStats = {};
                
                // 遍歷所有月度統計
                for (const [yearMonth, stats] of Object.entries(monthlyStats)) {
                    const year = yearMonth.split('-')[0];
                    const month = parseInt(yearMonth.split('-')[1]);
                    
                    if (!yearlyStats[year]) {
                        yearlyStats[year] = {
                            totalLoan: 0,
                            totalReceived: 0,
                            loanCount: 0,
                            paidCount: 0,
                            months: {}
                        };
                    }
                    
                    // 更新年度總計
                    yearlyStats[year].totalLoan = Object.entries(monthlyStats)
                        .filter(([ym, _]) => ym.startsWith(year))
                        .reduce((sum, [_, s]) => sum + s.totalLoan, 0);
                    
                    yearlyStats[year].totalReceived = Object.entries(monthlyStats)
                        .filter(([ym, _]) => ym.startsWith(year))
                        .reduce((sum, [_, s]) => sum + s.totalReceived, 0);
                    
                    yearlyStats[year].loanCount = Object.entries(monthlyStats)
                        .filter(([ym, _]) => ym.startsWith(year))
                        .reduce((sum, [_, s]) => sum + s.loanCount, 0);
                    
                    yearlyStats[year].paidCount = Object.entries(monthlyStats)
                        .filter(([ym, _]) => ym.startsWith(year))
                        .reduce((sum, [_, s]) => sum + s.paidCount, 0);
                    
                    // 更新月份數據
                    yearlyStats[year].months[month] = {
                        totalLoan: stats.totalLoan,
                        totalReceived: stats.totalReceived,
                        loanCount: stats.loanCount,
                        paidCount: stats.paidCount
                    };
                }
            }
            
            // 重新計算所有統計數據
            function recalculateAllStats() {
                monthlyStats = {};
                yearlyStats = {};
                
                // 重新建立統計數據
                loans.forEach(loan => {
                    const date = new Date(loan.createdDate || new Date());
                    const year = date.getFullYear();
                    const month = date.getMonth() + 1;
                    const yearMonth = `${year}-${month.toString().padStart(2, '0')}`;
                    
                    if (!monthlyStats[yearMonth]) {
                        monthlyStats[yearMonth] = {
                            totalLoan: 0,
                            totalReceived: 0,
                            loanCount: 0,
                            paidCount: 0,
                            loans: []
                        };
                    }
                    
                    monthlyStats[yearMonth].totalLoan += loan.loanAmount;
                    monthlyStats[yearMonth].totalReceived += loan.receivedAmount || 0;
                    monthlyStats[yearMonth].loanCount++;
                    if (loan.status === 'paid') monthlyStats[yearMonth].paidCount++;
                    monthlyStats[yearMonth].loans.push(loan);
                });
                
                // 更新年度統計
                updateYearlyStatsFromMonthly();
            }
            
            // 保存所有數據到本地存儲
            function saveAllData() {
                localStorage.setItem('loans', JSON.stringify(loans));
                localStorage.setItem('loanHistory', JSON.stringify(history));
                localStorage.setItem('monthlyStats', JSON.stringify(monthlyStats));
                localStorage.setItem('yearlyStats', JSON.stringify(yearlyStats));
            }
            
            // 添加操作到歷史記錄
            function addToHistory(loan, action, value = 0) {
                const historyEntry = {
                    id: loan.id,
                    timestamp: new Date().toISOString(),
                    action,
                    data: {
                        extendedDays: loan.extendedDays,
                        status: loan.status,
                        receivedAmount: loan.receivedAmount,
                        loanAmount: loan.loanAmount
                    },
                    value,
                    loanData: JSON.parse(JSON.stringify(loan)) // 保存完整的借款數據
                };
                
                history.push(historyEntry);
            }
            
            // 從歷史記錄恢復
            function restoreFromHistory(loan) {
                // 找到該貸款的最新歷史記錄
                const loanHistory = history.filter(h => h.id === loan.id).sort((a, b) => 
                    new Date(b.timestamp) - new Date(a.timestamp));
                
                if (loanHistory.length > 0) {
                    const lastAction = loanHistory[0];
                    
                    if (lastAction.action === 'delete') {
                        // 如果是刪除操作，恢復整個借款記錄
                        loans.push(lastAction.loanData);
                    } else {
                        // 恢復數據
                        loan.extendedDays = lastAction.data.extendedDays;
                        loan.status = lastAction.data.status;
                        loan.receivedAmount = lastAction.data.receivedAmount;
                    }
                    
                    // 從歷史中刪除這條記錄
                    history = history.filter(h => h !== lastAction);
                }
            }
            
            // 初始化時間選擇器
            function initPeriodSelectors() {
                // 初始化月份選擇器
                const monthSelect = document.getElementById('month-select');
                const yearSelect = document.getElementById('year-select');
                const yearSelectYearly = document.getElementById('year-select-yearly');
                
                // 清空現有選項
                monthSelect.innerHTML = '';
                yearSelect.innerHTML = '';
                yearSelectYearly.innerHTML = '';
                
                // 添加月份選項
                for (let i = 1; i <= 12; i++) {
                    const option = document.createElement('option');
                    option.value = i;
                    option.textContent = `${i}月`;
                    monthSelect.appendChild(option);
                }
                
                // 添加年份選項 (過去5年到今年)
                const currentYear = new Date().getFullYear();
                for (let i = currentYear; i >= currentYear - 5; i--) {
                    const option = document.createElement('option');
                    option.value = i;
                    option.textContent = `${i}年`;
                    yearSelect.appendChild(option.cloneNode(true));
                    yearSelectYearly.appendChild(option);
                }
                
                // 設置默認選中當前年月
                const currentMonth = new Date().getMonth() + 1;
                monthSelect.value = currentMonth;
                yearSelect.value = currentYear;
                yearSelectYearly.value = currentYear;
            }
            
            // 加載月度統計
            window.loadMonthlyStats = function() {
                const month = document.getElementById('month-select').value;
                const year = document.getElementById('year-select').value;
                const yearMonth = `${year}-${month.toString().padStart(2, '0')}`;
                
                const stats = monthlyStats[yearMonth] || {
                    totalLoan: 0,
                    totalReceived: 0,
                    loanCount: 0,
                    paidCount: 0,
                    loans: []
                };
                
                // 更新統計卡片
                document.getElementById('monthly-total-loan').textContent = `$${stats.totalLoan.toLocaleString()}`;
                document.getElementById('monthly-total-received').textContent = `$${stats.totalReceived.toLocaleString()}`;
                document.getElementById('monthly-loan-count').textContent = stats.loanCount;
                document.getElementById('monthly-paid-count').textContent = stats.paidCount;
                
                // 更新表格
                const tbody = document.getElementById('monthly-body');
                tbody.innerHTML = '';
                
                stats.loans.forEach(loan => {
                    const dueDate = new Date(loan.dueDate);
                    dueDate.setDate(dueDate.getDate() + loan.extendedDays);
                    
                    const row = document.createElement('tr');
                    row.innerHTML = `
                        <td>${loan.loanId}</td>
                        <td>${loan.customerName}</td>
                        <td>${loan.customerPhone || '-'}</td>
                        <td>$${loan.loanAmount.toLocaleString()}</td>
                        <td>$${loan.receivedAmount.toLocaleString()}</td>
                        <td>${formatDate(dueDate)}</td>
                        <td>${loan.status === 'paid' ? '已清償' : '未清償'}</td>
                    `;
                    tbody.appendChild(row);
                });
            };
            
            // 加載年度統計
            window.loadYearlyStats = function() {
                const year = document.getElementById('year-select-yearly').value;
                
                const stats = yearlyStats[year] || {
                    totalLoan: 0,
                    totalReceived: 0,
                    loanCount: 0,
                    paidCount: 0,
                    months: {}
                };
                
                // 更新統計卡片
                document.getElementById('yearly-total-loan').textContent = `$${stats.totalLoan.toLocaleString()}`;
                document.getElementById('yearly-total-received').textContent = `$${stats.totalReceived.toLocaleString()}`;
                document.getElementById('yearly-loan-count').textContent = stats.loanCount;
                document.getElementById('yearly-paid-count').textContent = stats.paidCount;
                
                // 更新表格
                const tbody = document.getElementById('yearly-body');
                tbody.innerHTML = '';
                
                for (let month = 1; month <= 12; month++) {
                    const monthStats = stats.months[month] || {
                        totalLoan: 0,
                        totalReceived: 0,
                        loanCount: 0,
                        paidCount: 0
                    };
                    
                    const row = document.createElement('tr');
                    row.innerHTML = `
                        <td>${year}年${month}月</td>
                        <td>$${monthStats.totalLoan.toLocaleString()}</td>
                        <td>$${monthStats.totalReceived.toLocaleString()}</td>
                        <td>${monthStats.loanCount}</td>
                        <td>${monthStats.paidCount}</td>
                    `;
                    tbody.appendChild(row);
                }
            };
            
            // 切換標籤頁
            window.showTab = function(tabName) {
                document.getElementById('current-tab').style.display = tabName === 'current' ? 'block' : 'none';
                document.getElementById('monthly-tab').style.display = tabName === 'monthly' ? 'block' : 'none';
                document.getElementById('yearly-tab').style.display = tabName === 'yearly' ? 'block' : 'none';
                
                // 更新標籤按鈕樣式
                document.querySelectorAll('.tab-button').forEach(btn => {
                    btn.classList.remove('active');
                });
                event.target.classList.add('active');
                
                // 如果是統計標籤，加載數據
                if (tabName === 'monthly') loadMonthlyStats();
                if (tabName === 'yearly') loadYearlyStats();
            };
            
            // 更新借款表格
            function updateLoansTable() {
                const tbody = document.getElementById('loans-body');
                tbody.innerHTML = '';
                
                // 按到期日排序 (即將到期的排在最上面)
                loans.sort((a, b) => {
                    if (a.status === 'paid' && b.status !== 'paid') return 1;
                    if (a.status !== 'paid' && b.status === 'paid') return -1;
                    
                    const dateA = new Date(a.dueDate);
                    const dateB = new Date(b.dueDate);
                    
                    // 考慮展延天數
                    dateA.setDate(dateA.getDate() + a.extendedDays);
                    dateB.setDate(dateB.getDate() + b.extendedDays);
                    
                    return dateA - dateB;
                });
                
                const today = new Date();
                today.setHours(0, 0, 0, 0);
                
                loans.forEach(loan => {
                    const dueDate = new Date(loan.dueDate);
                    dueDate.setDate(dueDate.getDate() + loan.extendedDays);
                    
                    const timeDiff = dueDate - today;
                    const daysRemaining = Math.ceil(timeDiff / (1000 * 60 * 60 * 24));
                    
                    const row = document.createElement('tr');
                    row.setAttribute('data-id', loan.id);
                    
                    // 根據狀態和剩餘天數添加樣式
                    if (loan.status === 'paid') {
                        row.classList.add('paid');
                    } else if (daysRemaining < 0) {
                        row.classList.add('overdue');
                    } else if (daysRemaining <= 7) {
                        row.classList.add('due-soon');
                    }
                    
                    row.innerHTML = `
                        <td>${loan.loanId}</td>
                        <td>${loan.customerName}</td>
                        <td>${loan.customerPhone || '-'}</td>
                        <td>$${loan.loanAmount.toLocaleString()}</td>
                        <td><input type="number" class="received-input" value="${loan.receivedAmount}" min="0" step="0.01"></td>
                        <td>${formatDate(dueDate)}</td>
                        <td>${loan.status === 'paid' ? '已清償' : (daysRemaining >= 0 ? daysRemaining + '天' : '已過期')}</td>
                        <td>
                            <input type="number" class="payment-input" min="0" step="0.01" placeholder="金額" ${loan.status === 'paid' ? 'disabled' : ''}>
                            <input type="number" class="extend-input" min="1" value="30" ${loan.status === 'paid' ? 'disabled' : ''}>
                            <button class="extend-btn" ${loan.status === 'paid' ? 'disabled' : ''}>展延</button>
                        </td>
                        <td>${loan.status === 'paid' ? '已清償' : '未清償'}</td>
                        <td>
                            <div class="action-buttons">
                                <button class="pay-btn" ${loan.status === 'paid' ? 'disabled' : ''}>清償</button>
                                <button class="restore-btn">恢復</button>
                                <button class="delete-btn">刪除</button>
                            </div>
                        </td>
                    `;
                    
                    tbody.appendChild(row);
                });
            }
            
            // 更新統計數據
            function updateStats() {
                const today = new Date();
                today.setHours(0, 0, 0, 0);
                
                let totalLoanAmount = 0;
                let totalReceivedAmount = 0;
                let dueSoonCount = 0;
                let overdueCount = 0;
                
                loans.forEach(loan => {
                    totalLoanAmount += loan.loanAmount;
                    totalReceivedAmount += loan.receivedAmount || 0;
                    
                    if (loan.status !== 'paid') {
                        const dueDate = new Date(loan.dueDate);
                        dueDate.setDate(dueDate.getDate() + loan.extendedDays);
                        
                        const timeDiff = dueDate - today;
                        const daysRemaining = Math.ceil(timeDiff / (1000 * 60 * 60 * 24));
                        
                        if (daysRemaining < 0) {
                            overdueCount++;
                        } else if (daysRemaining <= 7) {
                            dueSoonCount++;
                        }
                    }
                });
                
                document.getElementById('total-loan-amount').textContent = `$${totalLoanAmount.toLocaleString()}`;
                document.getElementById('total-received-amount').textContent = `$${totalReceivedAmount.toLocaleString()}`;
                document.getElementById('due-soon-count').textContent = dueSoonCount;
                document.getElementById('overdue-count').textContent = overdueCount;
            }
            
            // 格式化日期為 YYYY-MM-DD
            function formatDate(date) {
                const d = new Date(date);
                const year = d.getFullYear();
                const month = String(d.getMonth() + 1).padStart(2, '0');
                const day = String(d.getDate()).padStart(2, '0');
                return `${year}-${month}-${day}`;
            }
        });
    </script>
</body>
</html># .github.io-
