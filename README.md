<html>
<head>
<title>តារាងបម្លែងថ្ងៃខែឆ្នាំជាមួយប៊ូតុង</title>
<style>
  /* CSS សម្រាប់រចនាតារាង */
  body {
    font-family: 'Khmer OS Siemreap', Arial, sans-serif;
    padding: 20px;
  }
  table {
    width: 100%;
    border-collapse: collapse;
    margin-top: 15px;
  }
  th, td {
    border: 1px solid #ddd;
    padding: 12px;
    text-align: left;
  }
  th {
    background-color: #007bff;
    color: white;
  }
  button {
    padding: 10px 20px;
    font-size: 16px;
    cursor: pointer;
    background-color: #28a745;
    color: white;
    border: none;
    border-radius: 5px;
  }
  button:hover {
    background-color: #218838;
  }
</style>
</head>
<body>

  <h2>🔄 បម្លែងទម្រង់កាលបរិច្ឆេទក្នុងតារាង</h2>
  <p>ចុចប៊ូតុងខាងក្រោមដើម្បីផ្លាស់ប្តូរទម្រង់ថ្ងៃខែឆ្នាំ:</p>

  <button id="toggleFormatButton" onclick="toggleDateFormat()">
    ប្ដូរទៅទម្រង់: ខែ/ថ្ងៃ/ឆ្នាំ
  </button>

  <table id="dateTable">
    <thead>
      <tr>
        <th>ឈ្មោះគម្រោង</th>
        <th>កាលបរិច្ឆេទដើម (ISO)</th>
        <th>កាលបរិច្ឆេទដែលបានបម្លែង</th>
      </tr>
    </thead>
    <tbody>
      </tbody>
  </table>

  <script>
    // ទិន្នន័យកាលបរិច្ឆេទដើម (ត្រូវបានរក្សាទុកជា String សម្រាប់ងាយស្រួល)
    const projectData = [
      { name: "គម្រោង A", date: "2025-01-15" },
      { name: "គម្រោង B", date: "2024-11-02" },
      { name: "គម្រោង C", date: "2023-05-20" },
      { name: "គម្រោង D", date: "2025-10-28" }
    ];

    // ស្ថានភាពបច្ចុប្បន្ននៃទម្រង់កាលបរិច្ឆេទ
    // 'DMY' = ថ្ងៃ/ខែ/ឆ្នាំ (DD/MM/YYYY)
    // 'MDY' = ខែ/ថ្ងៃ/ឆ្នាំ (MM/DD/YYYY)
    let currentFormat = 'DMY';

    // មុខងារសម្រាប់បម្លែងកាលបរិច្ឆេទទៅជាទម្រង់ជាក់លាក់
    function formatDate(isoDateString, formatType) {
        const dateObject = new Date(isoDateString);
        if (isNaN(dateObject)) return isoDateString; // ប្រសិនបើកាលបរិច្ឆេទមិនត្រឹមត្រូវ គឺបង្ហាញកាលបរិច្ឆេទដើម

        if (formatType === 'MDY') {
            // ទម្រង់: ខែ/ថ្ងៃ/ឆ្នាំ (MM/DD/YYYY) - ប្រើ en-US
            return dateObject.toLocaleDateString('en-US', {
                year: 'numeric',
                month: '2-digit',
                day: '2-digit'
            });
        } else { // លំនាំដើមគឺ 'DMY'
            // ទម្រង់: ថ្ងៃ/ខែ/ឆ្នាំ (DD/MM/YYYY) - ប្រើ km-KH ឬ en-GB
            return dateObject.toLocaleDateString('en-GB', {
                year: 'numeric',
                month: '2-digit',
                day: '2-digit'
            });
        }
    }

    // មុខងារសម្រាប់ផ្ទុកទិន្នន័យទៅក្នុងតារាង
    function renderTable() {
      const tableBody = document.querySelector('#dateTable tbody');
      tableBody.innerHTML = ''; // លុបទិន្នន័យចាស់

      projectData.forEach(item => {
        const formattedDate = formatDate(item.date, currentFormat);

        // បង្កើតជួរដេកថ្មី (<tr>)
        const newRow = tableBody.insertRow();
        
        // ក្រឡា 1: ឈ្មោះគម្រោង
        newRow.insertCell(0).textContent = item.name;
        
        // ក្រឡា 2: កាលបរិច្ឆេទដើម
        newRow.insertCell(1).textContent = item.date;
        
        // ក្រឡា 3: កាលបរិច្ឆេទដែលបានបម្លែង (ផ្លាស់ប្តូរតាមប៊ូតុង)
        newRow.insertCell(2).textContent = formattedDate;
        
        // រក្សាទុកកាលបរិច្ឆេទដើមនៅក្នុង HTML element (attribute) ដើម្បីងាយស្រួលបម្លែងឡើងវិញ
        newRow.cells[2].setAttribute('data-original-date', item.date);
      });
      
      // ផ្លាស់ប្តូរអត្ថបទលើប៊ូតុង
      const button = document.getElementById('toggleFormatButton');
      if (currentFormat === 'DMY') {
        button.textContent = 'ប្ដូរទៅទម្រង់: ខែ/ថ្ងៃ/ឆ្នាំ';
      } else {
        button.textContent = 'ប្ដូរទៅទម្រង់: ថ្ងៃ/ខែ/ឆ្នាំ';
      }
    }

    // មុខងារសម្រាប់ផ្លាស់ប្តូរទម្រង់កាលបរិច្ឆេទនៅពេលចុចប៊ូតុង
    function toggleDateFormat() {
      // ផ្លាស់ប្តូរស្ថានភាពបច្ចុប្បន្ន
      currentFormat = (currentFormat === 'DMY') ? 'MDY' : 'DMY';
      
      // ផ្ទុកទិន្នន័យក្នុងតារាងឡើងវិញជាមួយនឹងទម្រង់ថ្មី
      renderTable();
    }

    // ហៅមុខងារដើម្បីផ្ទុកតារាងលើកដំបូង
    renderTable();

  </script>

</body>

</html>
