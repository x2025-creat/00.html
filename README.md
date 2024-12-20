<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>قائمة المشتريات</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 20px;
      padding: 0;
      background-color: #f8f9fa;
    }
    h1 {
      text-align: center;
      color: #333;
    }
    #container {
      max-width: 500px;
      margin: 0 auto;
      background: #fff;
      padding: 20px;
      border-radius: 8px;
      box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    }
    .item {
      display: flex;
      align-items: center;
      margin-bottom: 10px;
    }
    .item input[type="text"] {
      flex: 1;
      padding: 5px;
      margin-right: 10px;
      border: 1px solid #ccc;
      border-radius: 4px;
    }
    .item input[type="checkbox"] {
      margin-right: 10px;
    }
    .buttons {
      display: flex;
      gap: 5px;
      margin-top: 10px;
    }
    button {
      padding: 5px 10px;
      border: none;
      border-radius: 4px;
      cursor: pointer;
    }
    button.add {
      background-color: #28a745;
      color: white;
    }
    button.delete {
      background-color: #dc3545;
      color: white;
    }
    button.save {
      background-color: #007bff;
      color: white;
    }
    button.show-summary {
      background-color: #ffc107;
      color: black;
      margin-top: 10px;
    }
    .summary {
      margin-top: 20px;
      padding: 10px;
      background-color: #f0f0f0;
      border-radius: 5px;
    }
  </style>
</head>
<body>
  <h1>قائمة المشتريات</h1>
  <div id="container">
    <button class="add">+ إضافة صنف</button>
    <div id="list"></div>
    <button class="save">💾 حفظ القائمة</button>
    <button class="show-summary">📋 عرض القائمة النهائية</button>
    <div class="summary" id="summary"></div>
  </div>

  <script>
    const container = document.getElementById("list");
    const addButton = document.querySelector(".add");
    const saveButton = document.querySelector(".save");
    const showSummaryButton = document.querySelector(".show-summary");
    const summaryDiv = document.getElementById("summary");

    // تحميل البيانات المحفوظة
    const loadItems = () => {
      const savedItems = JSON.parse(localStorage.getItem("shoppingList")) || [];
      savedItems.forEach(({ text, checked }) => addItem(text, checked));
    };

    // إضافة عنصر جديد
    const addItem = (text = "", checked = false) => {
      const itemDiv = document.createElement("div");
      itemDiv.className = "item";

      const checkbox = document.createElement("input");
      checkbox.type = "checkbox";
      checkbox.checked = checked;

      const input = document.createElement("input");
      input.type = "text";
      input.value = text;

      const deleteButton = document.createElement("button");
      deleteButton.textContent = "حذف";
      deleteButton.className = "delete";
      deleteButton.onclick = () => container.removeChild(itemDiv);

      itemDiv.appendChild(checkbox);
      itemDiv.appendChild(input);
      itemDiv.appendChild(deleteButton);

      container.appendChild(itemDiv);
    };

    // حفظ البيانات في Local Storage
    const saveItems = () => {
      const items = Array.from(container.querySelectorAll(".item")).map((item) => ({
        text: item.querySelector("input[type='text']").value,
        checked: item.querySelector("input[type='checkbox']").checked,
      }));
      localStorage.setItem("shoppingList", JSON.stringify(items));
      alert("تم حفظ القائمة!");
    };

    // عرض القائمة النهائية
    const showSummary = () => {
      const items = Array.from(container.querySelectorAll(".item")).map((item) => ({
        text: item.querySelector("input[type='text']").value,
        checked: item.querySelector("input[type='checkbox']").checked,
      }));

      const ownedItems = items.filter(item => item.checked).map(item => item.text);
      const neededItems = items.filter(item => !item.checked).map(item => item.text);

      summaryDiv.innerHTML = `
        <h3>📋 القائمة النهائية:</h3><strong>✅ الأشياء التي تملكها:</strong>
        <ul>${ownedItems.map(item => `<li>${item}</li>`).join("")}</ul>
        <strong>❌ الأشياء التي تحتاجها:</strong>
        <ul>${neededItems.map(item => `<li>${item}</li>`).join("")}</ul>
      `;
    };

    // الأحداث
    addButton.onclick = () => addItem();
    saveButton.onclick = saveItems;
    showSummaryButton.onclick = showSummary;

    // تحميل العناصر عند فتح الصفحة
    window.onload = loadItems;
  </script>
</body>
</html>
