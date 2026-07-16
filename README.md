<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Working Time Tracker - Management</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            padding: 20px;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
        }

        header {
            background: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
            margin-bottom: 30px;
        }

        h1 {
            color: #333;
            margin-bottom: 10px;
        }

        .header-info {
            color: #666;
            font-size: 14px;
        }

        .main-content {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 30px;
            margin-bottom: 30px;
        }

        @media (max-width: 768px) {
            .main-content {
                grid-template-columns: 1fr;
            }
        }

        .card {
            background: white;
            padding: 25px;
            border-radius: 10px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
        }

        .card h2 {
            color: #333;
            margin-bottom: 20px;
            font-size: 20px;
            border-bottom: 2px solid #667eea;
            padding-bottom: 10px;
        }

        .form-group {
            margin-bottom: 20px;
        }

        label {
            display: block;
            margin-bottom: 8px;
            color: #333;
            font-weight: 500;
        }

        input[type="text"],
        input[type="date"],
        input[type="time"],
        input[type="number"],
        select {
            width: 100%;
            padding: 12px;
            border: 1px solid #ddd;
            border-radius: 5px;
            font-size: 14px;
            font-family: inherit;
            transition: border-color 0.3s;
        }

        input[type="text"]:focus,
        input[type="date"]:focus,
        input[type="time"]:focus,
        input[type="number"]:focus,
        select:focus {
            outline: none;
            border-color: #667eea;
            box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.1);
        }

        .button-group {
            display: flex;
            gap: 10px;
            margin-top: 20px;
        }

        button {
            padding: 12px 20px;
            border: none;
            border-radius: 5px;
            font-size: 14px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s;
            flex: 1;
        }

        .btn-primary {
            background: #667eea;
            color: white;
        }

        .btn-primary:hover {
            background: #5568d3;
            transform: translateY(-2px);
            box-shadow: 0 4px 12px rgba(102, 126, 234, 0.4);
        }

        .btn-danger {
            background: #f56565;
            color: white;
        }

        .btn-danger:hover {
            background: #e53e3e;
        }

        .btn-success {
            background: #48bb78;
            color: white;
        }

        .btn-success:hover {
            background: #38a169;
        }

        .btn-small {
            padding: 8px 12px;
            font-size: 12px;
            flex: none;
        }

        .employees-list {
            display: flex;
            flex-direction: column;
            gap: 15px;
        }

        .employee-card {
            background: #f9fafb;
            padding: 15px;
            border-radius: 8px;
            border-left: 4px solid #667eea;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .employee-info {
            flex: 1;
        }

        .employee-name {
            font-weight: 600;
            color: #333;
            margin-bottom: 5px;
        }

        .employee-hours {
            font-size: 24px;
            color: #667eea;
            font-weight: bold;
            margin-bottom: 5px;
        }

        .employee-status {
            font-size: 12px;
            color: #666;
        }

        .status-working {
            color: #48bb78;
            font-weight: 600;
        }

        .status-idle {
            color: #ed8936;
            font-weight: 600;
        }

        .employee-actions {
            display: flex;
            gap: 8px;
        }

        .badge {
            display: inline-block;
            padding: 4px 8px;
            border-radius: 4px;
            font-size: 12px;
            font-weight: 600;
            margin-right: 8px;
        }

        .badge-active {
            background: #c6f6d5;
            color: #22543d;
        }

        .badge-inactive {
            background: #fed7d7;
            color: #742a2a;
        }

        .table-container {
            overflow-x: auto;
        }

        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
        }

        table thead {
            background: #f9fafb;
        }

        table th {
            padding: 12px;
            text-align: left;
            color: #333;
            font-weight: 600;
            border-bottom: 2px solid #e2e8f0;
        }

        table td {
            padding: 12px;
            border-bottom: 1px solid #e2e8f0;
        }

        table tr:hover {
            background: #f9fafb;
        }

        .modal {
            display: none;
            position: fixed;
            z-index: 1000;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0,0,0,0.5);
        }

        .modal.active {
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .modal-content {
            background-color: white;
            padding: 30px;
            border-radius: 10px;
            max-width: 500px;
            width: 90%;
            box-shadow: 0 10px 40px rgba(0,0,0,0.3);
        }

        .modal-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
        }

        .modal-header h2 {
            color: #333;
        }

        .close-btn {
            background: none;
            border: none;
            font-size: 28px;
            cursor: pointer;
            color: #666;
            padding: 0;
            width: auto;
        }

        .close-btn:hover {
            color: #333;
        }

        .stats-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 15px;
            margin-top: 20px;
        }

        .stat-box {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 20px;
            border-radius: 8px;
            text-align: center;
        }

        .stat-value {
            font-size: 32px;
            font-weight: bold;
            margin-bottom: 5px;
        }

        .stat-label {
            font-size: 14px;
            opacity: 0.9;
        }

        .alert {
            padding: 15px;
            border-radius: 5px;
            margin-bottom: 20px;
            display: none;
        }

        .alert.active {
            display: block;
        }

        .alert-success {
            background: #c6f6d5;
            color: #22543d;
            border-left: 4px solid #48bb78;
        }

        .alert-error {
            background: #fed7d7;
            color: #742a2a;
            border-left: 4px solid #f56565;
        }

        .time-display {
            font-family: 'Courier New', monospace;
            font-size: 18px;
            color: #667eea;
            font-weight: bold;
            margin: 10px 0;
        }

        .tabs {
            display: flex;
            gap: 10px;
            margin-bottom: 20px;
            border-bottom: 2px solid #e2e8f0;
        }

        .tab-btn {
            background: none;
            border: none;
            padding: 10px 20px;
            font-size: 14px;
            font-weight: 600;
            color: #666;
            cursor: pointer;
            border-bottom: 3px solid transparent;
            margin-bottom: -2px;
            transition: all 0.3s;
        }

        .tab-btn.active {
            color: #667eea;
            border-bottom-color: #667eea;
        }

        .tab-content {
            display: none;
        }

        .tab-content.active {
            display: block;
        }
    </style>
