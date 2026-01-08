<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <title>مستخرج النصوص الذكي | الحفاظ على التنسيق والجداول</title>
    <style>
        body { 
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; 
            background: linear-gradient(135deg, #f0f2f5 0%, #f0f8ff 100%);
            display: flex; 
            justify-content: center; 
            align-items: center;
            min-height: 100vh;
            padding: 20px;
            margin: 0;
        }
        .card { 
            background: white; 
            width: 100%; 
            max-width: 800px; 
            padding: 30px; 
            border-radius: 16px; 
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.15); 
        }
        h2 { 
            color: #2c3e50; 
            text-align: center; 
            margin-bottom: 10px;
            font-size: 28px;
            background: linear-gradient(to right, #3498db, #2c3e50);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
        }
        .subtitle {
            text-align: center;
            color: #7f8c8d;
            margin-bottom: 30px;
            font-size: 16px;
            line-height: 1.5;
        }
        .config-section {
            background: linear-gradient(135deg, #f8f9fa 0%, #e9ecef 100%);
            padding: 20px;
            border-radius: 12px;
            border: 1px solid #dee2e6;
            margin-bottom: 25px;
            box-shadow: 0 3px 10px rgba(0,0,0,0.05);
        }
        .section-title {
            color: #2c3e50;
            margin-top: 0;
            margin-bottom: 20px;
            font-size: 18px;
            display: flex;
            align-items: center;
            gap: 10px;
        }
        .section-title i {
            color: #3498db;
        }
        .api-config {
            display: grid;
            grid-template-columns: 1fr auto;
            gap: 15px;
            margin-bottom: 15px;
        }
        .api-input {
            padding: 12px 15px;
            border: 2px solid #ddd;
            border-radius: 8px;
            font-size: 16px;
            font-family: 'Courier New', monospace;
            transition: all 0.3s;
        }
        .api-input:focus {
            outline: none;
            border-color: #3498db;
            box-shadow: 0 0 0 3px rgba(52, 152, 219, 0.1);
        }
        .btn-save {
            background: linear-gradient(to right, #27ae60, #219653);
            color: white;
            border: none;
            padding: 12px 25px;
            border-radius: 8px;
            cursor: pointer;
            font-weight: bold;
            transition: all 0.3s;
        }
        .btn-save:hover {
            background: linear-gradient(to right, #219653, #1e8449);
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(33, 150, 83, 0.3);
        }
        .api-status {
            padding: 10px;
            border-radius: 6px;
            font-size: 14px;
            display: flex;
            align-items: center;
            gap: 10px;
            margin-top: 10px;
        }
        .status-valid {
            background: #d4edda;
            color: #155724;
            border: 1px solid #c3e6cb;
        }
        .status-invalid {
            background: #f8d7da;
            color: #721c24;
            border: 1px solid #f5c6cb;
        }
        .model-config {
            display: grid;
            grid-template-columns: 1fr auto;
            gap: 15px;
            margin-top: 20px;
        }
        .model-select {
            padding: 12px 15px;
            border: 2px solid #ddd;
            border-radius: 8px;
            font-size: 16px;
            background: white;
            cursor: pointer;
        }
        .btn-test {
            background: linear-gradient(to right, #3498db, #2980b9);
            color: white;
            border: none;
            padding: 12px 20px;
            border-radius: 8px;
            cursor: pointer;
            font-weight: bold;
            transition: all 0.3s;
        }
        .btn-test:hover {
            background: linear-gradient(to right, #2980b9, #1f618d);
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(52, 152, 219, 0.3);
        }
        .upload-area { 
            border: 3px dashed #3498db; 
            padding: 40px 20px; 
            text-align: center; 
            cursor: pointer; 
            border-radius: 12px; 
            background: linear-gradient(135deg, #f8fbff 0%, #e6f2ff 100%); 
            margin-bottom: 25px;
            transition: all 0.3s;
            position: relative;
        }
        .upload-area:hover {
            background: linear-gradient(135deg, #e6f2ff 0%, #d4e6ff 100%);
            border-color: #2980b9;
        }
        .upload-area.dragover {
            background: linear-gradient(135deg, #d4edda 0%, #c3e6cb 100%);
            border-color: #27ae60;
        }
        .upload-icon {
            font-size: 48px;
            color: #3498db;
            margin-bottom: 15px;
        }
        .upload-text {
            font-size: 18px;
            color: #2c3e50;
            margin-bottom: 10px;
            font-weight: 500;
        }
        .upload-info {
            color: #7f8c8d;
            font-size: 14px;
            margin-top: 10px;
        }
        #result { 
            background: #f8f9fa; 
            padding: 25px; 
            border-radius: 12px; 
            min-height: 250px; 
            border: 1px solid #dee2e6; 
            margin-top: 25px;
            line-height: 1.8;
            max-height: 500px;
            overflow-y: auto;
            font-family: 'Arial', sans-serif;
        }
        .result-table {
            border-collapse: collapse;
            width: 100%;
            margin: 15px 0;
            border: 1px solid #ddd;
            background: white;
        }
        .result-table th {
            background: #f2f2f2;
            padding: 12px;
            text-align: right;
            border: 1px solid #ddd;
            font-weight: bold;
        }
        .result-table td {
            padding: 10px;
            border: 1px solid #ddd;
            text-align: right;
        }
        .result-list {
            padding-right: 20px;
            margin: 15px 0;
        }
        .result-list li {
            margin-bottom: 8px;
        }
        .btn-extract { 
            background: linear-gradient(to right, #e74c3c, #c0392b); 
            color: white; 
            border: none; 
            padding: 16px; 
            border-radius: 10px; 
            width: 100%; 
            cursor: pointer; 
            font-size: 18px;
            font-weight: bold;
            transition: all 0.3s;
            margin-top: 10px;
        }
        .btn-extract:hover:not(:disabled) {
            background: linear-gradient(to right, #c0392b, #a93226);
            transform: translateY(-3px);
            box-shadow: 0 8px 20px rgba(231, 76, 60, 0.3);
        }
        .btn-extract:disabled { 
            background: #bdc3c7; 
            cursor: not-allowed;
            transform: none;
        }
        .action-buttons {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 15px;
            margin-top: 20px;
        }
        .btn-copy {
            background: linear-gradient(to right, #9b59b6, #8e44ad);
            color: white;
            border: none;
            padding: 14px;
            border-radius: 8px;
            cursor: pointer;
            font-weight: bold;
            transition: all 0.3s;
        }
        .btn-copy:hover:not(:disabled) {
            background: linear-gradient(to right, #8e44ad, #7d3c98);
            transform: translateY(-2px);
        }
        .btn-clear {
            background: linear-gradient(to right, #95a5a6, #7f8c8d);
            color: white;
            border: none;
            padding: 14px;
            border-radius: 8px;
            cursor: pointer;
            font-weight: bold;
            transition: all 0.3s;
        }
        .btn-clear:hover {
            background: linear-gradient(to right, #7f8c8d, #6c7b7d);
            transform: translateY(-2px);
        }
        .stats-bar {
            display: flex;
            justify-content: space-between;
            margin-top: 15px;
            padding: 10px 15px;
            background: #f8f9fa;
            border-radius: 8px;
            border: 1px solid #e9ecef;
            font-size: 14px;
            color: #6c757d;
        }
        .loading {
            display: inline-block;
            width: 20px;
            height: 20px;
            border: 3px solid rgba(52, 152, 219, 0.3);
            border-radius: 50%;
            border-top-color: #3498db;
            animation: spin 1s ease-in-out infinite;
        }
        @keyframes spin {
            to { transform: rotate(360deg); }
        }
        .format-options {
            background: #fff8e1;
            padding: 15px;
            border-radius: 8px;
            margin-top: 15px;
            border: 1px solid #ffd54f;
        }
        .format-title {
            color: #e65100;
            margin-top: 0;
            font-size: 16px;
            display: flex;
            align-items: center;
            gap: 8px;
        }
        .format-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 10px;
            margin-top: 10px;
        }
        .format-option {
            display: flex;
            align-items: center;
            gap: 8px;
        }
        .format-option input[type="checkbox"] {
            width: 18px;
            height: 18px;
            cursor: pointer;
        }
        .hidden {
            display: none !important;
        }
        .progress-bar {
            height: 5px;
            background: #3498db;
            border-radius: 3px;
            margin-top: 10px;
            transition: width 0.3s;
            width: 0%;
        }
        .models-list {
            max-height: 200px;
            overflow-y: auto;
            margin-top: 10px;
            padding: 10px;
            background: #f8f9fa;
            border-radius: 8px;
            border: 1px solid #dee2e6;
            display: none;
        }
        .models-list.show {
            display: block;
        }
        .model-item {
            padding: 8px;
            border-bottom: 1px solid #e9ecef;
            cursor: pointer;
            transition: background 0.2s;
        }
        .model-item:hover {
            background: #e9ecef;
        }
        .model-name {
            font-weight: bold;
            color: #2c3e50;
        }
        .model-desc {
            font-size: 12px;
            color: #7f8c8d;
        }
        .footer {
            text-align: center;
            margin-top: 25px;
            color: #95a5a6;
            font-size: 14px;
            border-top: 1px solid #ecf0f1;
            padding-top: 15px;
        }
    </style>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
</head>
<body>

<div class="card">
    <h2><i class="fas fa-file-alt"></i> مستخرج النصوص الذكي</h2>
    <p class="subtitle">استخرج النصوص من ملفات PDF والصور مع الحفاظ الكامل على التنسيق والجداول والعناصر الهيكلية</p>
    
    <div class="config-section">
        <div class="section-title">
            <i class="fas fa-key"></i>
            <span>إعدادات API</span>
        </div>
        
        <div class="api-config">
            <input type="password" id="apiKeyInput" class="api-input" placeholder="أدخل مفتاح Google Gemini API هنا..." value="">
            <button id="saveApiBtn" class="btn-save">
                <i class="fas fa-save"></i> حفظ المفتاح
            </button>
        </div>
        
        <div id="apiStatus" class="api-status status-invalid">
            <i class="fas fa-times-circle"></i>
            <span id="apiStatusText">لم يتم إضافة مفتاح API بعد</span>
        </div>
        
        <div class="model-config">
            <select id="modelSelect" class="model-select">
                <option value="gemini-1.5-flash">Gemini 1.5 Flash (سريع ومنخفض التكلفة)</option>
                <option value="gemini-1.5-pro">Gemini 1.5 Pro (متوازن للجداول)</option>
                <option value="gemini-pro">Gemini Pro (للنصوص المعقدة)</option>
                <option value="gemini-pro-vision">Gemini Pro Vision (للصور والجداول)</option>
            </select>
            <button id="testConnectionBtn" class="btn-test hidden">
                <i class="fas fa-wifi"></i> اختبار الاتصال
            </button>
        </div>
        
        <div id="modelsList" class="models-list"></div>
        
        <div class="format-options">
            <div class="format-title">
                <i class="fas fa-cogs"></i>
                <span>خيارات تنسيق الاستخراج</span>
            </div>
            <div class="format-grid">
                <div class="format-option">
                    <input type="checkbox" id="preserveTables" checked>
                    <label for="preserveTables">الحفاظ على الجداول</label>
                </div>
                <div class="format-option">
                    <input type="checkbox" id="preserveLists" checked>
                    <label for="preserveLists">الحفاظ على القوائم</label>
                </div>
                <div class="format-option">
                    <input type="checkbox" id="preserveFormatting" checked>
                    <label for="preserveFormatting">الحفاظ على التنسيق</label>
                </div>
                <div class="format-option">
                    <input type="checkbox" id="preserveStructure" checked>
                    <label for="preserveStructure">الحفاظ على الهيكل</label>
                </div>
            </div>
        </div>
    </div>
    
    <div class="upload-area" id="dropZone">
        <div class="upload-icon">
            <i class="fas fa-cloud-upload-alt"></i>
        </div>
        <div class="upload-text" id="fileLabel">اسحب ملف PDF أو صورة هنا أو انقر للاختيار</div>
        <div class="upload-info" id="fileInfo">الحد الأقصى: 10MB | المدعوم: PDF, JPG, PNG, GIF, BMP</div>
        <input type="file" id="fileInput" accept="application/pdf,image/*" style="display:none">
    </div>
    
    <div class="progress-bar" id="progressBar"></div>
    
    <button id="btn" class="btn-extract" disabled>
        <i class="fas fa-magic"></i> بدء استخراج النصوص
    </button>
    
    <div class="stats-bar">
        <div id="charCount">عدد الأحرف: 0</div>
        <div id="wordCount">عدد الكلمات: 0</div>
        <div id="processingTime">زمن المعالجة: 0 ثانية</div>
    </div>
    
    <div class="action-buttons">
        <button id="copyBtn" class="btn-copy" disabled>
            <i class="fas fa-copy"></i> نسخ النص
        </button>
        <button id="clearBtn" class="btn-clear">
            <i class="fas fa-trash-alt"></i> مسح الكل
        </button>
    </div>
    
    <div id="result">
        <div style="text-align: center; color: #7f8c8d; padding: 40px;">
            <i class="fas fa-file-alt" style="font-size: 48px; margin-bottom: 15px; color: #bdc3c7;"></i>
            <h3 style="color: #95a5a6;">النتائج ستظهر هنا</h3>
            <p style="margin-top: 10px;">بعد تحميل الملف واختيار إعدادات التنسيق، انقر على "بدء استخراج النصوص" لاستخراج المحتوى مع الحفاظ على الجداول والتنسيق الأصلي.</p>
        </div>
    </div>
    
    <div class="footer">
        <i class="fas fa-code"></i> مستخرج النصوص الذكي | يدعم الجداول والتنسيقات المعقدة | v3.0
    </div>
</div>

<script type="module">
    // استخدام إصدار محدد من المكتبة
    import { GoogleGenerativeAI } from "https://esm.run/@google/generative-ai@0.1.0";

    // عناصر واجهة المستخدم
    const elements = {
        btn: document.getElementById('btn'),
        resultDiv: document.getElementById('result'),
        fileInput: document.getElementById('fileInput'),
        dropZone: document.getElementById('dropZone'),
        fileLabel: document.getElementById('fileLabel'),
        fileInfo: document.getElementById('fileInfo'),
        apiKeyInput: document.getElementById('apiKeyInput'),
        saveApiBtn: document.getElementById('saveApiBtn'),
        apiStatus: document.getElementById('apiStatus'),
        apiStatusText: document.getElementById('apiStatusText'),
        copyBtn: document.getElementById('copyBtn'),
        clearBtn: document.getElementById('clearBtn'),
        charCount: document.getElementById('charCount'),
        wordCount: document.getElementById('wordCount'),
        processingTime: document.getElementById('processingTime'),
        modelSelect: document.getElementById('modelSelect'),
        testConnectionBtn: document.getElementById('testConnectionBtn'),
        modelsList: document.getElementById('modelsList'),
        progressBar: document.getElementById('progressBar'),
        preserveTables: document.getElementById('preserveTables'),
        preserveLists: document.getElementById('preserveLists'),
        preserveFormatting: document.getElementById('preserveFormatting'),
        preserveStructure: document.getElementById('preserveStructure')
    };

    // بيانات التطبيق
    const appState = {
        API_KEY: localStorage.getItem('gemini_api_key') || '',
        SELECTED_MODEL: localStorage.getItem('selected_model') || 'gemini-1.5-flash',
        isProcessing: false,
        startTime: null
    };

    // تهيئة التطبيق
    function initApp() {
        // تحميل الإعدادات المحفوظة
        if (appState.API_KEY) {
            elements.apiKeyInput.value = "••••••••" + appState.API_KEY.slice(-4);
            elements.modelSelect.value = appState.SELECTED_MODEL;
            updateApiStatus(true);
            elements.testConnectionBtn.classList.remove('hidden');
        }
        
        // تحديث الإحصائيات
        updateStats();
    }

    // تحديث حالة API
    function updateApiStatus(isValid) {
        if (isValid && appState.API_KEY) {
            elements.apiStatus.className = 'api-status status-valid';
            elements.apiStatusText.innerHTML = '<i class="fas fa-check-circle"></i> مفتاح API صالح ومحفوظ';
            elements.btn.disabled = false;
            elements.testConnectionBtn.classList.remove('hidden');
        } else {
            elements.apiStatus.className = 'api-status status-invalid';
            elements.apiStatusText.innerHTML = '<i class="fas fa-times-circle"></i> يرجى إضافة مفتاح API صالح';
            elements.btn.disabled = true;
        }
    }

    // تحديث الإحصائيات
    function updateStats() {
        const text = elements.resultDiv.innerText;
        const charCount = text.replace(/\s/g, '').length;
        const wordCount = text.trim().split(/\s+/).filter(word => word.length > 0).length;
        
        elements.charCount.textContent = `عدد الأحرف: ${charCount}`;
        elements.wordCount.textContent = `عدد الكلمات: ${wordCount}`;
    }

    // حفظ مفتاح API
    elements.saveApiBtn.addEventListener('click', () => {
        const inputKey = elements.apiKeyInput.value.trim();
        
        // إذا كان الحقل يحتوي على نقاط (مفتاح مخفي)
        if (inputKey.includes('••••')) {
            updateApiStatus(true);
            return;
        }
        
        if (inputKey === '') {
            // مسح المفتاح
            localStorage.removeItem('gemini_api_key');
            appState.API_KEY = '';
            elements.apiKeyInput.value = '';
            updateApiStatus(false);
            showMessage('تم مسح مفتاح API بنجاح', 'success');
            return;
        }
        
        // التحقق من شكل مفتاح API
        if (!inputKey.startsWith('AIza')) {
            showMessage('يبدو أن مفتاح API غير صحيح. يجب أن يبدأ المفتاح بـ "AIza"', 'error');
            return;
        }
        
        // حفظ المفتاح الجديد
        appState.API_KEY = inputKey;
        appState.SELECTED_MODEL = elements.modelSelect.value;
        
        localStorage.setItem('gemini_api_key', appState.API_KEY);
        localStorage.setItem('selected_model', appState.SELECTED_MODEL);
        
        elements.apiKeyInput.value = "••••••••" + appState.API_KEY.slice(-4);
        updateApiStatus(true);
        
        showMessage('تم حفظ مفتاح API بنجاح!', 'success');
    });

    // اختبار الاتصال
    elements.testConnectionBtn.addEventListener('click', async () => {
        if (!appState.API_KEY) {
            showMessage('يرجى إضافة مفتاح API أولاً', 'error');
            return;
        }
        
        elements.testConnectionBtn.innerHTML = '<span class="loading"></span> جاري الاختبار...';
        elements.testConnectionBtn.disabled = true;
        
        try {
            const response = await fetch(`https://generativelanguage.googleapis.com/v1/models?key=${appState.API_KEY}`);
            
            if (!response.ok) throw new Error(`خطأ في الاتصال: ${response.status}`);
            
            const data = await response.json();
            
            if (data.models && data.models.length > 0) {
                const geminiModels = data.models.filter(m => 
                    m.name.includes('gemini') && 
                    m.supportedGenerationMethods?.includes('generateContent')
                );
                
                displayAvailableModels(geminiModels);
                showMessage(`✅ تم العثور على ${geminiModels.length} نموذج متاح`, 'success');
            } else {
                showMessage('⚠️ لا توجد نماذج متاحة في حسابك', 'warning');
            }
        } catch (error) {
            console.error("Connection test error:", error);
            showMessage(`❌ فشل اختبار الاتصال: ${error.message}`, 'error');
        } finally {
            elements.testConnectionBtn.innerHTML = '<i class="fas fa-wifi"></i> اختبار الاتصال';
            elements.testConnectionBtn.disabled = false;
        }
    });

    // عرض النماذج المتاحة
    function displayAvailableModels(models) {
        elements.modelsList.innerHTML = '';
        
        if (models.length === 0) {
            elements.modelsList.innerHTML = '<div style="text-align: center; padding: 20px; color: #7f8c8d;">لا توجد نماذج متاحة</div>';
        } else {
            models.forEach(model => {
                const modelName = model.name.split('/').pop();
                const div = document.createElement('div');
                div.className = 'model-item';
                div.innerHTML = `
                    <div class="model-name">${modelName}</div>
                    <div class="model-desc">${model.description || 'نموذج Gemini للذكاء الاصطناعي'}</div>
                `;
                div.addEventListener('click', () => {
                    elements.modelSelect.value = modelName;
                    appState.SELECTED_MODEL = modelName;
                    localStorage.setItem('selected_model', modelName);
                    elements.modelsList.classList.remove('show');
                });
                elements.modelsList.appendChild(div);
            });
        }
        
        elements.modelsList.classList.add('show');
    }

    // إدارة تحميل الملفات
    elements.dropZone.addEventListener('click', () => elements.fileInput.click());
    
    elements.fileInput.addEventListener('change', handleFileSelect);
    
    function handleFileSelect() {
        if (elements.fileInput.files[0]) {
            const file = elements.fileInput.files[0];
            const fileSize = (file.size / 1024 / 1024).toFixed(2);
            const fileName = file.name.length > 30 ? file.name.substring(0, 27) + '...' : file.name;
            
            elements.fileLabel.innerHTML = `
                <i class="fas fa-file"></i> ${fileName}
            `;
            elements.fileInfo.innerHTML = `
                <i class="fas fa-info-circle"></i> حجم الملف: ${fileSize} MB | النوع: ${file.type}
            `;
        }
    }

    // سحب وإفلات الملفات
    ['dragover', 'dragleave', 'drop'].forEach(eventName => {
        elements.dropZone.addEventListener(eventName, (e) => {
            e.preventDefault();
            e.stopPropagation();
            
            if (eventName === 'dragover') {
                elements.dropZone.classList.add('dragover');
            } else if (eventName === 'dragleave' || eventName === 'drop') {
                elements.dropZone.classList.remove('dragover');
                
                if (eventName === 'drop' && e.dataTransfer.files.length) {
                    elements.fileInput.files = e.dataTransfer.files;
                    handleFileSelect();
                }
            }
        });
    });

    // نسخ النص
    elements.copyBtn.addEventListener('click', async () => {
        const textToCopy = elements.resultDiv.innerText;
        
        try {
            await navigator.clipboard.writeText(textToCopy);
            showMessage('تم نسخ النص إلى الحافظة', 'success');
            
            elements.copyBtn.innerHTML = '<i class="fas fa-check"></i> تم النسخ!';
            setTimeout(() => {
                elements.copyBtn.innerHTML = '<i class="fas fa-copy"></i> نسخ النص';
            }, 2000);
        } catch (err) {
            showMessage('فشل نسخ النص', 'error');
        }
    });

    // مسح النتائج
    elements.clearBtn.addEventListener('click', () => {
        elements.resultDiv.innerHTML = `
            <div style="text-align: center; color: #7f8c8d; padding: 40px;">
                <i class="fas fa-file-alt" style="font-size: 48px; margin-bottom: 15px; color: #bdc3c7;"></i>
                <h3 style="color: #95a5a6;">النتائج ستظهر هنا</h3>
                <p style="margin-top: 10px;">بعد تحميل الملف واختيار إعدادات التنسيق، انقر على "بدء استخراج النصوص" لاستخراج المحتوى مع الحفاظ على الجداول والتنسيق الأصلي.</p>
            </div>
        `;
        
        elements.fileInput.value = '';
        elements.fileLabel.innerHTML = 'اسحب ملف PDF أو صورة هنا أو انقر للاختيار';
        elements.fileInfo.innerHTML = 'الحد الأقصى: 10MB | المدعوم: PDF, JPG, PNG, GIF, BMP';
        elements.copyBtn.disabled = true;
        updateStats();
        elements.processingTime.textContent = 'زمن المعالجة: 0 ثانية';
        elements.modelsList.classList.remove('show');
    });

    // استخراج النصوص
    elements.btn.addEventListener('click', async () => {
        if (!appState.API_KEY) {
            showMessage('يرجى إضافة مفتاح API أولاً', 'error');
            return;
        }
        
        if (!elements.fileInput.files[0]) {
            showMessage('الرجاء اختيار ملف أولاً', 'error');
            return;
        }
        
        const file = elements.fileInput.files[0];
        const maxSize = 10 * 1024 * 1024;
        
        if (file.size > maxSize) {
            showMessage('حجم الملف كبير جداً. الحد الأقصى هو 10MB', 'error');
            return;
        }
        
        // بدء المعالجة
        appState.isProcessing = true;
        appState.startTime = Date.now();
        
        elements.btn.disabled = true;
        elements.btn.innerHTML = '<span class="loading"></span> جاري استخراج النصوص...';
        elements.copyBtn.disabled = true;
        
        // عرض شريط التقدم
        elements.progressBar.style.width = '30%';
        
        // تحضير النص التوضيحي حسب خيارات التنسيق
        const formatOptions = {
            tables: elements.preserveTables.checked,
            lists: elements.preserveLists.checked,
            formatting: elements.preserveFormatting.checked,
            structure: elements.preserveStructure.checked
        };
        
        const promptText = buildPrompt(formatOptions);
        
        try {
            const genAI = new GoogleGenerativeAI(appState.API_KEY);
            const model = genAI.getGenerativeModel({ model: appState.SELECTED_MODEL });
            
            // تحديث شريط التقدم
            elements.progressBar.style.width = '60%';
            
            const reader = new FileReader();
            
            reader.onloadend = async () => {
                try {
                    const base64Data = reader.result.split(',')[1];
                    
                    // تحديث شريط التقدم
                    elements.progressBar.style.width = '80%';
                    
                    const result = await model.generateContent([
                        promptText,
                        { inlineData: { data: base64Data, mimeType: file.type } }
                    ]);
                    
                    const response = await result.response;
                    const extractedText = response.text();
                    
                    // عرض النتائج مع تنسيق HTML
                    displayFormattedResults(extractedText);
                    
                    // إكمال شريط التقدم
                    elements.progressBar.style.width = '100%';
                    
                    // حساب زمن المعالجة
                    const processingTime = ((Date.now() - appState.startTime) / 1000).toFixed(2);
                    elements.processingTime.textContent = `زمن المعالجة: ${processingTime} ثانية`;
                    
                    // تمكين الأزرار
                    elements.copyBtn.disabled = false;
                    
                    // إخفاء قائمة النماذج
                    elements.modelsList.classList.remove('show');
                    
                    // رسالة نجاح
                    showMessage('تم استخراج النصوص بنجاح مع الحفاظ على التنسيق!', 'success');
                    
                } catch (apiError) {
                    handleApiError(apiError);
                } finally {
                    finishProcessing();
                }
            };
            
            reader.onerror = () => {
                showMessage('حدث خطأ أثناء قراءة الملف', 'error');
                finishProcessing();
            };
            
            reader.readAsDataURL(file);
            
        } catch (error) {
            console.error("General error:", error);
            showMessage(`حدث خطأ: ${error.message}`, 'error');
            finishProcessing();
        }
    });

    // بناء النص التوضيحي حسب خيارات التنسيق
    function buildPrompt(options) {
        let prompt = "اقرأ الملف المرفق واستخرج النص منه بدقة عالية جداً. ";
        
        if (options.structure) {
            prompt += "حافظ على الهيكل الأصلي للمستند بما في ذلك العناوين الرئيسية والفرعية. ";
        }
        
        if (options.formatting) {
            prompt += "حافظ على التنسيق مثل الخطوط الغامقة والمائلة والتسطير. ";
        }
        
        if (options.tables) {
            prompt += "بالنسبة للجداول: استخرجها بدقة مع الحفاظ على ترتيب الأعمدة والصفوف. استخدم علامات الجداول | لفصل الأعمدة و - لفصل الصفوف. لا تفقد أي بيانات من الجداول. ";
        }
        
        if (options.lists) {
            prompt += "بالنسبة للقوائم: حافظ على التسلسل الرقمي أو النقطي. استخدم * للقوائم النقطية والأرقام للقوائم المرقمة. ";
        }
        
        prompt += "إذا كان هناك أي أرقام أو تواريخ أو أسماء خاصة، تأكد من استخراجها بدقة. ";
        prompt += "لا تضيف أي تعليقات أو تفسيرات إضافية، فقط استخرج النص كما هو مع الحفاظ على الهيكل والتنسيق. ";
        
        return prompt;
    }

    // عرض النتائج مع تنسيق HTML
    function displayFormattedResults(text) {
        // معالجة النص للحفاظ على التنسيق
        let formattedText = text;
        
        // تحويل الجداول إلى HTML
        formattedText = formatTables(formattedText);
        
        // تحويل القوائم إلى HTML
        formattedText = formatLists(formattedText);
        
        // تحويل العناوين
        formattedText = formatHeadings(formattedText);
        
        // تحويل النصوص الغامقة والمائلة
        formattedText = formatTextStyles(formattedText);
        
        // إضافة تنسيق HTML الأساسي
        const htmlContent = `
            <div style="font-family: 'Arial', sans-serif; line-height: 1.8;">
                <div style="background: #f8f9fa; padding: 15px; border-radius: 8px; margin-bottom: 20px; border-right: 4px solid #3498db;">
                    <h3 style="color: #2c3e50; margin-top: 0;">
                        <i class="fas fa-check-circle" style="color: #27ae60;"></i>
                        تم استخراج النصوص بنجاح
                    </h3>
                    <p style="color: #7f8c8d; margin: 0;">
                        تم الحفاظ على التنسيق الأصلي بما في ذلك الجداول والقوائم والعناوين.
                    </p>
                </div>
                <div style="background: white; padding: 20px; border-radius: 8px; border: 1px solid #e9ecef;">
                    ${formattedText}
                </div>
            </div>
        `;
        
        elements.resultDiv.innerHTML = htmlContent;
        updateStats();
    }

    // تنسيق الجداول
    function formatTables(text) {
        // نمط الجداول البسيطة بـ | و -
        const tableRegex = /(\|.*\|\n)+(\|-.*\-\|?\n)?/g;
        return text.replace(tableRegex, (match) => {
            const rows = match.trim().split('\n').filter(row => row.trim());
            let html = '<div style="overflow-x: auto; margin: 15px 0;">';
            html += '<table class="result-table">';
            
            rows.forEach((row, index) => {
                // تحديد إذا كان هذا الصف رأس الجدول
                const isHeaderRow = row.includes('---') || index === 0;
                const cells = row.split('|').filter(cell => cell.trim() !== '');
                
                html += '<tr>';
                cells.forEach(cell => {
                    const content = cell.trim();
                    if (isHeaderRow && row.includes('---')) {
                        // تخطي صف الفواصل
                        return;
                    }
                    if (isHeaderRow && !row.includes('---')) {
                        html += `<th>${content}</th>`;
                    } else {
                        html += `<td>${content}</td>`;
                    }
                });
                html += '</tr>';
            });
            
            html += '</table></div>';
            return html;
        });
    }

    // تنسيق القوائم
    function formatLists(text) {
        // القوائم النقطية
        text = text.replace(/^\s*[-•*]\s+(.+)$/gm, '<li>$1</li>');
        
        // القوائم المرقمة
        text = text.replace(/^\s*\d+\.\s+(.+)$/gm, '<li>$1</li>');
        
        // إذا كان هناك أي عناصر قائمة، ضعها في ul أو ol
        if (text.includes('<li>')) {
            text = '<ul class="result-list">' + text + '</ul>';
        }
        
        return text;
    }

    // تنسيق العناوين
    function formatHeadings(text) {
        // العناوين الرئيسية
        text = text.replace(/^(#+)\s+(.+)$/gm, (match, hashes, content) => {
            const level = hashes.length;
            return `<h${level} style="color: #2c3e50; margin-top: 20px; margin-bottom: 10px;">${content}</h${level}>`;
        });
        
        // العناوين المميزة بنص غامق
        text = text.replace(/^\s*(.+)\n[-=]+\s*$/gm, '<h2 style="color: #3498db; border-bottom: 2px solid #3498db; padding-bottom: 5px;">$1</h2>');
        
        return text;
    }

    // تنسيق النصوص الغامقة والمائلة
    function formatTextStyles(text) {
        // نص غامق
        text = text.replace(/\*\*(.+?)\*\*/g, '<strong>$1</strong>');
        text = text.replace(/__(.+?)__/g, '<strong>$1</strong>');
        
        // نص مائل
        text = text.replace(/\*(.+?)\*/g, '<em>$1</em>');
        text = text.replace(/_(.+?)_/g, '<em>$1</em>');
        
        return text;
    }

    // معالجة أخطاء API
    function handleApiError(error) {
        console.error("API Error:", error);
        
        let errorMessage = "حدث خطأ أثناء معالجة الملف";
        
        if (error.message.includes("API key not valid") || error.message.includes("403")) {
            errorMessage = "مفتاح API غير صالح. يرجى التحقق من المفتاح وإعادة المحاولة.";
            updateApiStatus(false);
        } else if (error.message.includes("404") || error.message.includes("not found")) {
            errorMessage = `النموذج "${appState.SELECTED_MODEL}" غير متوفر. يرجى اختيار نموذج آخر من القائمة.`;
        } else if (error.message.includes("quota")) {
            errorMessage = "تم تجاوز حصة الاستخدام. يرجى التحقق من حسابك أو استخدام مفتاح آخر.";
        } else {
            errorMessage += ": " + error.message;
        }
        
        showMessage(errorMessage, 'error');
        elements.resultDiv.innerHTML = `
            <div style="text-align: center; color: #e74c3c; padding: 40px;">
                <i class="fas fa-exclamation-triangle" style="font-size: 48px; margin-bottom: 15px;"></i>
                <h3 style="color: #c0392b;">خطأ في المعالجة</h3>
                <p style="margin-top: 10px;">${errorMessage}</p>
            </div>
        `;
    }

    // إنهاء المعالجة
    function finishProcessing() {
        appState.isProcessing = false;
        elements.btn.disabled = false;
        elements.btn.innerHTML = '<i class="fas fa-magic"></i> بدء استخراج النصوص';
        
        // إعادة ضبط شريط التقدم بعد ثانية
        setTimeout(() => {
            elements.progressBar.style.width = '0%';
        }, 1000);
    }

    // عرض الرسائل
    function showMessage(message, type) {
        const colors = {
            success: '#27ae60',
            error: '#e74c3c',
            warning: '#f39c12',
            info: '#3498db'
        };
        
        const icon = {
            success: 'fa-check-circle',
            error: 'fa-exclamation-circle',
            warning: 'fa-exclamation-triangle',
            info: 'fa-info-circle'
        };
        
        // إنشاء عنصر الرسالة
        const messageDiv = document.createElement('div');
        messageDiv.style.cssText = `
            position: fixed;
            top: 20px;
            left: 50%;
            transform: translateX(-50%);
            background: white;
            padding: 15px 25px;
            border-radius: 8px;
            box-shadow: 0 5px 15px rgba(0,0,0,0.2);
            z-index: 1000;
            display: flex;
            align-items: center;
            gap: 10px;
            border-right: 4px solid ${colors[type]};
            animation: slideIn 0.3s ease-out;
        `;
        
        messageDiv.innerHTML = `
            <i class="fas ${icon[type]}" style="color: ${colors[type]}; font-size: 20px;"></i>
            <span>${message}</span>
        `;
        
        document.body.appendChild(messageDiv);
        
        // إزالة الرسالة بعد 3 ثواني
        setTimeout(() => {
            messageDiv.style.animation = 'slideOut 0.3s ease-in';
            setTimeout(() => {
                if (messageDiv.parentNode) {
                    messageDiv.parentNode.removeChild(messageDiv);
                }
            }, 300);
        }, 3000);
        
        // إضافة أنيميشن CSS
        if (!document.getElementById('message-animations')) {
            const style = document.createElement('style');
            style.id = 'message-animations';
            style.textContent = `
                @keyframes slideIn {
                    from { top: -100px; opacity: 0; }
                    to { top: 20px; opacity: 1; }
                }
                @keyframes slideOut {
                    from { top: 20px; opacity: 1; }
                    to { top: -100px; opacity: 0; }
                }
            `;
            document.head.appendChild(style);
        }
    }

    // تحديث حالة زر النسخ
    elements.resultDiv.addEventListener('input', () => {
        const hasText = elements.resultDiv.textContent.trim() !== '' && 
                       !elements.resultDiv.textContent.includes('النتائج ستظهر هنا') &&
                       !elements.resultDiv.textContent.includes('خطأ في المعالجة');
        
        elements.copyBtn.disabled = !hasText;
    });

    // تحديث النموذج المختار
    elements.modelSelect.addEventListener('change', () => {
        appState.SELECTED_MODEL = elements.modelSelect.value;
        localStorage.setItem('selected_model', appState.SELECTED_MODEL);
    });

    // عرض/إخفاء مفتاح API
    elements.apiKeyInput.addEventListener('focus', () => {
        if (appState.API_KEY && elements.apiKeyInput.value.includes('••••')) {
            elements.apiKeyInput.value = appState.API_KEY;
        }
    });
    
    elements.apiKeyInput.addEventListener('blur', () => {
        if (appState.API_KEY && !elements.apiKeyInput.value.includes('••••')) {
            elements.apiKeyInput.value = "••••••••" + appState.API_KEY.slice(-4);
        }
    });

    // تهيئة التطبيق
    initApp();
</script>
</body>
</html>
