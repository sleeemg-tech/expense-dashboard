<!DOCTYPE html>
<html lang="ar">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>داشبورد مصروفات شركة إبداع الرؤى</title>
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <header>
    <h1>داشبورد مصروفات شركة إبداع الرؤى</h1>
  </header>

  <section id="input-section">
    <h2>إضافة مصروف جديد</h2>
    <form id="expense-form">
      <label>رقم الصرف: <input type="number" id="expense-id" required /></label>
      <label>المبلغ: <input type="number" id="amount" required /></label>
      <label>الشركة: <input type="text" id="company" required /></label>
      <label>تم الصرف: 
        <select id="paid" required>
          <option value="نعم">نعم</option>
          <option value="لا">لا</option>
        </select>
      </label>
      <label>التاريخ: <input type="date" id="date" required /></label>
      <button type="submit">إضافة</button>
    </form>
  </section>

  <section id="search-section">
    <h2>البحث في المصروفات</h2>
    <input type="text" id="search-input" placeholder="ابحث برقم الصرف أو اسم الشركة" />
  </section>

  <section id="dashboard-section">
    <h2>البيانات</h2>
    <table id="expenses-table" border="1" cellspacing="0" cellpadding="5">
      <thead>
        <tr>
          <th>رقم الصرف</th>
          <th>المبلغ</th>
          <th>الشركة</th>
          <th>تم الصرف</th>
          <th>التاريخ</th>
        </tr>
      </thead>
      <tbody></tbody>
    </table>

    <div id="summary">
      <h3>الإجماليات</h3>
      <p id="total-expenses">إجمالي المصروفات: 0</p>
      <p id="total-by-company"></p>
      <p id="total-by-month"></p>
    </div>
  </section>
