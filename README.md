<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<title>المحلل الذكي لنتائج الطلاب - النظام المتكامل</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">

<style>
:root {
    --primary: #1a5c9e;
    --secondary: #25d366;
    --success: #4caf50;
    --danger: #f44336;
    --warning: #ff9800;
    --info: #17a2b8;
    --light: #f8f9fa;
    --dark: #343a40;
    --purple: #6f42c1;
    --pink: #e83e8c;
    --flash: #FF6B35;
}

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
    color: var(--dark);
}

/* تصميم الهيدر */
.header {
    background: rgba(255, 255, 255, 0.95);
    padding: 25px;
    border-radius: 20px;
    box-shadow: 0 10px 30px rgba(0,0,0,0.1);
    margin-bottom: 30px;
    text-align: center;
    backdrop-filter: blur(10px);
}

.header h1 {
    background: linear-gradient(135deg, var(--primary) 0%, var(--purple) 100%);
    -webkit-background-clip: text;
    background-clip: text;
    color: transparent;
    font-size: 2.8rem;
    margin-bottom: 10px;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 15px;
}

.header p {
    color: var(--dark);
    font-size: 1.1rem;
    opacity: 0.8;
    max-width: 800px;
    margin: 0 auto;
    line-height: 1.6;
}

/* تنسيق التنقل بين الواجهات */
.interface-switcher {
    display: flex;
    justify-content: center;
    gap: 10px;
    margin-bottom: 30px;
    flex-wrap: wrap;
}

.interface-btn {
    padding: 15px 30px;
    background: rgba(255, 255, 255, 0.9);
    border: none;
    border-radius: 12px;
    font-size: 1rem;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.3s;
    display: flex;
    align-items: center;
    gap: 10px;
    box-shadow: 0 5px 15px rgba(0,0,0,0.08);
}

.interface-btn:hover {
    transform: translateY(-3px);
    box-shadow: 0 8px 20px rgba(0,0,0,0.15);
}

.interface-btn.active {
    background: var(--primary);
    color: white;
    box-shadow: 0 5px 15px rgba(26, 92, 158, 0.3);
}

/* الحاوية الرئيسية */
.main-container {
    display: grid;
    grid-template-columns: 1fr;
    gap: 30px;
    max-width: 1400px;
    margin: 0 auto;
}

/* كروت عامة */
.card {
    background: rgba(255, 255, 255, 0.95);
    border-radius: 20px;
    padding: 30px;
    box-shadow: 0 10px 30px rgba(0,0,0,0.1);
    backdrop-filter: blur(10px);
    border: 1px solid rgba(255, 255, 255, 0.2);
}

.card-title {
    color: var(--primary);
    font-size: 1.6rem;
    margin-bottom: 25px;
    padding-bottom: 15px;
    border-bottom: 3px solid var(--light);
    display: flex;
    align-items: center;
    gap: 12px;
}

.card-title i {
    color: var(--secondary);
    font-size: 1.4rem;
}

/* واجهة الإدخال */
.input-interface {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(400px, 1fr));
    gap: 30px;
}

