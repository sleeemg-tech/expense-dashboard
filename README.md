<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>داشبورد المصروفات والمبيعات - إبداع الروؤ</title>
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <style>
    body { font-family: Arial, sans-serif; background: #f9fafb; margin: 20px; }
    .container { max-width: 900px; margin: auto; background: white; padding: 20px; border-radius: 10px; box-shadow: 0 0 5px #ccc; }
    input, select, button { padding: 8px; margin: 5px 0; width: 100%; }
    table { width: 100%; border-collapse: collapse; margin-top: 15px; }
    th, td { border: 1px solid #ddd; padding: 8px; text-align: center; }
    th { background: #eee; }
    .flex { display: flex; gap: 10px; }
    .flex > * { flex: 1; }
  </style>
</head>
<body>
  <div class="container">
    <h1>داشبورد المصروفات والمبيعات — إبداع الروؤ</h1>

    <section>
      <h2>إضافة مصروف</h2>
      <input id="expenseRef" placeholder="رقم الصرف" />
      <input id="expenseAmount" type="number" placeholder="المبلغ" />
      <input id="expenseCompany" placeholder="اسم الشركة" />
      <input id="expenseDate" type="date" />
      <label><input id="expensePaid" type="checkbox" checked /> تم الصرف</label>
      <button onclick="addExpense()">حفظ المصروف</button>
    </section>

    <section>
      <h2>إضافة مبيعات شهرية</h2>
      <input id="saleMonth" type="month" />
      <input id="saleCompany" placeholder="اسم الشركة" />
      <input id="saleAmount" type="number" placeholder="مبلغ المبيعات" />
      <button onclick="addSale()">حفظ المبيعات</button>
    </section>

    <section>
      <h2>البيانات</h2>
      <button onclick="exportData()">تصدير البيانات (تحميل JSON)</button>
      <input type="file" id="importFile" accept=".json" />
      <button onclick="importData()">استيراد البيانات</button>
    </section>

    <section>
      <h2>قائمة المصروفات</h2>
      <table id="expensesTable">
        <thead>
          <tr>
            <th>رقم الصرف</th><th>المبلغ</th><th>الشركة</th><th>التاريخ</th><th>الحالة</th>
          </tr>
        </thead>
        <tbody></tbody>
      </table>
    </section>

    <section>
      <h2>قائمة المبيعات</h2>
      <table id="salesTable">
        <thead>
          <tr>
            <th>الشهر</th><th>الشركة</th><th>المبلغ</th>
          </tr>
        </thead>
        <tbody></tbody>
      </table>
    </section>

    <section>
      <h2>إجماليات</h2>
      <p id="totalExpenses">إجمالي المصروفات: 0</p>
      <p id="totalSales">إجمالي المبيعات: 0</p>
    </section>

  </div>

  <script>
    let data = {
      expenses: [],
      sales: []
    };

    function render() {
      // جدول المصروفات
      const expensesBody = document.querySelector("#expensesTable tbody");
      expensesBody.innerHTML = "";
      data.expenses.forEach(e => {
        const tr = document.createElement("tr");
        tr.innerHTML = `
          <td>${e.refNo}</td>
          <td>${e.amount.toLocaleString()}</td>
          <td>${e.company}</td>
          <td>${e.date}</td>
          <td>${e.paid ? "تم" : "لم يتم"}</td>
        `;
        expensesBody.appendChild(tr);
      });

      // جدول المبيعات
      const salesBody = document.querySelector("#salesTable tbody");
      salesBody.innerHTML = "";
      data.sales.forEach(s => {
        const tr = document.createElement("tr");
        tr.innerHTML = `
          <td>${s.month}</td>
          <td>${s.company}</td>
          <td>${s.amount.toLocaleString()}</td>
        `;
        salesBody.appendChild(tr);
      });

      // إجماليات
      const totalExp = data.expenses.reduce((a,b) => a + b.amount, 0);
      const totalSal = data.sales.reduce((a,b) => a + b.amount, 0);
      document.getElementById("totalExpenses").textContent = "إجمالي المصروفات: " + totalExp.toLocaleString();
      document.getElementById("totalSales").textContent = "إجمالي المبيعات: " + totalSal.toLocaleString();
    }

    function addExpense() {
      const refNo = document.getElementById("expenseRef").value.trim();
      const amount = parseFloat(document.getElementById("expenseAmount").value);
      const company = document.getElementById("expenseCompany").value.trim();
      const date = document.getElementById("expenseDate").value;
      const paid = document.getElementById("expensePaid").checked;

      if (!refNo || !amount || !company || !date) {
        alert("يرجى تعبئة كل حقول المصروف.");
        return;
      }
      data.expenses.push({ refNo, amount, company, date, paid });
      clearExpenseForm();
      render();
    }

    function clearExpenseForm() {
      document.getElementById("expenseRef").value = "";
      document.getElementById("expenseAmount").value = "";
      document.getElementById("expenseCompany").value = "";
      document.getElementById("expenseDate").value = "";
      document.getElementById("expensePaid").checked = true;
    }

    function addSale() {
      const month = document.getElementById("saleMonth").value;
      const company = document.getElementById("saleCompany").value.trim();
      const amount = parseFloat(document.getElementById("saleAmount").value);

      if (!month || !company || !amount) {
        alert("يرجى تعبئة كل حقول المبيعات.");
        return;
      }
      data.sales.push({ month, company, amount });
      clearSaleForm();
      render();
    }

    function clearSaleForm() {
      document.getElementById("saleMonth").value = "";
      document.getElementById("saleCompany").value = "";
      document.getElementById("saleAmount").value = "";
    }

    function exportData() {
      const blob = new Blob([JSON.stringify(data, null, 2)], { type: "application/json" });
      const url = URL.createObjectURL(blob);
      const a = document.createElement("a");
      a.href = url;
      a.download = "dashboard_data.json";
      a.click();
      URL.revokeObjectURL(url);
      alert("تم تحميل نسخة من البيانات يمكنك رفعها في المجلد المشترك.");
    }

    function importData() {
      const fileInput = document.getElementById("importFile");
      if (!fileInput.files.length) {
        alert("اختر ملف JSON للاستيراد.");
        return;
      }
      const file = fileInput.files[0];
      const reader = new FileReader();
      reader.onload = function(e) {
        try {
          const imported = JSON.parse(e.target.result);
          if (imported.expenses && imported.sales) {
            data = imported;
            render();
            alert("تم استيراد البيانات بنجاح.");
          } else {
            alert("الملف غير صالح.");
          }
        } catch {
          alert("فشل في قراءة الملف.");
        }
      };
      reader.readAsText(file);
    }

    // عرض مبدئي
    render();
  </script>
</body>
</html>