</head>
<body>
    <div class="container">
        <!-- Header -->
        <header>
            <h1>⏱️ Working Time Tracker</h1>
            <p class="header-info">Manage employee working hours and track time entries</p>
        </header>

        <!-- Alert Messages -->
        <div id="alert" class="alert"></div>

        <!-- Main Content -->
        <div class="main-content">
            <!-- Add/Track Employee Time -->
            <div class="card">
                <h2>Track Time</h2>
                
                <div class="form-group">
                    <label>Employee Name</label>
                    <input type="text" id="employeeName" placeholder="e.g., John Doe">
                </div>

                <div class="form-group">
                    <label>Work Date</label>
                    <input type="date" id="workDate">
                </div>

                <div class="form-group">
                    <label>Start Time</label>
                    <input type="time" id="startTime">
                </div>

                <div class="form-group">
                    <label>End Time</label>
                    <input type="time" id="endTime">
                </div>

                <div class="form-group">
                    <label>Task/Description</label>
                    <input type="text" id="taskDescription" placeholder="Optional: What did they work on?">
                </div>

                <div class="button-group">
                    <button class="btn-primary" onclick="addTimeEntry()">Add Entry</button>
                    <button class="btn-success" onclick="startTracking()">Start Tracking</button>
                </div>
            </div>

            <!-- Employee Summary -->
            <div class="card">
                <h2>Employee Summary</h2>
                <div class="employees-list" id="employeesList"></div>
            </div>
        </div>

        <!-- Statistics -->
        <div class="card">
            <h2>Statistics</h2>
            <div class="stats-grid">
                <div class="stat-box">
                    <div class="stat-value" id="totalEmployees">0</div>
                    <div class="stat-label">Active Employees</div>
                </div>
                <div class="stat-box">
                    <div class="stat-value" id="totalHours">0h</div>
                    <div class="stat-label">Total Hours Tracked</div>
                </div>
                <div class="stat-box">
                    <div class="stat-value" id="avgHours">0h</div>
                    <div class="stat-label">Average Hours</div>
                </div>
                <div class="stat-box">
                    <div class="stat-value" id="activeNow">0</div>
                    <div class="stat-label">Working Now</div>
                </div>
            </div>
        </div>

        <!-- Time Entries Table -->
        <div class="card" style="margin-top: 30px;">
            <h2>Time Entries</h2>
            
            <div class="tabs">
                <button class="tab-btn active" onclick="switchTab('all')">All Entries</button>
                <button class="tab-btn" onclick="switchTab('today')">Today</button>
                <button class="tab-btn" onclick="switchTab('weekly')">This Week</button>
            </div>

            <div id="all" class="tab-content active">
                <div class="table-container">
                    <table id="entriesTable">
                        <thead>
                            <tr>
                                <th>Employee</th>
                                <th>Date</th>
                                <th>Start Time</th>
                                <th>End Time</th>
                                <th>Hours</th>
                                <th>Task</th>
                                <th>Actions</th>
                            </tr>
                        </thead>
                        <tbody id="entriesBody"></tbody>
                    </table>
                </div>
            </div>

            <div id="today" class="tab-content">
                <div class="table-container">
                    <table id="todayTable">
                        <thead>
                            <tr>
                                <th>Employee</th>
                                <th>Start Time</th>
                                <th>End Time</th>
                                <th>Hours</th>
                                <th>Status</th>
                                <th>Actions</th>
                            </tr>
                        </thead>
                        <tbody id="todayBody"></tbody>
                    </table>
                </div>
            </div>

            <div id="weekly" class="tab-content">
                <div class="table-container">
                    <table id="weeklyTable">
                        <thead>
                            <tr>
                                <th>Employee</th>
                                <th>Mon</th>
                                <th>Tue</th>
                                <th>Wed</th>
                                <th>Thu</th>
                                <th>Fri</th>
                                <th>Sat</th>
                                <th>Sun</th>
                                <th>Total</th>
                            </tr>
                        </thead>
                        <tbody id="weeklyBody"></tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>

    <!-- Edit Modal -->
    <div id="editModal" class="modal">
        <div class="modal-content">
            <div class="modal-header">
                <h2>Edit Time Entry</h2>
                <button class="close-btn" onclick="closeEditModal()">&times;</button>
            </div>
            <div class="form-group">
                <label>Employee Name</label>
                <input type="text" id="editEmployeeName" readonly>
            </div>
            <div class="form-group">
                <label>Date</label>
                <input type="date" id="editDate">
            </div>
            <div class="form-group">
                <label>Start Time</label>
                <input type="time" id="editStartTime">
            </div>
            <div class="form-group">
                <label>End Time</label>
                <input type="time" id="editEndTime">
            </div>
            <div class="form-group">
                <label>Task/Description</label>
                <input type="text" id="editTask">
            </div>
            <div class="button-group">
                <button class="btn-primary" onclick="saveEdit()">Save Changes</button>
                <button class="btn-danger" onclick="closeEditModal()">Cancel</button>
            </div>
        </div>
    </div>

    <script>
        // Data Storage
        let timeEntries = JSON.parse(localStorage.getItem('timeEntries')) || [];
        let activeTracking = JSON.parse(localStorage.getItem('activeTracking')) || {};
        let editingIndex = null;

        // Set today's date as default
        document.getElementById('workDate').valueAsDate = new Date();

        // Update UI
        function updateUI() {
            updateEmployeesList();
            updateStatistics();
            updateEntriesTable();
            saveData();
        }

        // Add Time Entry
        function addTimeEntry() {
            const employeeName = document.getElementById('employeeName').value.trim();
            const workDate = document.getElementById('workDate').value;
            const startTime = document.getElementById('startTime').value;
            const endTime = document.getElementById('endTime').value;
            const taskDescription = document.getElementById('taskDescription').value;

            if (!employeeName || !workDate || !startTime || !endTime) {
                showAlert('Please fill in all required fields', 'error');
                return;
            }

            const start = new Date(`${workDate}T${startTime}`);
            const end = new Date(`${workDate}T${endTime}`);

            if (end <= start) {
                showAlert('End time must be after start time', 'error');
                return;
            }

            const hours = (end - start) / (1000 * 60 * 60);

            timeEntries.push({
                id: Date.now(),
                employeeName,
                date: workDate,
                startTime,
                endTime,
                hours: hours.toFixed(2),
                task: taskDescription,
                createdAt: new Date().toISOString()
            });

            showAlert(`Time entry added for ${employeeName} (${hours.toFixed(2)} hours)`, 'success');
            
            // Clear form
            document.getElementById('employeeName').value = '';
            document.getElementById('startTime').value = '';
            document.getElementById('endTime').value = '';
            document.getElementById('taskDescription').value = '';
            document.getElementById('workDate').valueAsDate = new Date();

            updateUI();
        }

        // Start Tracking Live
        function startTracking() {
            const employeeName = document.getElementById('employeeName').value.trim();
            
            if (!employeeName) {
                showAlert('Please enter employee name', 'error');
                return;
            }

            if (activeTracking[employeeName]) {
                showAlert(`Already tracking time for ${employeeName}`, 'error');
                return;
            }

            activeTracking[employeeName] = {
                startTime: new Date().toISOString(),
                date: new Date().toISOString().split('T')[0]
            };

            showAlert(`Started tracking time for ${employeeName}`, 'success');
            updateUI();
        }

        // Stop Tracking
        function stopTracking(employeeName) {
            if (!activeTracking[employeeName]) {
                showAlert('No active tracking for this employee', 'error');
                return;
            }

            const startIso = activeTracking[employeeName].startTime;
            const start = new Date(startIso);
            const end = new Date();

            const startTime = start.toTimeString().slice(0, 5);
            const endTime = end.toTimeString().slice(0, 5);
            const date = activeTracking[employeeName].date;

            timeEntries.push({
                id: Date.now(),
                employeeName,
                date,
                startTime,
                endTime,
                hours: ((end - start) / (1000 * 60 * 60)).toFixed(2),
                task: 'Auto-tracked',
                createdAt: new Date().toISOString()
            });

            delete activeTracking[employeeName];
            showAlert(`Stopped tracking for ${employeeName}`, 'success');
            updateUI();
        }

        // Update Employees List
        function updateEmployeesList() {
            const employeesList = document.getElementById('employeesList');
            const employees = {};

            // Group entries by employee
            timeEntries.forEach(entry => {
                if (!employees[entry.employeeName]) {
                    employees[entry.employeeName] = {
                        totalHours: 0,
                        entries: 0,
                        isActive: !!activeTracking[entry.employeeName]
                    };
                }
                employees[entry.employeeName].totalHours += parseFloat(entry.hours);
                employees[entry.employeeName].entries += 1;
            });

            let html = '';
            Object.entries(employees).forEach(([name, data]) => {
                const statusClass = data.isActive ? 'status-working' : 'status-idle';
                const statusText = data.isActive ? '● Working' : '● Idle';
                const badge = data.isActive ? 'badge-active' : 'badge-inactive';

                html += `
                    <div class="employee-card">
                        <div class="employee-info">
                            <div class="employee-name">${name}</div>
                            <div class="employee-hours">${data.totalHours.toFixed(2)}h</div>
                            <div class="employee-status">
                                <span class="badge ${badge}">${statusText}</span>
                                ${data.entries} entries
                            </div>
                        </div>
                        <div class="employee-actions">
                            ${data.isActive ? 
                                `<button class="btn-danger btn-small" onclick="stopTracking('${name}')">Stop</button>` :
                                `<button class="btn-success btn-small" onclick="resumeTracking('${name}')">Resume</button>`
                            }
                        </div>
                    </div>
                `;
            });

            employeesList.innerHTML = html || '<p style="color: #999;">No employees yet</p>';
        }

        // Resume Tracking
        function resumeTracking(employeeName) {
            document.getElementById('employeeName').value = employeeName;
            document.getElementById('employeeName').focus();
            startTracking();
        }

        // Update Entries Table
        function updateEntriesTable() {
            const tbody = document.getElementById('entriesBody');
            let html = '';

            const sorted = [...timeEntries].sort((a, b) => new Date(b.date) - new Date(a.date));

            sorted.forEach((entry, index) => {
                html += `
                    <tr>
                        <td>${entry.employeeName}</td>
                        <td>${formatDate(entry.date)}</td>
                        <td>${entry.startTime}</td>
                        <td>${entry.endTime}</td>
                        <td><strong>${entry.hours}h</strong></td>
                        <td>${entry.task || '-'}</td>
                        <td>
                            <button class="btn-primary btn-small" onclick="openEditModal(${index})">Edit</button>
                            <button class="btn-danger btn-small" onclick="deleteEntry(${index})">Delete</button>
                        </td>
                    </tr>
                `;
            });

            tbody.innerHTML = html || '<tr><td colspan="7" style="text-align: center; color: #999;">No entries yet</td></tr>';

            // Update today's table
            updateTodayTable();
            // Update weekly table
            updateWeeklyTable();
        }

        // Update Today's Table
        function updateTodayTable() {
            const tbody = document.getElementById('todayBody');
            const today = new Date().toISOString().split('T')[0];
            const todayEntries = timeEntries.filter(e => e.date === today);

            let html = '';
            todayEntries.forEach(entry => {
                const isActive = activeTracking[entry.employeeName] ? '🟢 Active' : '⏹️ Completed';
                html += `
                    <tr>
                        <td>${entry.employeeName}</td>
                        <td>${entry.startTime}</td>
                        <td>${entry.endTime}</td>
                        <td><strong>${entry.hours}h</strong></td>
                        <td>${isActive}</td>
                        <td>
                            <button class="btn-primary btn-small" onclick="openEditModal(${timeEntries.indexOf(entry)})">Edit</button>
                        </td>
                    </tr>
                `;
            });

            document.getElementById('todayBody').innerHTML = html || '<tr><td colspan="6" style="text-align: center; color: #999;">No entries for today</td></tr>';
        }

        // Update Weekly Table
        function updateWeeklyTable() {
            const tbody = document.getElementById('weeklyBody');
            const employees = {};

            // Group by employee
            timeEntries.forEach(entry => {
                if (!employees[entry.employeeName]) {
                    employees[entry.employeeName] = {
                        Mon: 0, Tue: 0, Wed: 0, Thu: 0, Fri: 0, Sat: 0, Sun: 0
                    };
                }
                const date = new Date(entry.date);
                const day = date.toLocaleDateString('en-US', { weekday: 'short' });
                employees[entry.employeeName][day] += parseFloat(entry.hours);
            });

            let html = '';
            Object.entries(employees).forEach(([name, hours]) => {
                const total = Object.values(hours).reduce((a, b) => a + b, 0);
                html += `
                    <tr>
                        <td>${name}</td>
                        <td>${(hours.Mon || 0).toFixed(2)}</td>
                        <td>${(hours.Tue || 0).toFixed(2)}</td>
                        <td>${(hours.Wed || 0).toFixed(2)}</td>
                        <td>${(hours.Thu || 0).toFixed(2)}</td>
                        <td>${(hours.Fri || 0).toFixed(2)}</td>
                        <td>${(hours.Sat || 0).toFixed(2)}</td>
                        <td>${(hours.Sun || 0).toFixed(2)}</td>
                        <td><strong>${total.toFixed(2)}h</strong></td>
                    </tr>
                `;
            });

            document.getElementById('weeklyBody').innerHTML = html || '<tr><td colspan="9" style="text-align: center; color: #999;">No entries this week</td></tr>';
        }

        // Update Statistics
        function updateStatistics() {
            const employees = new Set(timeEntries.map(e => e.employeeName));
            const totalHours = timeEntries.reduce((sum, e) => sum + parseFloat(e.hours), 0);
            const avgHours = employees.size > 0 ? (totalHours / employees.size).toFixed(2) : 0;
            const activeCount = Object.keys(activeTracking).length;

            document.getElementById('totalEmployees').textContent = employees.size;
            document.getElementById('totalHours').textContent = totalHours.toFixed(1) + 'h';
            document.getElementById('avgHours').textContent = avgHours + 'h';
            document.getElementById('activeNow').textContent = activeCount;
        }

        // Edit Entry
        function openEditModal(index) {
            const entry = timeEntries[index];
            editingIndex = index;

            document.getElementById('editEmployeeName').value = entry.employeeName;
            document.getElementById('editDate').value = entry.date;
            document.getElementById('editStartTime').value = entry.startTime;
            document.getElementById('editEndTime').value = entry.endTime;
            document.getElementById('editTask').value = entry.task;

            document.getElementById('editModal').classList.add('active');
        }

        function closeEditModal() {
            document.getElementById('editModal').classList.remove('active');
            editingIndex = null;
        }

        function saveEdit() {
            if (editingIndex === null) return;

            const date = document.getElementById('editDate').value;
            const startTime = document.getElementById('editStartTime').value;
            const endTime = document.getElementById('editEndTime').value;
            const task = document.getElementById('editTask').value;

            const start = new Date(`${date}T${startTime}`);
            const end = new Date(`${date}T${endTime}`);

            if (end <= start) {
                showAlert('End time must be after start time', 'error');
                return;
            }

            const hours = (end - start) / (1000 * 60 * 60);

            timeEntries[editingIndex] = {
                ...timeEntries[editingIndex],
                date,
                startTime,
                endTime,
                hours: hours.toFixed(2),
                task
            };

            showAlert('Entry updated successfully', 'success');
            closeEditModal();
            updateUI();
        }

        // Delete Entry
        function deleteEntry(index) {
            if (confirm('Are you sure you want to delete this entry?')) {
                timeEntries.splice(index, 1);
                showAlert('Entry deleted successfully', 'success');
                updateUI();
            }
        }

        // Switch Tab
        function switchTab(tabName) {
            document.querySelectorAll('.tab-btn').forEach(btn => btn.classList.remove('active'));
            document.querySelectorAll('.tab-content').forEach(tab => tab.classList.remove('active'));

            event.target.classList.add('active');
            document.getElementById(tabName).classList.add('active');
        }

        // Show Alert
        function showAlert(message, type) {
            const alert = document.getElementById('alert');
            alert.textContent = message;
            alert.className = `alert active alert-${type}`;
            setTimeout(() => alert.classList.remove('active'), 3000);
        }

        // Format Date
        function formatDate(dateStr) {
            return new Date(dateStr).toLocaleDateString('en-US', { 
                month: 'short', 
                day: 'numeric', 
                year: 'numeric' 
            });
        }

        // Save Data to localStorage
        function saveData() {
            localStorage.setItem('timeEntries', JSON.stringify(timeEntries));
            localStorage.setItem('activeTracking', JSON.stringify(activeTracking));
        }

        // Close modal on outside click
        document.getElementById('editModal').addEventListener('click', (e) => {
            if (e.target.id === 'editModal') closeEditModal();
        });

        // Initial UI Update
        updateUI();

        // Update active tracking every second
        setInterval(() => {
            if (Object.keys(activeTracking).length > 0) {
                updateEmployeesList();
            }
        }, 1000);
    </script>
</body>
</html>
