<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ระบบแดชบอร์ดเฝ้าระวังทางระบาดวิทยา</title>
    <!-- Bootstrap 5 CSS -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
    <!-- Leaflet CSS -->
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Sarabun:wght@300;400;600&display=swap" rel="stylesheet">
    
    <style>
        body {
            font-family: 'Sarabun', sans-serif;
            background-color: #0f172a;
            color: #f8fafc;
        }
        .navbar-custom {
            background-color: #1e293b;
            border-bottom: 1px solid #334155;
        }
        .card {
            background-color: #1e293b;
            color: #f8fafc;
            border: 1px solid #334155;
            border-radius: 10px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.3);
            margin-bottom: 20px;
        }
        .kpi-card {
            border-left: 5px solid;
        }
        .kpi-primary { border-left-color: #3b82f6; }
        .kpi-warning { border-left-color: #f59e0b; }
        .kpi-danger { border-left-color: #ef4444; }
        .kpi-success { border-left-color: #10b981; }
        #map {
            height: 400px;
            width: 100%;
            border-radius: 8px;
            background-color: #262626;
        }
        .table-dark-custom {
            color: #cbd5e1;
            background-color: #1e293b;
        }
        .table-dark-custom td, .table-dark-custom th {
            border-color: #334155;
        }
    </style>
</head>
<body>

    <!-- Navbar -->
    <nav class="navbar navbar-dark navbar-custom mb-4">
        <div class="container-fluid">
            <span class="navbar-brand mb-0 h1 text-info fw-bold">
                📊 ระบบเฝ้าระวังและวิเคราะห์ข้อมูลทางระบาดวิทยา (Epidemiology Dashboard)
            </span>
        </div>
    </nav>

    <div class="container-fluid px-4">
        <!-- KPI Row -->
        <div class="row">
            <div class="col-xl-3 col-md-6">
                <div class="card kpi-card kpi-primary p-3">
                    <div class="text-slate-400 small text-uppercase">ผู้ป่วยสะสมทั้งหมด</div>
                    <div class="h3 fw-bold mb-0 text-white" id="totalCases">1,245</div>
                    <small class="text-danger">↑ 8.2% จากเดือนที่แล้ว</small>
                </div>
            </div>
            <div class="col-xl-3 col-md-6">
                <div class="card kpi-card kpi-warning p-3">
                    <div class="text-slate-400 small text-uppercase">อัตราป่วย (ต่อแสนประชากร)</div>
                    <div class="h3 fw-bold mb-0 text-white">185.4</div>
                    <small class="text-muted">เกณฑ์เฝ้าระวังระดับปานกลาง</small>
                </div>
            </div>
            <div class="col-xl-3 col-md-6">
                <div class="card kpi-card kpi-danger p-3">
                    การระบุคำสั่ง (Prompt) เพื่อใช้แผนที่ฐาน **Esri World Dark Gray Canvas** ในการสร้างแผนที่บนแดชบอร์ด สามารถเลือกใช้โครงสร้างพรอมต์ตามรูปแบบการนำไปใช้งานได้ดังนี้:

### 1. พรอมต์สำหรับออกแบบแดชบอร์ด (AI / UI Design Prompt)
> "สร้างหน้าแดชบอร์ดแสดงผลข้อมูลเชิงพื้นที่ โดยกำหนดแผนที่ฐาน (Basemap) เป็น **Esri World Dark Gray Canvas** คุมโทนแดชบอร์ดแบบ Dark Mode และเลือกใช้สีสัญลักษณ์ข้อมูล (Symbology) โทนสว่างหรือสีนีออน (เช่น ส้มสด, ฟ้าสว่าง) เพื่อให้ข้อมูลมีความเด่นชัดตัดกับพื้นหลังสีเทาดำ"

---

### 2. พรอมต์สำหรับสร้างโค้ด / พัฒนาแอปพลิเคชัน (Developer Prompt)
> "เขียนโค้ดแสดงผลแผนที่บนแดชบอร์ดด้วย [ระบุ Library เช่น Leaflet / ArcGIS Maps SDK for JavaScript] โดยกำหนด Basemap เป็น **Esri World Dark Gray Canvas** 
> - Base Tile URL: `https://services.arcgisonline.com/arcgis/rest/services/Canvas/World_Dark_Gray_Base/MapServer`
> - Reference Overlay URL: `https://services.arcgisonline.com/arcgis/rest/services/Canvas/World_Dark_Gray_Reference/MapServer`
> ปรับสไตล์ป๊อปอัพและคอนโทรลเลอร์ให้เข้ากับธีมโทนเข้ม"

---

### ข้อมูลทางเทคนิคสำหรับนำไปอ้างอิง (Technical Specification)

* **Basemap ID (ArcGIS SDKs):** `dark-gray-vector` หรือ `dark-gray`
* **ผู้ให้บริการ:** Esri (ArcGIS Online)
* **การใช้งานที่เหมาะสม:** เหมาะสำหรับแดชบอร์ดที่ต้องการเน้นความโดดเด่นของชั้นข้อมูล (Data Layer) โดยไม่ให้รายละเอียดภูมิประเทศหรือถนนบนแผนที่ฐานมาบดบังสายตา