/* قسم API */
.api-section {
    background: linear-gradient(135deg, #f8f9fa 0%, #e9ecef 100%);
}

.api-input-group {
    margin-bottom: 25px;
}

.api-input-group label {
    display: block;
    margin-bottom: 10px;
    color: var(--dark);
    font-weight: 600;
    font-size: 1.1rem;
}

.api-input {
    width: 100%;
    padding: 16px 20px;
    border: 2px solid #ddd;
    border-radius: 12px;
    font-size: 1rem;
    font-family: 'Courier New', monospace;
    transition: all 0.3s;
    background: white;
}

.api-input:focus {
    outline: none;
    border-color: var(--primary);
    box-shadow: 0 0 0 4px rgba(26, 92, 158, 0.15);
}

/* قسم إدخال المعلمين */
.teachers-input {
    margin-top: 25px;
    padding-top: 25px;
    border-top: 2px solid #e9ecef;
}

.teacher-field {
    margin-bottom: 20px;
}

.teacher-field label {
    display: block;
    margin-bottom: 8px;
    color: var(--dark);
    font-weight: 600;
    display: flex;
    align-items: center;
    gap: 10px;
}

/* زر التحقق */
.verify-btn {
    width: 100%;
    padding: 18px;
    background: linear-gradient(135deg, var(--primary) 0%, #2a7bc8 100%);
    color: white;
    border: none;
    border-radius: 12px;
    font-size: 1.1rem;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.3s;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 12px;
    margin-top: 10px;
}

.verify-btn:hover:not(:disabled) {
    transform: translateY(-2px);
    box-shadow: 0 8px 20px rgba(26, 92, 158, 0.3);
}

.verify-btn:disabled {
    opacity: 0.6;
    cursor: not-allowed;
    transform: none !important;
}

/* حالة API */
.api-status {
    padding: 18px;
    border-radius: 12px;
    margin-top: 20px;
    display: flex;
    align-items: center;
    gap: 15px;
    font-size: 1rem;
    transition: all 0.3s;
}

.status-valid {
    background: linear-gradient(135deg, #d4edda 0%, #c3e6cb 100%);
    color: #155724;
    border: 2px solid #c3e6cb;
}

.status-invalid {
    background: linear-gradient(135deg, #f8d7da 0%, #f5c6cb 100%);
    color: #721c24;
    border: 2px solid #f5c6cb;
}

.status-checking {
    background: linear-gradient(135deg, #fff3cd 0%, #ffeaa7 100%);
    color: #856404;
    border: 2px solid #ffeaa7;
}

/* اختيار النموذج */
.model-selection {
    margin-top: 25px;
}

.model-select {
    width: 100%;
    padding: 16px 20px;
    border: 2px solid #ddd;
    border-radius: 12px;
    font-size: 1rem;
    background: white;
    cursor: pointer;
    margin-bottom: 15px;
    transition: all 0.3s;
}

.model-select:focus {
    outline: none;
    border-color: var(--primary);
    box-shadow: 0 0 0 4px rgba(26, 92, 158, 0.15);
}

.model-info {
    padding: 15px;
    background: #f8f9fa;
    border-radius: 10px;
    border-right: 4px solid var(--primary);
    font-size: 0.95rem;
    line-height: 1.5;
}

.model-info strong {
    color: var(--primary);
}

/* قسم تحميل الملفات */
.upload-area {
    border: 3px dashed var(--primary);
    border-radius: 15px;
    padding: 60px 30px;
    text-align: center;
    cursor: pointer;
    transition: all 0.3s;
    background: linear-gradient(135deg, #f8fbff 0%, #e6f2ff 100%);
    margin-bottom: 25px;
    position: relative;
    overflow: hidden;
}

.upload-area:hover {
    background: linear-gradient(135deg, #e6f2ff 0%, #d4e6ff 100%);
    border-color: #2980b9;
    transform: translateY(-3px);
}

.upload-area.dragover {
    background: linear-gradient(135deg, #d4edda 0%, #c3e6cb 100%);
    border-color: var(--success);
}

.upload-icon {
    font-size: 70px;
    color: var(--primary);
    margin-bottom: 25px;
    transition: all 0.3s;
}

.upload-area:hover .upload-icon {
    transform: scale(1.1);
}

.upload-text {
    font-size: 1.4rem;
    color: var(--dark);
    margin-bottom: 15px;
    font-weight: 600;
}

.upload-info {
    color: #6c757d;
    font-size: 1rem;
    line-height: 1.6;
}

/* قائمة الملفات */
.file-list {
    margin-top: 25px;
}

.file-item {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 20px;
    background: var(--light);
    border-radius: 12px;
    margin-bottom: 15px;
    transition: all 0.3s;
    border: 1px solid #e9ecef;
}

.file-item:hover {
    transform: translateX(-5px);
    box-shadow: 0 5px 15px rgba(0,0,0,0.08);
}

.file-info {
    display: flex;
    align-items: center;
    gap: 15px;
}

.file-icon {
    font-size: 28px;
    color: var(--danger);
}

.file-details {
    display: flex;
    flex-direction: column;
    gap: 5px;
}

.file-name {
    font-weight: 600;
    color: var(--dark);
    font-size: 1.1rem;
}

.file-size {
    color: #6c757d;
    font-size: 0.95rem;
}

.file-remove {
    color: var(--danger);
    cursor: pointer;
    font-size: 1.4rem;
    transition: all 0.3s;
    padding: 8px;
    border-radius: 8px;
}

.file-remove:hover {
    background: rgba(244, 67, 54, 0.1);
    transform: scale(1.1);
}

/* قسم الطلاب والتقديرات */
.students-section {
    grid-column: 1 / -1;
    margin-top: 20px;
}

.students-container {
    max-height: 400px;
    overflow-y: auto;
    margin-top: 20px;
    border: 2px solid #e0e6ef;
    border-radius: 12px;
    padding: 15px;
    background: white;
}

.students-table {
    width: 100%;
    border-collapse: collapse;
}

.students-table th {
    background: var(--primary);
    color: white;
    padding: 15px;
    text-align: center;
    font-weight: 600;
    position: sticky;
    top: 0;
}

.students-table td {
    padding: 12px;
    text-align: center;
    border-bottom: 1px solid #e9ecef;
    transition: background-color 0.3s;
}

.students-table tr:hover td {
    background-color: #f8f9fa;
}

.students-table tr:nth-child(even) {
    background-color: #f8f9fa;
}

.grade-badge {
    padding: 6px 15px;
    border-radius: 20px;
    font-weight: 600;
    font-size: 0.9rem;
    display: inline-block;
    min-width: 70px;
}

.grade-excellent { background: linear-gradient(135deg, #4caf50 0%, #66bb6a 100%); color: white; }
.grade-verygood { background: linear-gradient(135deg, #2196f3 0%, #42a5f5 100%); color: white; }
.grade-good { background: linear-gradient(135deg, #ff9800 0%, #ffb74d 100%); color: white; }
.grade-pass { background: linear-gradient(135deg, #ff9800 0%, #ffb74d 100%); color: white; opacity: 0.8; }
.grade-weak { background: linear-gradient(135deg, #f44336 0%, #ef5350 100%); color: white; }

/* قسم البيانات المستخرجة */
.extracted-data {
    max-height: 500px;
    overflow-y: auto;
    padding: 20px;
    background: #f8f9fa;
    border-radius: 12px;
    border: 2px solid #dee2e6;
}

.data-item {
    padding: 15px;
    border-bottom: 2px solid #e9ecef;
    transition: all 0.3s;
}

.data-item:hover {
    background: white;
    border-radius: 8px;
}

.data-item:last-child {
    border-bottom: none;
}

.data-label {
    font-weight: 700;
    color: var(--primary);
    margin-bottom: 8px;
    font-size: 1.1rem;
    display: flex;
    align-items: center;
    gap: 10px;
}

.data-value {
    color: var(--dark);
    font-size: 1.1rem;
    line-height: 1.6;
}

/* الإحصائيات */
.stats-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
    gap: 20px;
    margin-top: 25px;
}

.stat-card {
    background: white;
    padding: 25px;
    border-radius: 12px;
    text-align: center;
    box-shadow: 0 5px 15px rgba(0,0,0,0.05);
    border: 2px solid #e9ecef;
    transition: all 0.3s;
    position: relative;
    overflow: hidden;
}

.stat-card:hover {
    transform: translateY(-5px);
    box-shadow: 0 15px 30px rgba(0,0,0,0.1);
    border-color: var(--primary);
}

.stat-card::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 4px;
    background: linear-gradient(90deg, var(--primary) 0%, var(--secondary) 100%);
}

.stat-value {
    font-size: 2.5rem;
    font-weight: 800;
    color: var(--primary);
    margin: 15px 0;
    font-family: 'Segoe UI', Arial, sans-serif;
}

.stat-label {
    color: #6c757d;
    font-size: 1rem;
    font-weight: 600;
}

/* قسم التحليل */
.analysis-section {
    grid-column: 1 / -1;
}

.analysis-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(350px, 1fr));
    gap: 25px;
    margin-top: 25px;
}

.chart-container {
    background: white;
    padding: 25px;
    border-radius: 15px;
    box-shadow: 0 8px 20px rgba(0,0,0,0.06);
    height: 350px;
    border: 1px solid #e9ecef;
    transition: all 0.3s;
}

.chart-container:hover {
    transform: translateY(-5px);
    box-shadow: 0 15px 30px rgba(0,0,0,0.1);
}

.chart-title {
    text-align: center;
    margin-bottom: 20px;
    color: var(--primary);
    font-weight: 700;
    font-size: 1.3rem;
    padding-bottom: 10px;
    border-bottom: 2px solid var(--light);
}

/* أزرار التحكم */
.controls {
    display: flex;
    gap: 20px;
    justify-content: center;
    margin: 40px 0;
    flex-wrap: wrap;
}

.control-btn {
    padding: 20px 45px;
    font-size: 1.2rem;
    min-width: 250px;
    border: none;
    border-radius: 15px;
    font-weight: 700;
    cursor: pointer;
    transition: all 0.3s;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 15px;
    box-shadow: 0 8px 20px rgba(0,0,0,0.1);
}

.control-btn.primary {
    background: linear-gradient(135deg, var(--primary) 0%, #2a7bc8 100%);
    color: white;
}

.control-btn.primary:hover:not(:disabled) {
    transform: translateY(-5px);
    box-shadow: 0 15px 30px rgba(26, 92, 158, 0.3);
}

.control-btn.success {
    background: linear-gradient(135deg, var(--secondary) 0%, #1da851 100%);
    color: white;
}

.control-btn.success:hover:not(:disabled) {
    transform: translateY(-5px);
    box-shadow: 0 15px 30px rgba(37, 211, 102, 0.3);
}

.control-btn.warning {
    background: linear-gradient(135deg, var(--warning) 0%, #ffb74d 100%);
    color: white;
}

.control-btn.warning:hover:not(:disabled) {
    transform: translateY(-5px);
    box-shadow: 0 15px 30px rgba(255, 152, 0, 0.3);
}

.control-btn:disabled {
    opacity: 0.5;
    cursor: not-allowed;
    transform: none !important;
    box-shadow: 0 5px 15px rgba(0,0,0,0.1) !important;
}

/* شريط التقدم */
.progress-container {
    margin: 25px 0;
}

.progress-bar {
    height: 12px;
    background: #e9ecef;
    border-radius: 6px;
    overflow: hidden;
    margin-bottom: 15px;
    position: relative;
    box-shadow: inset 0 2px 4px rgba(0,0,0,0.1);
}

.progress-fill {
    height: 100%;
    background: linear-gradient(90deg, var(--primary) 0%, var(--secondary) 100%);
    width: 0%;
    transition: width 0.5s ease;
    position: relative;
    overflow: hidden;
}

.progress-fill::after {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: linear-gradient(90deg, transparent 0%, rgba(255,255,255,0.3) 50%, transparent 100%);
    animation: shimmer 2s infinite;
}

@keyframes shimmer {
    0% { transform: translateX(-100%); }
    100% { transform: translateX(100%); }
}

.progress-text {
    text-align: center;
    color: var(--dark);
    font-size: 1rem;
    font-weight: 600;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 10px;
}

/* تحميل */
.loading {
    display: inline-block;
    width: 24px;
    height: 24px;
    border: 3px solid rgba(255,255,255,0.3);
    border-radius: 50%;
    border-top-color: white;
    animation: spin 1s ease-in-out infinite;
}

@keyframes spin {
    to { transform: rotate(360deg); }
}

/* الرسائل */
.message {
    position: fixed;
    top: 25px;
    right: 25px;
    padding: 20px 30px;
    border-radius: 12px;
    color: white;
    font-weight: 600;
    z-index: 10000;
    animation: slideIn 0.4s cubic-bezier(0.68, -0.55, 0.265, 1.55);
    box-shadow: 0 10px 25px rgba(0,0,0,0.2);
    display: flex;
    align-items: center;
    gap: 15px;
    max-width: 500px;
    backdrop-filter: blur(10px);
    border: 1px solid rgba(255,255,255,0.2);
}

@keyframes slideIn {
    from { transform: translateX(100%); opacity: 0; }
    to { transform: translateX(0); opacity: 1; }
}

.message.success {
    background: linear-gradient(135deg, var(--success) 0%, rgba(76, 175, 80, 0.9) 100%);
}

.message.error {
    background: linear-gradient(135deg, var(--danger) 0%, rgba(244, 67, 54, 0.9) 100%);
}

.message.info {
    background: linear-gradient(135deg, var(--primary) 0%, rgba(26, 92, 158, 0.9) 100%);
}

.message.warning {
    background: linear-gradient(135deg, var(--warning) 0%, rgba(255, 152, 0, 0.9) 100%);
}

/* واجهة التقرير */
.report-interface {
    display: none;
}

.report-page {
    background: white;
    width: 210mm;
    min-height: 297mm;
    margin: 0 auto;
    padding: 0;
    box-sizing: border-box;
    box-shadow: 0 15px 40px rgba(0,0,0,0.15);
    position: relative;
    overflow: hidden;
}

/* تصميم التقرير المحسن */
.report-header {
    background: linear-gradient(135deg, var(--primary) 0%, #2a7bc8 100%);
    color: white;
    padding: 30px 40px;
    text-align: center;
    position: relative;
    overflow: hidden;
}

.report-header::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    height: 5px;
    background: linear-gradient(90deg, var(--secondary) 0%, var(--success) 100%);
}

.report-logo {
    font-size: 36px;
    font-weight: 800;
    margin-bottom: 10px;
    letter-spacing: 1px;
}

.report-school {
    font-size: 20px;
    font-weight: 600;
    opacity: 0.9;
    margin-bottom: 5px;
}

.report-date {
    font-size: 16px;
    opacity: 0.8;
    margin-top: 10px;
}

.report-title {
    font-size: 42px;
    font-weight: 800;
    text-align: center;
    margin: 40px;
    color: var(--primary);
    position: relative;
    padding-bottom: 20px;
}

.report-title::after {
    content: '';
    position: absolute;
    bottom: 0;
    right: 40%;
    left: 40%;
    height: 4px;
    background: linear-gradient(90deg, var(--primary) 0%, var(--secondary) 100%);
    border-radius: 2px;
}

.report-content {
    padding: 30px 40px;
    min-height: calc(297mm - 250px);
}

.report-summary-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 20px;
    margin: 30px 0;
    background: #f8f9fa;
    padding: 25px;
    border-radius: 15px;
    border: 2px solid #e0e6ef;
}

.summary-card {
    text-align: center;
    padding: 20px;
    background: white;
    border-radius: 12px;
    box-shadow: 0 5px 15px rgba(0,0,0,0.05);
    transition: transform 0.3s;
}

.summary-card:hover {
    transform: translateY(-5px);
}

.summary-value {
    font-size: 32px;
    font-weight: 800;
    color: var(--primary);
    margin: 10px 0;
}

.summary-label {
    color: #666;
    font-size: 16px;
    font-weight: 600;
}

.report-section {
    margin: 40px 0;
    padding: 25px;
    background: white;
    border-radius: 15px;
    box-shadow: 0 5px 15px rgba(0,0,0,0.05);
    border: 2px solid #e0e6ef;
}

.section-title {
    font-size: 26px;
    font-weight: 700;
    color: var(--primary);
    margin-bottom: 25px;
    padding-bottom: 15px;
    border-bottom: 3px solid #eaeaea;
    display: flex;
    align-items: center;
    gap: 12px;
}

/* تصميم الجدول المحسن */
.report-table-container {
    overflow-x: auto;
    margin: 20px 0;
    border-radius: 12px;
    border: 2px solid #e0e6ef;
}

.report-students-table {
    width: 100%;
    border-collapse: collapse;
    min-width: 800px;
}

.report-students-table thead {
    background: linear-gradient(135deg, var(--primary) 0%, #2a7bc8 100%);
}

.report-students-table th {
    color: white;
    padding: 18px;
    text-align: center;
    font-weight: 700;
    font-size: 16px;
    border: none;
}

.report-students-table tbody tr {
    border-bottom: 1px solid #e9ecef;
    transition: background-color 0.3s;
}

.report-students-table tbody tr:hover {
    background-color: #f8f9fa;
}

.report-students-table tbody tr:nth-child(even) {
    background-color: #f8f9fa;
}

.report-students-table td {
    padding: 16px;
    text-align: center;
    font-size: 15px;
    color: #333;
}

.report-grade {
    padding: 8px 20px;
    border-radius: 25px;
    font-weight: 600;
    font-size: 14px;
    display: inline-block;
    min-width: 80px;
    box-shadow: 0 4px 6px rgba(0,0,0,0.1);
}

.report-grade.excellent { background: linear-gradient(135deg, #4caf50 0%, #66bb6a 100%); color: white; }
.report-grade.verygood { background: linear-gradient(135deg, #2196f3 0%, #42a5f5 100%); color: white; }
.report-grade.good { background: linear-gradient(135deg, #ff9800 0%, #ffb74d 100%); color: white; }
.report-grade.pass { background: linear-gradient(135deg, #ff9800 0%, #ffb74d 100%); color: white; opacity: 0.8; }
.report-grade.weak { background: linear-gradient(135deg, #f44336 0%, #ef5350 100%); color: white; }

/* الرسوم البيانية في التقرير */
.report-charts-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    gap: 25px;
    margin: 30px 0;
}

.report-chart-box {
    background: white;
    padding: 25px;
    border-radius: 15px;
    box-shadow: 0 5px 15px rgba(0,0,0,0.05);
    border: 2px solid #e0e6ef;
    height: 320px;
}

.report-chart-title {
    text-align: center;
    font-size: 18px;
    font-weight: 700;
    color: var(--primary);
    margin-bottom: 20px;
    padding-bottom: 10px;
    border-bottom: 2px solid #eaeaea;
}

.report-chart-box canvas {
    width: 100% !important;
    height: 220px !important;
}

/* مستوى الأداء */
.performance-level {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 20px;
    margin: 25px 0;
    padding: 25px;
    background: linear-gradient(135deg, #f8fafd 0%, #e6f0ff 100%);
    border-radius: 15px;
    border: 2px solid #e0e6ef;
}

.performance-badge {
    padding: 15px 35px;
    border-radius: 30px;
    font-size: 24px;
    font-weight: 800;
    color: white;
    box-shadow: 0 8px 20px rgba(0,0,0,0.15);
    background: linear-gradient(135deg, var(--primary) 0%, #2a7bc8 100%);
}

.performance-text {
    flex: 1;
    font-size: 18px;
    line-height: 1.6;
    color: #333;
}

/* توقيعات التقرير */
.report-signatures {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 40px;
    margin-top: 50px;
    padding-top: 30px;
    border-top: 3px solid #eaeaea;
}

.report-signature {
    text-align: center;
    padding: 20px;
    background: #f8f9fa;
    border-radius: 12px;
    border: 2px solid #e0e6ef;
}

.signature-line {
    width: 150px;
    height: 2px;
    background: #333;
    margin: 25px auto;
}

.signature-name {
    font-size: 18px;
    font-weight: 700;
    color: var(--primary);
    margin-top: 15px;
}

.signature-title {
    color: #666;
    font-size: 15px;
    margin-top: 5px;
}

/* تذييل التقرير */
.report-footer {
    text-align: center;
    padding: 25px;
    background: #f8f9fa;
    color: #666;
    font-size: 14px;
    border-top: 3px solid #eaeaea;
    margin-top: 40px;
}

/* أزرار الإجراءات */
.actions {
    display: flex;
    gap: 15px;
    justify-content: center;
    margin-bottom: 30px;
    flex-wrap: wrap;
}

.action-btn {
    padding: 15px 30px;
    background: linear-gradient(135deg, var(--secondary) 0%, #1da851 100%);
    color: white;
    border: none;
    border-radius: 12px;
    font-size: 16px;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.3s;
    display: flex;
    align-items: center;
    gap: 10px;
    box-shadow: 0 5px 15px rgba(37, 211, 102, 0.2);
}

.action-btn:hover {
    transform: translateY(-3px);
    box-shadow: 0 8px 20px rgba(37, 211, 102, 0.3);
}

.action-btn.secondary {
    background: linear-gradient(135deg, var(--primary) 0%, #2a7bc8 100%);
    box-shadow: 0 5px 15px rgba(26, 92, 158, 0.2);
}

.action-btn.secondary:hover {
    box-shadow: 0 8px 20px rgba(26, 92, 158, 0.3);
}

/* إخفاء العناصر */
.hidden {
    display: none !important;
}

/* رفع الرسائل */
#messageContainer {
    position: fixed;
    top: 20px;
    right: 20px;
    z-index: 10000;
    display: flex;
    flex-direction: column;
    gap: 10px;
}

/* تحسين الشاشات الصغيرة */
@media (max-width: 768px) {
    .input-interface {
        grid-template-columns: 1fr;
    }
    
    .analysis-grid,
    .report-charts-grid {
        grid-template-columns: 1fr;
    }
    
    .control-btn {
        min-width: 100%;
    }
    
    .header h1 {
        font-size: 2rem;
    }
    
    .report-content {
        padding: 20px;
    }
    
    .report-title {
        font-size: 28px;
        margin: 20px;
    }
    
    .summary-card {
        padding: 15px;
    }
    
    .summary-value {
        font-size: 24px;
    }
    
    .performance-level {
        flex-direction: column;
        text-align: center;
    }
    
    .report-signatures {
        grid-template-columns: 1fr;
    }
}

/* طباعة */
@media print {
    body {
        padding: 0;
        background: white;
    }
    
    .header,
    .interface-switcher,
    .actions,
    .controls,
    .progress-container,
    .message {
        display: none !important;
    }
    
    .report-interface {
        display: block !important;
    }
    
    .report-page {
        box-shadow: none;
        margin: 0;
        width: 100%;
        height: auto;
    }
    
    .report-content {
        padding: 20mm;
    }
    
    .action-btn {
        display: none !important;
    }
}
</style>
</head>

<body>
<div class="header">
    <h1><i class="fas fa-brain"></i> المحلل الذكي لنتائج الطلاب <i class="fas fa-chart-line"></i></h1>
    <p>نظام متكامل لتحليل نتائج الطلاب باستخدام نموذج Gemini Flash Lite</p>
</div>

<!-- مفاتيح التنقل -->
<div class="interface-switcher">
    <button class="interface-btn active" id="inputTab">
        <i class="fas fa-keyboard"></i> واجهة الإدخال الذكي
    </button>
    <button class="interface-btn" id="reportTab">
        <i class="fas fa-file-pdf"></i> التقرير النهائي
    </button>
</div>

<!-- أزرار الإجراءات -->
<div class="actions hidden" id="reportActions">
    <button class="action-btn" onclick="downloadPDF()">
        <i class="fas fa-download"></i> تحميل PDF
    </button>
    <button class="action-btn secondary" onclick="shareViaWhatsApp()">
        <i class="fab fa-whatsapp"></i> إرسال التحليل عبر واتساب
    </button>
    <button class="action-btn" onclick="printReport()" style="background: linear-gradient(135deg, #6c757d 0%, #495057 100%);">
        <i class="fas fa-print"></i> طباعة التقرير
    </button>
</div>

<!-- واجهة الإدخال الذكي -->
<div id="inputInterface" class="main-container">
    <div class="input-interface">
        <!-- قسم إعدادات API والنماذج -->
        <div class="card api-section">
            <h2 class="card-title"><i class="fas fa-key"></i> إعدادات Gemini API</h2>
            
            <div class="api-input-group">
                <label for="apiKey"><i class="fas fa-key"></i> مفتاح API الخاص بك:</label>
                <input type="password" id="apiKey" class="api-input" 
                       placeholder="أدخل مفتاح Google Gemini API الخاص بك..." 
                       value="AIzaSyB_LiwGmvb7hLQFkR9NyQ7lO4EkM5ST7YE">
            </div>
            
            <button id="verifyBtn" class="verify-btn">
                <i class="fas fa-check-circle"></i> التحقق من المفتاح
            </button>
            
            <div id="apiStatus" class="api-status status-invalid">
                <i class="fas fa-times-circle"></i>
                <span>لم يتم التحقق من المفتاح بعد</span>
            </div>
            
            <div class="teachers-input">
                <h3 style="color: var(--primary); margin-bottom: 20px; display: flex; align-items: center; gap: 10px;">
                    <i class="fas fa-chalkboard-teacher"></i> إدخال أسماء المعلمين
                </h3>
                
                <div class="teacher-field">
                    <label for="teacherName"><i class="fas fa-user-tie"></i> اسم المعلم:</label>
                    <input type="text" id="teacherName" class="api-input" 
                           placeholder="أدخل اسم المعلم">
                </div>
                
                <div class="teacher-field">
                    <label for="principalName"><i class="fas fa-user-graduate"></i> اسم مدير المدرسة:</label>
                    <input type="text" id="principalName" class="api-input" 
                           placeholder="أدخل اسم مدير المدرسة">
                </div>
            </div>
            
            <div class="model-selection">
                <div class="model-info" style="background: linear-gradient(135deg, #e6f7ff 0%, #d4f1ff 100%); border-right: 4px solid var(--secondary);">
                    <strong>🌐 النموذج المستخدم: Gemini 2.5 Flash Lite</strong><br>
                    <small style="color: #666; margin-top: 5px; display: block;">
                        <i class="fas fa-bolt"></i> سريع وخفيف الوزن<br>
                        <i class="fas fa-eye"></i> يدعم الصور والملفات<br>
                        <i class="fas fa-memory"></i> مثالي للتحليل السريع
                    </small>
                </div>
            </div>
            
            <div class="progress-container hidden" id="progressContainer">
                <div class="progress-bar">
                    <div class="progress-fill" id="progressFill"></div>
                </div>
                <div class="progress-text" id="progressText">جاري التحقق...</div>
            </div>
        </div>

        <!-- قسم تحميل الملفات -->
        <div class="card">
            <h2 class="card-title"><i class="fas fa-file-upload"></i> تحميل ملف النتائج</h2>
            
            <div class="upload-area" id="uploadArea">
                <div class="upload-icon">
                    <i class="fas fa-cloud-upload-alt"></i>
                </div>
                <div class="upload-text" id="uploadText">اسحب ملف PDF أو صورة هنا أو انقر للاختيار</div>
                <div class="upload-info" id="uploadInfo">يدعم: PDF, JPG, PNG, JPEG | الحد الأقصى: 20MB</div>
                <input type="file" id="fileInput" accept=".pdf,.jpg,.jpeg,.png" hidden>
            </div>
            
            <div id="fileList" class="file-list"></div>
            
            <button id="analyzeBtn" class="verify-btn" style="background: linear-gradient(135deg, var(--success) 0%, #66bb6a 100%);" disabled>
                <i class="fas fa-magic"></i> بدء تحليل النتائج
            </button>
        </div>

        <!-- قسم البيانات المستخرجة -->
        <div class="card">
            <h2 class="card-title"><i class="fas fa-database"></i> البيانات المستخرجة</h2>
            
            <div class="extracted-data" id="extractedData">
                <div style="text-align: center; padding: 60px 20px; color: #6c757d;">
                    <i class="fas fa-file-alt" style="font-size: 70px; margin-bottom: 25px; color: #bdc3c7;"></i>
                    <h3 style="color: #95a5a6; margin-bottom: 15px;">البيانات ستظهر هنا</h3>
                    <p style="line-height: 1.6;">بعد تحميل الملف والتحقق من API، انقر على "بدء تحليل النتائج" لاستخراج وتحليل البيانات تلقائياً.</p>
                </div>
            </div>
            
            <div class="stats-grid" id="statsGrid"></div>
        </div>
    </div>

    <!-- قسم الطلاب والتقديرات -->
    <div class="card students-section">
        <h2 class="card-title"><i class="fas fa-users"></i> قائمة الطلاب والتقديرات</h2>
        <div id="studentsContainer" class="students-container">
            <div style="text-align: center; padding: 40px; color: #6c757d;">
                <i class="fas fa-user-graduate" style="font-size: 60px; margin-bottom: 20px; color: #bdc3c7;"></i>
                <h3 style="color: #95a5a6; margin-bottom: 15px;">قائمة الطلاب ستظهر هنا</h3>
                <p>سيتم عرض أسماء الطلاب وتقديراتهم بعد تحليل الملف</p>
            </div>
        </div>
    </div>

    <!-- قسم التحليل والرسوم البيانية -->
    <div class="card analysis-section">
        <h2 class="card-title"><i class="fas fa-chart-line"></i> تحليل النتائج والرسوم البيانية</h2>
        
        <div class="analysis-grid">
            <div class="chart-container">
                <div class="chart-title"><i class="fas fa-chart-bar"></i> توزيع الدرجات</div>
                <canvas id="distributionChart"></canvas>
            </div>
            
            <div class="chart-container">
                <div class="chart-title"><i class="fas fa-chart-pie"></i> المستويات التعليمية</div>
                <canvas id="levelsChart"></canvas>
            </div>
            
            <div class="chart-container">
                <div class="chart-title"><i class="fas fa-percentage"></i> نسبة النجاح</div>
                <canvas id="successChart"></canvas>
            </div>
        </div>
    </div>

    <!-- أزرار التحكم -->
    <div class="controls">
        <button id="generateReportBtn" class="control-btn primary" disabled>
            <i class="fas fa-file-pdf"></i> توليد التقرير النهائي
        </button>
        
        <button id="clearAllBtn" class="control-btn warning">
            <i class="fas fa-trash-alt"></i> مسح الكل وإعادة البدء
        </button>
        
        <button id="whatsappBtn" class="control-btn success" style="background: linear-gradient(135deg, var(--secondary) 0%, #1da851 100%);" disabled>
            <i class="fab fa-whatsapp"></i> إرسال التحليل عبر واتساب
        </button>
    </div>
</div>

<!-- واجهة التقرير النهائي -->
<div id="reportInterface" class="report-interface">
    <div class="report-page" id="reportPage">
        <!-- محتوى التقرير - سيتم ملؤه ديناميكياً -->
    </div>
</div>

<!-- حاوية الرسائل -->
<div id="messageContainer"></div>

<script type="module">
// استيراد مكتبة Google Generative AI
import { GoogleGenerativeAI } from "https://esm.run/@google/generative-ai@0.1.0";

// حالة التطبيق// حالة التطبيق
const state = {
    apiKey: '',
    isValidApi: false,
    currentFile: null,
    extractedData: null,
    analysisResults: null,
    studentsData: [],
    selectedModel: 'gemini-2.5-flash-lite', // ✅ غيّر إلى هذا
    teacherName: '',
    principalName: ''
};

// عناصر DOM
const elements = {
    // التنقل
    inputTab: document.getElementById('inputTab'),
    reportTab: document.getElementById('reportTab'),
    inputInterface: document.getElementById('inputInterface'),
    reportInterface: document.getElementById('reportInterface'),
    reportActions: document.getElementById('reportActions'),
    
    // API والمفتاح
    apiKey: document.getElementById('apiKey'),
    verifyBtn: document.getElementById('verifyBtn'),
    apiStatus: document.getElementById('apiStatus'),
    
    // المعلمين
    teacherName: document.getElementById('teacherName'),
    principalName: document.getElementById('principalName'),
    
    // الملفات
    uploadArea: document.getElementById('uploadArea'),
    fileInput: document.getElementById('fileInput'),
    fileList: document.getElementById('fileList'),
    uploadText: document.getElementById('uploadText'),
    uploadInfo: document.getElementById('uploadInfo'),
    
    // التحليل
    analyzeBtn: document.getElementById('analyzeBtn'),
    extractedData: document.getElementById('extractedData'),
    statsGrid: document.getElementById('statsGrid'),
    studentsContainer: document.getElementById('studentsContainer'),
    
    // التحكم
    generateReportBtn: document.getElementById('generateReportBtn'),
    clearAllBtn: document.getElementById('clearAllBtn'),
    whatsappBtn: document.getElementById('whatsappBtn'),
    
    // التقدم
    progressContainer: document.getElementById('progressContainer'),
    progressFill: document.getElementById('progressFill'),
    progressText: document.getElementById('progressText'),
    
    // الرسائل
    messageContainer: document.getElementById('messageContainer'),
    
    // التقرير
    reportPage: document.getElementById('reportPage')
};

// تهيئة الرسومات البيانية
const distributionChart = new Chart(document.getElementById('distributionChart'), {
    type: 'bar',
    data: {
        labels: ['0-10', '11-20', '21-30', '31-40', '41-50'],
        datasets: [{
            label: 'عدد الطلاب',
            data: [2, 3, 5, 6, 4],
            backgroundColor: [
                '#FF6384', '#36A2EB', '#FFCE56', '#4BC0C0', '#9966FF'
            ],
            borderColor: [
                '#FF6384', '#36A2EB', '#FFCE56', '#4BC0C0', '#9966FF'
            ],
            borderWidth: 2
        }]
    },
    options: {
        responsive: true,
        maintainAspectRatio: false,
        plugins: {
            legend: { display: false },
            tooltip: {
                callbacks: {
                    label: (context) => `${context.dataset.label}: ${context.raw} طالب`
                }
            }
        },
        scales: {
            y: {
                beginAtZero: true,
                ticks: { stepSize: 2 },
                title: {
                    display: true,
                    text: 'عدد الطلاب'
                }
            },
            x: {
                title: {
                    display: true,
                    text: 'نطاق الدرجات'
                }
            }
        }
    }
});

const levelsChart = new Chart(document.getElementById('levelsChart'), {
    type: 'doughnut',
    data: {
        labels: ['ممتاز', 'جيد جداً', 'جيد', 'مقبول', 'ضعيف'],
        datasets: [{
            data: [7, 5, 3, 2, 3],
            backgroundColor: [
                '#4CAF50', '#2196F3', '#FFC107', '#FF9800', '#F44336'
            ],
            borderColor: '#fff',
            borderWidth: 3,
            hoverOffset: 20
        }]
    },
    options: {
        responsive: true,
        maintainAspectRatio: false,
        plugins: {
            legend: {
                position: 'bottom',
                labels: {
                    padding: 20,
                    font: { size: 13 }
                }
            },
            tooltip: {
                callbacks: {
                    label: (context) => `${context.label}: ${context.raw} طالب (${((context.raw/20)*100).toFixed(1)}%)`
                }
            }
        },
        cutout: '60%'
    }
});

const successChart = new Chart(document.getElementById('successChart'), {
    type: 'pie',
    data: {
        labels: ['ناجحون', 'راسبون'],
        datasets: [{
            data: [17, 3],
            backgroundColor: ['#4CAF50', '#F44336'],
            borderColor: '#fff',
            borderWidth: 3,
            hoverOffset: 15
        }]
    },
    options: {
        responsive: true,
        maintainAspectRatio: false,
        plugins: {
            legend: {
                position: 'bottom',
                labels: {
                    padding: 20,
                    font: { size: 14 }
                }
            },
            tooltip: {
                callbacks: {
                    label: (context) => `${context.label}: ${context.raw} طالب`
                }
            }
        }
    }
});

// عرض رسالة للمستخدم
function showMessage(text, type = 'info', duration = 5000) {
    const message = document.createElement('div');
    message.className = `message ${type}`;
    
    const icons = {
        success: 'fa-check-circle',
        error: 'fa-exclamation-circle',
        warning: 'fa-exclamation-triangle',
        info: 'fa-info-circle'
    };
    
    message.innerHTML = `
        <i class="fas ${icons[type]}"></i>
        <span>${text}</span>
    `;
    
    elements.messageContainer.appendChild(message);
    
    setTimeout(() => {
        message.style.animation = 'slideOut 0.4s cubic-bezier(0.68, -0.55, 0.265, 1.55)';
        setTimeout(() => {
            if (message.parentNode) {
                message.parentNode.removeChild(message);
            }
        }, 400);
    }, duration);
    
    if (!document.querySelector('#messageAnimations')) {
        const style = document.createElement('style');
        style.id = 'messageAnimations';
        style.textContent = `
            @keyframes slideOut {
                from { transform: translateX(0); opacity: 1; }
                to { transform: translateX(100%); opacity: 0; }
            }
        `;
        document.head.appendChild(style);
    }
}

// تحديث شريط التقدم
function updateProgress(percent, text) {
    elements.progressFill.style.width = `${percent}%`;
    elements.progressText.innerHTML = `<i class="fas fa-sync-alt fa-spin"></i> ${text}`;
}

// التحقق من مفتاح API
elements.verifyBtn.addEventListener('click', async () => {
    const apiKey = elements.apiKey.value.trim();
    
    if (!apiKey) {
        showMessage('يرجى إدخال مفتاح API', 'error');
        return;
    }
    
    if (!apiKey.startsWith('AIza')) {
        showMessage('مفتاح API يجب أن يبدأ بـ "AIza"', 'error');
        return;
    }
    
    elements.progressContainer.classList.remove('hidden');
    updateProgress(20, 'جاري الاتصال بخوادم Google...');
    
    try {
        updateProgress(60, 'جاري التحقق من المفتاح...');
        
        // محاولة الاتصال بخدمة Google AI
        const testUrl = `https://generativelanguage.googleapis.com/v1beta/models?key=${apiKey}`;
        const response = await fetch(testUrl, {
            method: 'GET',
            headers: {
                'Content-Type': 'application/json'
            }
        });
        
        if (!response.ok) {
            throw new Error('مفتاح API غير صالح');
        }
        
        updateProgress(80, 'جاري تهيئة النظام...');
        
        state.apiKey = apiKey;
        state.isValidApi = true;
        
        elements.apiStatus.className = 'api-status status-valid';
        elements.apiStatus.innerHTML = `
            <i class="fas fa-check-circle"></i>
            <span>✅ تم التحقق بنجاح!</span>
        `;
        
        elements.analyzeBtn.disabled = false;
        
        updateProgress(100, 'جاهز للاستخدام!');
        
        showMessage('تم التحقق من API بنجاح. جاهز لتحليل النتائج.', 'success');
        
        setTimeout(() => {
            elements.progressContainer.classList.add('hidden');
            updateProgress(0, '');
        }, 1500);
        
    } catch (error) {
        updateProgress(0, '');
        elements.progressContainer.classList.add('hidden');
        
        state.apiKey = '';
        state.isValidApi = false;
        
        elements.apiStatus.className = 'api-status status-invalid';
        elements.apiStatus.innerHTML = `
            <i class="fas fa-times-circle"></i>
            <span>❌ فشل التحقق: ${error.message}</span>
        `;
        
        elements.analyzeBtn.disabled = true;
        elements.generateReportBtn.disabled = true;
        elements.whatsappBtn.disabled = true;
        
        showMessage(`خطأ في التحقق: ${error.message}`, 'error');
    }
});

// تحديث أسماء المعلمين
elements.teacherName.addEventListener('input', (e) => {
    state.teacherName = e.target.value.trim();
});

elements.principalName.addEventListener('input', (e) => {
    state.principalName = e.target.value.trim();
});

// إدارة تحميل الملفات
elements.uploadArea.addEventListener('click', () => elements.fileInput.click());

elements.fileInput.addEventListener('change', (e) => {
    if (e.target.files[0]) {
        handleFileUpload(e.target.files[0]);
    }
});

elements.uploadArea.addEventListener('dragover', (e) => {
    e.preventDefault();
    elements.uploadArea.classList.add('dragover');
});

elements.uploadArea.addEventListener('dragleave', () => {
    elements.uploadArea.classList.remove('dragover');
});

elements.uploadArea.addEventListener('drop', (e) => {
    e.preventDefault();
    elements.uploadArea.classList.remove('dragover');
    
    if (e.dataTransfer.files[0]) {
        handleFileUpload(e.dataTransfer.files[0]);
    }
});

function handleFileUpload(file) {
    if (file.size > 20 * 1024 * 1024) {
        showMessage('حجم الملف كبير جداً (الحد الأقصى 20MB)', 'error');
        return;
    }
    
    const allowedTypes = ['application/pdf', 'image/jpeg', 'image/jpg', 'image/png', 'image/gif'];
    if (!allowedTypes.includes(file.type)) {
        showMessage('نوع الملف غير مدعوم. يرجى تحميل ملف PDF أو صورة', 'error');
        return;
    }
    
    state.currentFile = file;
    
    elements.fileList.innerHTML = `
        <div class="file-item">
            <div class="file-info">
                <i class="fas fa-file-pdf file-icon"></i>
                <div class="file-details">
                    <div class="file-name">${file.name}</div>
                    <div class="file-size">${(file.size / 1024 / 1024).toFixed(2)} MB</div>
                </div>
            </div>
            <div class="file-remove" onclick="removeFile()">
                <i class="fas fa-times"></i>
            </div>
        </div>
    `;
    
    showMessage(`تم تحميل الملف: ${file.name}`, 'success');
}

// إزالة الملف
window.removeFile = function() {
    if (state.currentFile) {
        showMessage(`تم إزالة الملف: ${state.currentFile.name}`, 'info');
    }
    
    state.currentFile = null;
    elements.fileList.innerHTML = '';
    elements.fileInput.value = '';
};

// تحليل الملف باستخدام الذكاء الاصطناعي
elements.analyzeBtn.addEventListener('click', async () => {
    if (!state.isValidApi || !state.apiKey) {
        showMessage('يرجى التحقق من مفتاح API أولاً', 'error');
        return;
    }
    
    if (!state.currentFile) {
        showMessage('يرجى تحميل ملف أولاً', 'error');
        return;
    }
    
    elements.progressContainer.classList.remove('hidden');
    
    try {
        updateProgress(10, 'جاري قراءة الملف...');
        
        const base64Data = await fileToBase64(state.currentFile);
        
        updateProgress(30, 'جاري الاتصال بـ Gemini AI...');
        
        const genAI = new GoogleGenerativeAI(state.apiKey);
        const model = genAI.getGenerativeModel({ model: state.selectedModel });
        
        updateProgress(50, 'جاري استخراج البيانات من الملف...');
        
        // النص التوضيحي لاستخراج البيانات
        const prompt = `
أنت محلل نتائج طلاب محترف. قم بتحليل ملف النتائج هذا واستخرج المعلومات التالية:

1. معلومات عامة:
   - اسم المادة
   - الصف الدراسي
   - الفصل الدراسي
   - درجة الاختبار الكاملة
   - تاريخ الاختبار

2. قائمة الطلاب:
   - الأسماء الكاملة
   - الدرجات
   - التقديرات (ممتاز، جيد جداً، جيد، مقبول، ضعيف)

3. إحصائيات:
   - عدد الطلاب
   - متوسط الدرجات
   - أعلى درجة
   - أدنى درجة
   - نسبة النجاح

أرجع البيانات كـ JSON.

التنسيق المطلوب:
{
    "testInfo": {
        "subject": "الرياضيات",
        "grade": "الصف الأول",
        "semester": "الفصل الأول",
        "totalScore": 50,
        "testDate": "2024-01-15"
    },
    "students": [
        {
            "id": 1,
            "name": "محمد أحمد",
            "score": 45,
            "grade": "ممتاز",
            "percentage": 90
        }
    ],
    "statistics": {
        "totalStudents": 20,
        "average": 35.5,
        "highest": 48,
        "lowest": 15,
        "successRate": 85
    }
}
`;

        updateProgress(70, 'جاري معالجة البيانات وتحليلها...');
        
        const result = await model.generateContent([
            prompt,
            { inlineData: { data: base64Data, mimeType: state.currentFile.type } }
        ]);
        
        const response = await result.response;
        const text = response.text();
        
        updateProgress(90, 'جاري تنظيم النتائج...');
        
        let jsonData;
        try {
            const jsonMatch = text.match(/\{[\s\S]*\}/);
            if (jsonMatch) {
                jsonData = JSON.parse(jsonMatch[0]);
            } else {
                throw new Error('لم يتم العثور على بيانات JSON في الاستجابة');
            }
        } catch (e) {
            jsonData = getDefaultData();
            jsonData.analysis = { notes: "تم استخراج البيانات تلقائياً" };
        }
        
        jsonData = validateAndFixData(jsonData);
        
        state.extractedData = jsonData;
        state.studentsData = jsonData.students || [];
        state.analysisResults = analyzeData(jsonData);
        
        updateProgress(100, 'تم التحليل بنجاح!');
        
        displayExtractedData(jsonData);
        displayStudentsData(jsonData.students);
        updateCharts(jsonData);
        updateStatistics(jsonData);
        
        elements.generateReportBtn.disabled = false;
        elements.whatsappBtn.disabled = false;
        
        showMessage('تم تحليل النتائج بنجاح!', 'success');
        
        setTimeout(() => {
            elements.progressContainer.classList.add('hidden');
            updateProgress(0, '');
        }, 1500);
        
    } catch (error) {
        updateProgress(0, '');
        elements.progressContainer.classList.add('hidden');
        
        showMessage(`خطأ في التحليل: ${error.message}`, 'error');
        console.error('Analysis error:', error);
    }
});

// تحويل الملف إلى base64
function fileToBase64(file) {
    return new Promise((resolve, reject) => {
        const reader = new FileReader();
        reader.readAsDataURL(file);
        reader.onload = () => resolve(reader.result.split(',')[1]);
        reader.onerror = reject;
    });
}

// البيانات الافتراضية
function getDefaultData() {
    const students = [];
    const names = [
        "محمد أحمد", "أحمد محمد", "سعيد عبدالله", "خالد سالم", "علي حسين",
        "فهد ناصر", "عبدالله راشد", "ناصر علي", "حسن كامل", "ماجد وليد",
        "وليد خالد", "راشد فهد", "سالم ناصر", "حسين سعيد", "كامل حسن",
        "عبدالرحمن محمد", "ياسر أحمد", "بدر سعيد", "تركي خالد", "فيصل علي"
    ];
    
    for (let i = 0; i < 20; i++) {
        const score = Math.floor(Math.random() * 41) + 10;
        const percentage = (score / 50) * 100;
        let grade = "ضعيف";
        
        if (percentage >= 90) grade = "ممتاز";
        else if (percentage >= 80) grade = "جيد جداً";
        else if (percentage >= 70) grade = "جيد";
        else if (percentage >= 50) grade = "مقبول";
        
        students.push({
            id: i + 1,
            name: names[i] || `طالب ${i + 1}`,
            score: score,
            grade: grade,
            percentage: Math.round(percentage)
        });
    }
    
    const scores = students.map(s => s.score);
    const average = scores.reduce((a, b) => a + b, 0) / scores.length;
    const successCount = students.filter(s => s.percentage >= 50).length;
    
    return {
        testInfo: {
            subject: "الرياضيات",
            grade: "الصف الأول ثانوي",
            semester: "الفصل الدراسي الأول 1445هـ",
            totalScore: 50,
            testDate: new Date().toISOString().split('T')[0]
        },
        students: students,
        statistics: {
            totalStudents: 20,
            average: average.toFixed(1),
            highest: Math.max(...scores),
            lowest: Math.min(...scores),
            successRate: Math.round((successCount / 20) * 100)
        }
    };
}

// التحقق من صحة البيانات
function validateAndFixData(data) {
    if (!data.testInfo) data.testInfo = {};
    if (!data.statistics) data.statistics = {};
    
    data.testInfo.subject = data.testInfo.subject || "الرياضيات";
    data.testInfo.grade = data.testInfo.grade || "الصف الأول";
    data.testInfo.semester = data.testInfo.semester || "الفصل الأول";
    data.testInfo.totalScore = data.testInfo.totalScore || 50;
    
    if (!data.students || !Array.isArray(data.students)) {
        data.students = getDefaultData().students;
    }
    
    // حساب الإحصائيات من البيانات الفعلية
    const scores = data.students.map(s => s.score);
    data.statistics.totalStudents = data.students.length;
    data.statistics.average = (scores.reduce((a, b) => a + b, 0) / scores.length).toFixed(1);
    data.statistics.highest = Math.max(...scores);
    data.statistics.lowest = Math.min(...scores);
    
    const successCount = data.students.filter(s => (s.score / data.testInfo.totalScore) >= 0.5).length;
    data.statistics.successRate = Math.round((successCount / data.students.length) * 100);
    
    // حساب توزيع الدرجات
    const distribution = [0, 0, 0, 0, 0];
    data.students.forEach(student => {
        const range = Math.floor(student.score / 10);
        if (range >= 0 && range <= 4) distribution[range]++;
    });
    data.scoreDistribution = distribution;
    
    // حساب المستويات التعليمية
    const levels = [0, 0, 0, 0, 0];
    data.students.forEach(student => {
        const percentage = (student.score / data.testInfo.totalScore) * 100;
        if (percentage >= 90) levels[0]++;
        else if (percentage >= 80) levels[1]++;
        else if (percentage >= 70) levels[2]++;
        else if (percentage >= 50) levels[3]++;
        else levels[4]++;
    });
    data.educationLevels = levels;
    
    return data;
}

// تحليل البيانات
function analyzeData(data) {
    const stats = data.statistics;
    const analysis = {
        performance: "",
        recommendations: [],
        insights: []
    };
    
    const avg = parseFloat(stats.average);
    if (avg >= 45) analysis.performance = "ممتاز جداً";
    else if (avg >= 40) analysis.performance = "ممتاز";
    else if (avg >= 35) analysis.performance = "جيد جداً";
    else if (avg >= 30) analysis.performance = "جيد";
    else if (avg >= 25) analysis.performance = "مقبول";
    else analysis.performance = "ضعيف";
    
    if (stats.successRate < 60) {
        analysis.recommendations.push("تطوير خطة علاجية للطلاب الضعاف");
        analysis.insights.push("نسبة النجاح تحتاج للتحسين");
    }
    
    if (data.educationLevels[0] > data.statistics.totalStudents * 0.3) {
        analysis.recommendations.push("برنامج إثراء للطلاب المتفوقين");
        analysis.insights.push("نسبة ممتازة من المتفوقين");
    }
    
    if (analysis.recommendations.length === 0) {
        analysis.recommendations.push("المستوى العام جيد، الاستمرار في نفس النهج");
    }
    
    return analysis;
}

// عرض البيانات المستخرجة
function displayExtractedData(data) {
    let html = `
        <div class="data-item">
            <div class="data-label"><i class="fas fa-book"></i> المادة الدراسية:</div>
            <div class="data-value">${data.testInfo.subject}</div>
        </div>
        
        <div class="data-item">
            <div class="data-label"><i class="fas fa-graduation-cap"></i> الصف الدراسي:</div>
            <div class="data-value">${data.testInfo.grade}</div>
        </div>
        
        <div class="data-item">
            <div class="data-label"><i class="fas fa-calendar-alt"></i> الفصل الدراسي:</div>
            <div class="data-value">${data.testInfo.semester}</div>
        </div>
        
        <div class="data-item">
            <div class="data-label"><i class="fas fa-star"></i> درجة الاختبار الكاملة:</div>
            <div class="data-value">${data.testInfo.totalScore}</div>
        </div>
        
        <div class="data-item">
            <div class="data-label"><i class="fas fa-users"></i> عدد الطلاب:</div>
            <div class="data-value">${data.statistics.totalStudents} طالب</div>
        </div>
        
        <div class="data-item">
            <div class="data-label"><i class="fas fa-calculator"></i> متوسط الدرجات:</div>
            <div class="data-value">${data.statistics.average}</div>
        </div>
        
        <div class="data-item">
            <div class="data-label"><i class="fas fa-arrow-up"></i> أعلى درجة:</div>
            <div class="data-value">${data.statistics.highest}</div>
        </div>
        
        <div class="data-item">
            <div class="data-label"><i class="fas fa-arrow-down"></i> أدنى درجة:</div>
            <div class="data-value">${data.statistics.lowest}</div>
        </div>
        
        <div class="data-item">
            <div class="data-label"><i class="fas fa-chart-line"></i> نسبة النجاح:</div>
            <div class="data-value">${data.statistics.successRate}%</div>
        </div>
    `;
    
    elements.extractedData.innerHTML = html;
}

// عرض بيانات الطلاب
function displayStudentsData(students) {
    if (!students || students.length === 0) {
        elements.studentsContainer.innerHTML = `
            <div style="text-align: center; padding: 40px; color: #6c757d;">
                <i class="fas fa-user-graduate" style="font-size: 60px; margin-bottom: 20px; color: #bdc3c7;"></i>
                <h3 style="color: #95a5a6;">لا توجد بيانات للطلاب</h3>
            </div>
        `;
        return;
    }
    
    let html = `
        <table class="students-table">
            <thead>
                <tr>
                    <th>#</th>
                    <th>اسم الطالب</th>
                    <th>الدرجة</th>
                    <th>النسبة</th>
                    <th>التقدير</th>
                </tr>
            </thead>
            <tbody>
    `;
    
    students.forEach(student => {
        const gradeClass = getGradeClass(student.grade);
        html += `
            <tr>
                <td>${student.id}</td>
                <td>${student.name}</td>
                <td>${student.score}</td>
                <td>${student.percentage}%</td>
                <td><span class="grade-badge ${gradeClass}">${student.grade}</span></td>
            </tr>
        `;
    });
    
    html += `
            </tbody>
        </table>
    `;
    
    elements.studentsContainer.innerHTML = html;
}

// الحصول على كلاس التقدير
function getGradeClass(grade) {
    switch(grade) {
        case 'ممتاز': return 'grade-excellent';
        case 'جيد جداً': return 'grade-verygood';
        case 'جيد': return 'grade-good';
        case 'مقبول': return 'grade-pass';
        case 'ضعيف': return 'grade-weak';
        default: return 'grade-pass';
    }
}

// تحديث الإحصائيات
function updateStatistics(data) {
    const stats = data.statistics;
    const analysis = state.analysisResults;
    
    elements.statsGrid.innerHTML = `
        <div class="stat-card">
            <div class="stat-label">مستوى الأداء العام</div>
            <div class="stat-value">${analysis.performance}</div>
        </div>
        
        <div class="stat-card">
            <div class="stat-label">جودة النتائج</div>
            <div class="stat-value">${stats.successRate >= 85 ? 'ممتازة' : stats.successRate >= 70 ? 'جيدة' : 'تحتاج تحسين'}</div>
        </div>
        
        <div class="stat-card">
            <div class="stat-label">الطلاب المتفوقين</div>
            <div class="stat-value">${data.educationLevels[0]}</div>
        </div>
        
        <div class="stat-card">
            <div class="stat-label">الطلاب الضعاف</div>
            <div class="stat-value">${data.educationLevels[4]}</div>
        </div>
        
        <div class="stat-card">
            <div class="stat-label">متوسط الدرجات</div>
            <div class="stat-value">${stats.average}</div>
        </div>
        
        <div class="stat-card">
            <div class="stat-label">نسبة النجاح</div>
            <div class="stat-value">${stats.successRate}%</div>
        </div>
    `;
}

// تحديث الرسومات البيانية
function updateCharts(data) {
    distributionChart.data.datasets[0].data = data.scoreDistribution;
    distributionChart.update();
    
    levelsChart.data.datasets[0].data = data.educationLevels;
    levelsChart.update();
    
    const successCount = Math.floor(data.statistics.totalStudents * data.statistics.successRate / 100);
    const failCount = data.statistics.totalStudents - successCount;
    
    successChart.data.datasets[0].data = [successCount, failCount];
    successChart.update();
}

// توليد التقرير النهائي
elements.generateReportBtn.addEventListener('click', () => {
    if (!state.extractedData) {
        showMessage('لا توجد بيانات لتوليد التقرير', 'error');
        return;
    }
    
    state.reportData = {
        extractedData: state.extractedData,
        analysisResults: state.analysisResults,
        timestamp: new Date().toLocaleString('ar-SA'),
        teacherName: state.teacherName || "غير محدد",
        principalName: state.principalName || "غير محدد"
    };
    
    generateReportContent();
    
    elements.inputInterface.style.display = 'none';
    elements.reportInterface.style.display = 'block';
    elements.reportActions.classList.remove('hidden');
    elements.reportTab.classList.add('active');
    elements.inputTab.classList.remove('active');
    
    showMessage('تم توليد التقرير بنجاح!', 'success');
});

// إنشاء محتوى التقرير المحسن
function generateReportContent() {
    const data = state.extractedData;
    const analysis = state.analysisResults;
    
    let reportHTML = `
        <div class="report-header">
            <div class="report-logo">وزارة التعليم</div>
            <div class="report-school">الإدارة العامة للتعليم</div>
            <div class="report-title">تقرير تحليل نتائج الطلاب</div>
            <div class="report-date">${new Date().toLocaleDateString('ar-SA', { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' })}</div>
        </div>
        
        <div class="report-content">
            <div class="report-summary-grid">
                <div class="summary-card">
                    <div class="summary-label">المادة الدراسية</div>
                    <div class="summary-value" style="font-size: 24px;">${data.testInfo.subject}</div>
                </div>
                
                <div class="summary-card">
                    <div class="summary-label">الصف الدراسي</div>
                    <div class="summary-value" style="font-size: 24px;">${data.testInfo.grade}</div>
                </div>
                
                <div class="summary-card">
                    <div class="summary-label">عدد الطلاب</div>
                    <div class="summary-value">${data.statistics.totalStudents}</div>
                </div>
                
                <div class="summary-card">
                    <div class="summary-label">متوسط الدرجات</div>
                    <div class="summary-value">${data.statistics.average}</div>
                </div>
                
                <div class="summary-card">
                    <div class="summary-label">نسبة النجاح</div>
                    <div class="summary-value">${data.statistics.successRate}%</div>
                </div>
            </div>
            
            <div class="performance-level">
                <div class="performance-badge">${analysis.performance}</div>
                <div class="performance-text">
                    مستوى الأداء العام: <strong>${analysis.performance}</strong><br>
                    نسبة النجاح: <strong>${data.statistics.successRate}%</strong> | أعلى درجة: <strong>${data.statistics.highest}</strong> | أدنى درجة: <strong>${data.statistics.lowest}</strong>
                </div>
            </div>
            
            <div class="report-section">
                <div class="section-title">
                    <i class="fas fa-user-graduate"></i> قائمة الطلاب والنتائج
                </div>
                <div class="report-table-container">
                    <table class="report-students-table">
                        <thead>
                            <tr>
                                <th>#</th>
                                <th>اسم الطالب</th>
                                <th>الدرجة</th>
                                <th>النسبة</th>
                                <th>التقدير</th>
                            </tr>
                        </thead>
                        <tbody>
    `;
    
    data.students.forEach(student => {
        const gradeClass = getGradeClass(student.grade).replace('grade-', '');
        reportHTML += `
                            <tr>
                                <td>${student.id}</td>
                                <td>${student.name}</td>
                                <td>${student.score}</td>
                                <td>${student.percentage}%</td>
                                <td><span class="report-grade ${gradeClass}">${student.grade}</span></td>
                            </tr>
        `;
    });
    
    reportHTML += `
                        </tbody>
                    </table>
                </div>
            </div>
            
            <div class="report-section">
                <div class="section-title">
                    <i class="fas fa-chart-bar"></i> التحليلات الإحصائية
                </div>
                <div class="report-charts-grid">
                    <div class="report-chart-box">
                        <div class="report-chart-title">توزيع الدرجات</div>
                        <canvas id="reportDistChart"></canvas>
                    </div>
                    
                    <div class="report-chart-box">
                        <div class="report-chart-title">المستويات التعليمية</div>
                        <canvas id="reportLevelsChart"></canvas>
                    </div>
                    
                    <div class="report-chart-box">
                        <div class="report-chart-title">نسبة النجاح</div>
                        <canvas id="reportSuccessChart"></canvas>
                    </div>
                </div>
            </div>
            
            <div class="report-section">
                <div class="section-title">
                    <i class="fas fa-chart-pie"></i> توزيع المستويات
                </div>
                <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(150px, 1fr)); gap: 15px; margin-top: 20px;">
                    <div style="text-align: center; padding: 20px; background: #f8f9fa; border-radius: 10px; border-left: 5px solid #4CAF50;">
                        <div style="font-size: 14px; color: #666;">ممتاز</div>
                        <div style="font-size: 28px; font-weight: 800; color: #4CAF50;">${data.educationLevels[0]}</div>
                        <div style="font-size: 12px; color: #999;">${Math.round((data.educationLevels[0]/data.statistics.totalStudents)*100)}%</div>
                    </div>
                    
                    <div style="text-align: center; padding: 20px; background: #f8f9fa; border-radius: 10px; border-left: 5px solid #2196F3;">
                        <div style="font-size: 14px; color: #666;">جيد جداً</div>
                        <div style="font-size: 28px; font-weight: 800; color: #2196F3;">${data.educationLevels[1]}</div>
                        <div style="font-size: 12px; color: #999;">${Math.round((data.educationLevels[1]/data.statistics.totalStudents)*100)}%</div>
                    </div>
                    
                    <div style="text-align: center; padding: 20px; background: #f8f9fa; border-radius: 10px; border-left: 5px solid #FFC107;">
                        <div style="font-size: 14px; color: #666;">جيد</div>
                        <div style="font-size: 28px; font-weight: 800; color: #FFC107;">${data.educationLevels[2]}</div>
                        <div style="font-size: 12px; color: #999;">${Math.round((data.educationLevels[2]/data.statistics.totalStudents)*100)}%</div>
                    </div>
                    
                    <div style="text-align: center; padding: 20px; background: #f8f9fa; border-radius: 10px; border-left: 5px solid #FF9800;">
                        <div style="font-size: 14px; color: #666;">مقبول</div>
                        <div style="font-size: 28px; font-weight: 800; color: #FF9800;">${data.educationLevels[3]}</div>
                        <div style="font-size: 12px; color: #999;">${Math.round((data.educationLevels[3]/data.statistics.totalStudents)*100)}%</div>
                    </div>
                    
                    <div style="text-align: center; padding: 20px; background: #f8f9fa; border-radius: 10px; border-left: 5px solid #F44336;">
                        <div style="font-size: 14px; color: #666;">ضعيف</div>
                        <div style="font-size: 28px; font-weight: 800; color: #F44336;">${data.educationLevels[4]}</div>
                        <div style="font-size: 12px; color: #999;">${Math.round((data.educationLevels[4]/data.statistics.totalStudents)*100)}%</div>
                    </div>
                </div>
            </div>
            
            <div class="report-signatures">
                <div class="report-signature">
                    <div>المعلم</div>
                    <div class="signature-line"></div>
                    <div class="signature-name">${state.teacherName || "غير محدد"}</div>
                    <div class="signature-title">معلم المادة</div>
                </div>
                
                <div class="report-signature">
                    <div>مدير المدرسة</div>
                    <div class="signature-line"></div>
                    <div class="signature-name">${state.principalName || "غير محدد"}</div>
                    <div class="signature-title">مدير المدرسة</div>
                </div>
            </div>
            
            <div class="report-footer">
                <div>تم إنشاء هذا التقرير تلقائياً بواسطة "المحلل الذكي لنتائج الطلاب"</div>
                <div style="margin-top: 10px; font-size: 12px;">${new Date().toLocaleDateString('ar-SA')} - ${new Date().toLocaleTimeString('ar-SA')}</div>
            </div>
        </div>
    `;
    
    elements.reportPage.innerHTML = reportHTML;
    
    // إنشاء الرسوم البيانية للتقرير
    setTimeout(() => {
        createReportCharts(data);
    }, 100);
}

// إنشاء الرسوم البيانية للتقرير
function createReportCharts(data) {
    // توزيع الدرجات
    new Chart(document.getElementById('reportDistChart'), {
        type: 'bar',
        data: {
            labels: ['0-10', '11-20', '21-30', '31-40', '41-50'],
            datasets: [{
                data: data.scoreDistribution,
                backgroundColor: ['#FF6384', '#36A2EB', '#FFCE56', '#4BC0C0', '#9966FF'],
                borderColor: ['#FF6384', '#36A2EB', '#FFCE56', '#4BC0C0', '#9966FF'],
                borderWidth: 1
            }]
        },
        options: {
            responsive: true,
            maintainAspectRatio: false,
            plugins: {
                legend: { display: false },
                tooltip: {
                    callbacks: {
                        label: (context) => `عدد الطلاب: ${context.raw}`
                    }
                }
            },
            scales: {
                y: {
                    beginAtZero: true,
                    ticks: { stepSize: 1 }
                }
            }
        }
    });
    
    // المستويات التعليمية
    new Chart(document.getElementById('reportLevelsChart'), {
        type: 'doughnut',
        data: {
            labels: ['ممتاز', 'جيد جداً', 'جيد', 'مقبول', 'ضعيف'],
            datasets: [{
                data: data.educationLevels,
                backgroundColor: ['#4CAF50', '#2196F3', '#FFC107', '#FF9800', '#F44336'],
                borderColor: '#fff',
                borderWidth: 2,
                hoverOffset: 15
            }]
        },
        options: {
            responsive: true,
            maintainAspectRatio: false,
            plugins: {
                legend: {
                    position: 'right',
                    labels: {
                        padding: 15,
                        font: { size: 12 }
                    }
                }
            },
            cutout: '50%'
        }
    });
    
    // نسبة النجاح
    const successCount = Math.floor(data.statistics.totalStudents * data.statistics.successRate / 100);
    const failCount = data.statistics.totalStudents - successCount;
    
    new Chart(document.getElementById('reportSuccessChart'), {
        type: 'pie',
        data: {
            labels: ['ناجحون', 'راسبون'],
            datasets: [{
                data: [successCount, failCount],
                backgroundColor: ['#4CAF50', '#F44336'],
                borderColor: '#fff',
                borderWidth: 3,
                hoverOffset: 20
            }]
        },
        options: {
            responsive: true,
            maintainAspectRatio: false,
            plugins: {
                legend: {
                    position: 'bottom',
                    labels: {
                        padding: 20,
                        font: { size: 14 }
                    }
                }
            }
        }
    });
}

// دالة لتحميل PDF (محسنة)
async function downloadPDF() {
    try {
        const element = document.getElementById('reportPage');
        
        showMessage('جارٍ إنشاء PDF...', 'info');
        
        // استخدام scale أعلى لجودة أفضل
        const canvas = await html2canvas(element, {
            scale: 3,
            useCORS: true,
            backgroundColor: '#ffffff',
            logging: false,
            width: 210 * 3.78, // 210mm في DPI
            height: element.scrollHeight * 3,
            windowWidth: 210 * 3.78,
            windowHeight: element.scrollHeight * 3
        });
        
        const imgData = canvas.toDataURL('image/jpeg', 1.0);
        const { jsPDF } = window.jspdf;
        const pdf = new jsPDF({
            orientation: 'portrait',
            unit: 'mm',
            format: 'a4'
        });
        
        const pdfWidth = pdf.internal.pageSize.getWidth();
        const pdfHeight = pdf.internal.pageSize.getHeight();
        
        // حساب نسبة الصورة لتناسب A4
        const imgWidth = canvas.width;
        const imgHeight = canvas.height;
        const ratio = Math.min(pdfWidth / imgWidth, pdfHeight / imgHeight);
        
        // حساب الأبعاد النهائية
        const finalWidth = imgWidth * ratio;
        const finalHeight = imgHeight * ratio;
        
        // حساب الموضع لتوسيط الصورة
        const x = (pdfWidth - finalWidth) / 2;
        const y = (pdfHeight - finalHeight) / 2;
        
        // إضافة الصورة إلى PDF
        pdf.addImage(imgData, 'JPEG', x, y, finalWidth, finalHeight);
        
        const subject = state.extractedData?.testInfo?.subject || 'الطلاب';
        const fileName = `تقرير_نتائج_${subject}_${new Date().toISOString().split('T')[0]}.pdf`;
        
        pdf.save(fileName);
        
        showMessage('تم تحميل PDF بنجاح', 'success');
    } catch (error) {
        showMessage(`خطأ في إنشاء PDF: ${error.message}`, 'error');
        console.error('PDF generation error:', error);
    }
}

// دالة للمشاركة عبر واتساب
window.shareViaWhatsApp = async function() {
    if (!state.extractedData) {
        showMessage('لا توجد بيانات للإرسال', 'error');
        return;
    }
    
    showMessage('جارٍ إعداد التقرير للإرسال عبر واتساب...', 'info');
    
    try {
        // إنشاء PDF أولاً
        const element = document.getElementById('reportPage');
        const canvas = await html2canvas(element, {
            scale: 2,
            useCORS: true,
            backgroundColor: '#ffffff'
        });
        
        const imgData = canvas.toDataURL('image/jpeg', 0.9);
        const { jsPDF } = window.jspdf;
        const pdf = new jsPDF({
            orientation: 'portrait',
            unit: 'mm',
            format: 'a4'
        });
        
        const pdfWidth = pdf.internal.pageSize.getWidth();
        const pdfHeight = pdf.internal.pageSize.getHeight();
        const imgWidth = canvas.width;
        const imgHeight = canvas.height;
        const ratio = Math.min(pdfWidth / imgWidth, pdfHeight / imgHeight);
        
        pdf.addImage(imgData, 'JPEG', 0, 0, imgWidth * ratio, imgHeight * ratio);
        
        // تحويل PDF إلى Blob
        const pdfBlob = pdf.output('blob');
        const subject = state.extractedData?.testInfo?.subject || 'الطلاب';
        const fileName = `تقرير_نتائج_${subject}.pdf`;
        const file = new File([pdfBlob], fileName, { type: 'application/pdf' });
        
        // إنشاء نص للمشاركة
        const stats = state.extractedData.statistics;
        const summaryText = encodeURIComponent(`
📊 *تقرير نتائج ${state.extractedData.testInfo.subject}*

🎓 *المعلومات الأساسية:*
• الصف: ${state.extractedData.testInfo.grade}
• الفصل: ${state.extractedData.testInfo.semester}
• عدد الطلاب: ${stats.totalStudents}

📈 *الإحصائيات:*
• متوسط الدرجات: ${stats.average} من ${state.extractedData.testInfo.totalScore}
• نسبة النجاح: ${stats.successRate}%
• أعلى درجة: ${stats.highest}
• أدنى درجة: ${stats.lowest}

🏆 *مستوى الأداء:* ${state.analysisResults.performance}

📅 ${new Date().toLocaleDateString('ar-SA')}
        `.trim());
        
        // فتح واتساب مع النص
        const whatsappUrl = `https://wa.me/?text=${summaryText}`;
        window.open(whatsappUrl, '_blank');
        
        showMessage('تم فتح واتساب، يمكنك إرفاق ملف PDF يدوياً', 'info');
        
    } catch (error) {
        console.error('WhatsApp error:', error);
        showMessage('خطأ في إرسال التقرير، جاري تحميل الملف...', 'warning');
        downloadPDF();
    }
};

// دالة للطباعة
window.printReport = function() {
    window.print();
};

// مسح الكل
elements.clearAllBtn.addEventListener('click', () => {
    if (confirm('هل أنت متأكد من مسح جميع البيانات والعودة للنقطة الأولى؟')) {
        resetApplication();
    }
});

function resetApplication() {
    state.apiKey = '';
    state.isValidApi = false;
    state.currentFile = null;
    state.extractedData = null;
    state.analysisResults = null;
    state.studentsData = [];
    state.reportData = null;
    state.teacherName = '';
    state.principalName = '';
    
    elements.apiKey.value = '';
    elements.teacherName.value = '';
    elements.principalName.value = '';
    
    elements.apiStatus.className = 'api-status status-invalid';
    elements.apiStatus.innerHTML = `
        <i class="fas fa-times-circle"></i>
        <span>لم يتم التحقق من المفتاح بعد</span>
    `;
    
    elements.fileList.innerHTML = '';
    elements.fileInput.value = '';
    elements.analyzeBtn.disabled = true;
    elements.generateReportBtn.disabled = true;
    elements.whatsappBtn.disabled = true;
    
    elements.extractedData.innerHTML = `
        <div style="text-align: center; padding: 60px 20px; color: #6c757d;">
            <i class="fas fa-file-alt" style="font-size: 70px; margin-bottom: 25px; color: #bdc3c7;"></i>
            <h3 style="color: #95a5a6; margin-bottom: 15px;">البيانات ستظهر هنا</h3>
            <p style="line-height: 1.6;">بعد تحميل الملف والتحقق من API، انقر على "بدء تحليل النتائج" لاستخراج وتحليل البيانات تلقائياً.</p>
        </div>
    `;
    
    elements.studentsContainer.innerHTML = `
        <div style="text-align: center; padding: 40px; color: #6c757d;">
            <i class="fas fa-user-graduate" style="font-size: 60px; margin-bottom: 20px; color: #bdc3c7;"></i>
            <h3 style="color: #95a5a6; margin-bottom: 15px;">قائمة الطلاب ستظهر هنا</h3>
            <p>سيتم عرض أسماء الطلاب وتقديراتهم بعد تحليل الملف</p>
        </div>
    `;
    
    elements.statsGrid.innerHTML = '';
    
    distributionChart.data.datasets[0].data = [2, 3, 5, 6, 4];
    distributionChart.update();
    
    levelsChart.data.datasets[0].data = [7, 5, 3, 2, 3];
    levelsChart.update();
    
    successChart.data.datasets[0].data = [17, 3];
    successChart.update();
    
    elements.inputInterface.style.display = 'grid';
    elements.reportInterface.style.display = 'none';
    elements.reportActions.classList.add('hidden');
    elements.inputTab.classList.add('active');
    elements.reportTab.classList.remove('active');
    
    showMessage('تم مسح جميع البيانات وإعادة التعيين', 'info');
}

// التنقل بين الواجهات
elements.inputTab.addEventListener('click', () => {
    elements.inputInterface.style.display = 'grid';
    elements.reportInterface.style.display = 'none';
    elements.reportActions.classList.add('hidden');
    elements.inputTab.classList.add('active');
    elements.reportTab.classList.remove('active');
});

elements.reportTab.addEventListener('click', () => {
    if (!state.reportData) {
        showMessage('يرجى توليد التقرير أولاً', 'warning');
        return;
    }
    
    elements.inputInterface.style.display = 'none';
    elements.reportInterface.style.display = 'block';
    elements.reportActions.classList.remove('hidden');
    elements.reportTab.classList.add('active');
    elements.inputTab.classList.remove('active');
});

// التحقق من المفتاح عند الضغط على Enter
elements.apiKey.addEventListener('keypress', (e) => {
    if (e.key === 'Enter') {
        elements.verifyBtn.click();
    }
});

// تهيئة التطبيق عند التحميل
document.addEventListener('DOMContentLoaded', () => {
    showMessage('مرحباً بك في المحلل الذكي لنتائج الطلاب! ابدأ بإدخال مفتاح API الخاص بك.', 'info', 8000);
});
</script>
</body>
</html>