
<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>منظومة دليل الأدوية والعيادات - مستشفى الحسين السلط الحكومي</title>
    
    <!-- الخطوط والمكتبات الخارجية -->
    <link href="https://fonts.googleapis.com/css2?family=Tajawal:wght@400;500;700;800;900&display=swap" rel="stylesheet">
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">

    <style>
        body { font-family: 'Tajawal', sans-serif; background: #090d16; color: #f8fafc; }
        .glass-panel { background: rgba(15, 23, 42, 0.75); backdrop-filter: blur(20px); border: 1px solid rgba(255, 255, 255, 0.08); }
        .glass-card { background: rgba(30, 41, 59, 0.4); backdrop-filter: blur(12px); border: 1px solid rgba(255, 255, 255, 0.05); }
        .clinic-pill { background: rgba(30, 41, 59, 0.6); border: 1px solid rgba(255, 255, 255, 0.1); transition: all 0.2s ease; }
        .clinic-pill:hover, .clinic-pill.active { background: linear-gradient(135deg, rgba(20, 184, 166, 0.25), rgba(6, 182, 212, 0.25)); border-color: #14b8a6; color: #5eead4; }
        .badge-avail { background: rgba(16, 185, 129, 0.15); color: #34d399; border: 1px solid rgba(52, 211, 153, 0.3); }
        .badge-out { background: rgba(239, 68, 68, 0.15); color: #f87171; border: 1px solid rgba(248, 113, 113, 0.3); }
        .badge-pending { background: rgba(245, 158, 11, 0.15); color: #fbbf24; border: 1px solid rgba(251, 191, 36, 0.3); }
        .glow-teal { box-shadow: 0 0 35px -5px rgba(20, 184, 166, 0.3); }
        .pulse-live { animation: pulse 2s cubic-bezier(0.4, 0, 0.6, 1) infinite; }
        @keyframes pulse { 0%, 100% { opacity: 1; } 50% { opacity: .4; } }
    </style>
</head>
<body class="min-h-screen flex flex-col justify-between selection:bg-teal-500 selection:text-white">

    <!-- نافذة تسجيل الدخول بالرمز السري -->
    <div id="loginModal" class="fixed inset-0 bg-slate-950/95 backdrop-blur-md z-[100] flex items-center justify-center p-4">
        <div class="glass-panel p-8 rounded-3xl max-w-md w-full text-center space-y-6 shadow-2xl glow-teal border border-teal-500/30">
            <div class="w-20 h-20 bg-gradient-to-tr from-teal-500 to-cyan-500 rounded-3xl mx-auto flex items-center justify-center text-white text-4xl shadow-lg shadow-teal-500/20">
                <i class="fa-solid fa-hospital-user"></i>
            </div>
            <div>
                <h2 class="text-2xl font-black text-white">صيدلية العيادات الفرعية</h2>
                <p class="text-xs text-teal-300 font-medium mt-1">مستشفى الحسين السلط الحكومي</p>
                <p class="text-xs text-slate-400 mt-2">يرجى إدخال الرمز السري المعتمد للدخول إلى المنظومة</p>
            </div>
            <div class="space-y-3">
                <input type="password" id="passInput" placeholder="أدخل الرمز السري..." 
                       class="w-full text-center px-4 py-3 bg-slate-900/90 border border-slate-700 rounded-xl focus:outline-none focus:border-teal-400 text-teal-300 font-bold text-lg tracking-widest">
                <p id="loginError" class="text-xs text-red-400 hidden font-semibold">الرمز السري غير صحيح!</p>
                <button onclick="checkPassword()" class="w-full py-3.5 bg-gradient-to-r from-teal-500 to-cyan-600 hover:from-teal-400 hover:to-cyan-500 text-white font-black rounded-xl shadow-lg shadow-teal-900/50 transition-all">
                    دخول المنظومة الإلكترونية
                </button>
            </div>
        </div>
    </div>

    <!-- الواجهة الرئيسية -->
    <div id="mainApp" class="flex-1 flex flex-col opacity-20 transition-opacity duration-500 pointer-events-none">
        
        <!-- الهيدر العلوي -->
        <header class="glass-panel sticky top-0 z-50 border-b border-slate-800/80">
            <div class="max-w-7xl mx-auto px-4 py-4 flex flex-col sm:flex-row justify-between items-center gap-4">
                <div class="flex items-center gap-3.5">
                    <div class="w-13 h-13 p-3 rounded-2xl bg-gradient-to-br from-teal-500 to-cyan-600 flex items-center justify-center text-white text-2xl shadow-lg shadow-teal-900/40">
                        <i class="fa-solid fa-staff-snake"></i>
                    </div>
                    <div>
                        <h1 class="text-xl font-black tracking-wide text-white">مستشفى الحسين السلط الحكومي</h1>
                        <p class="text-xs text-teal-400 font-bold">منظومة الاستعلام المباشر عن الأدوية &bull; صيدلية العيادات الفرعية</p>
                    </div>
                </div>

                <div class="flex items-center gap-3">
                    <button onclick="fetchSheetData()" class="px-4 py-2.5 bg-teal-500/10 hover:bg-teal-500/20 text-teal-300 border border-teal-500/30 rounded-xl text-xs font-bold flex items-center gap-2 transition-all">
                        <i class="fa-solid fa-arrows-rotate" id="syncIcon"></i>
                        <span>تحديث البيانات المباشرة</span>
                    </button>
                    <button onclick="logout()" class="px-3.5 py-2.5 bg-slate-800 hover:bg-slate-700 text-slate-400 rounded-xl text-xs font-semibold">
                        <i class="fa-solid fa-right-from-bracket"></i>
                    </button>
                </div>
            </div>
        </header>

        <!-- المحتوى الرئيسي -->
        <main class="flex-1 max-w-7xl w-full mx-auto px-4 py-6 space-y-6">
            
            <!-- بنر أوقات الرد المباشرة والتفاعلية -->
            <div class="glass-panel p-5 rounded-2xl border border-teal-500/20 relative overflow-hidden">
                <div class="flex flex-col md:flex-row justify-between items-center gap-4 relative z-10">
                    <div class="space-y-1 text-center md:text-right">
                        <div class="flex items-center justify-center md:justify-start gap-2">
                            <span class="w-2.5 h-2.5 rounded-full bg-teal-400 pulse-live"></span>
                            <h2 class="text-sm font-black text-white">أوقات الرد الرسمية على الاستفسارات</h2>
                        </div>
                        <p class="text-xs text-slate-400">يتم تحديث ردود الصيدلية والتحقق من التوفر خلال الفترتين المعينتين يومياً:</p>
                    </div>

                    <!-- الساعات -->
                    <div class="flex flex-wrap justify-center gap-3 text-xs font-bold">
                        <div class="bg-slate-900/80 px-4 py-2.5 rounded-xl border border-slate-700/60 flex items-center gap-2 text-teal-300">
                            <i class="fa-solid fa-sun text-amber-400"></i>
                            <span>الفترة الأولى: 8:30 ص – 9:30 ص</span>
                        </div>
                        <div class="bg-slate-900/80 px-4 py-2.5 rounded-xl border border-slate-700/60 flex items-center gap-2 text-cyan-300">
                            <i class="fa-solid fa-clock-three text-cyan-400"></i>
                            <span>الفترة الثانية: 2:30 ع – 3:30 ع</span>
                        </div>
                    </div>

                    <!-- شارة الحالة الحية -->
                    <div id="statusBadge" class="px-3.5 py-1.5 rounded-full text-xs font-black flex items-center gap-2">
                        <!-- يتم حسابه تلقائياً بالـ JavaScript -->
                    </div>
                </div>
            </div>

            <!-- شبكة العيادات والأقسام المخصصة (15 عيادة) -->
            <div class="space-y-3">
                <div class="flex items-center justify-between">
                    <h3 class="text-xs font-extrabold text-slate-400 uppercase tracking-wider flex items-center gap-2">
                        <i class="fa-solid fa-clinic-medical text-teal-400"></i>
                        <span>تصفية حسب العيادات والأقسام الفرعية:</span>
                    </h3>
                    <button onclick="filterByClinic('all')" class="text-xs text-teal-400 hover:underline font-bold">عرض الكل</button>
                </div>

                <div class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-5 lg:grid-cols-8 gap-2" id="clinicsGrid">
                    <!-- يتم إنشاؤها تلقائياً بالـ JS -->
                </div>
            </div>

            <!-- شريط البحث والإحصائيات -->
            <div class="grid grid-cols-1 md:grid-cols-4 gap-4">
                
                <!-- صندوق البحث -->
                <div class="md:col-span-2 glass-panel p-4 rounded-2xl flex flex-col justify-center space-y-1.5">
                    <label class="text-xs font-bold text-slate-400">البحث السريع عن دواء، علاج، أو عيادة:</label>
                    <div class="relative">
                        <i class="fa-solid fa-magnifying-glass absolute right-3.5 top-3.5 text-slate-500"></i>
                        <input type="text" id="searchInput" oninput="filterData()" placeholder="ابحث باسم الدواء، العيادة، أو حالة التوفر..." 
                               class="w-full pr-10 pl-4 py-2.5 bg-slate-900/90 rounded-xl border border-slate-700 text-sm font-bold text-teal-200 placeholder-slate-500 focus:outline-none focus:border-teal-400 transition-all">
                    </div>
                </div>

                <!-- إجمالي الاستفسارات -->
                <div class="glass-panel p-4 rounded-2xl flex items-center justify-between">
                    <div>
                        <p class="text-xs font-bold text-slate-400">إجمالي الأصناف</p>
                        <h3 id="totalCount" class="text-2xl font-black text-white mt-1">0</h3>
                    </div>
                    <div class="w-12 h-12 rounded-xl bg-slate-800/80 flex items-center justify-center text-teal-400 text-xl border border-slate-700/50">
                        <i class="fa-solid fa-pills"></i>
                    </div>
                </div>

                <!-- المتوفر بالصيدلية -->
                <div class="glass-panel p-4 rounded-2xl flex items-center justify-between">
                    <div>
                        <p class="text-xs font-bold text-slate-400">المتوفر في الصيدلية</p>
                        <h3 id="availCount" class="text-2xl font-black text-emerald-400 mt-1">0</h3>
                    </div>
                    <div class="w-12 h-12 rounded-xl bg-emerald-500/10 flex items-center justify-center text-emerald-400 text-xl border border-emerald-500/20">
                        <i class="fa-solid fa-circle-check"></i>
                    </div>
                </div>
            </div>

            <!-- جدول حالة التوفر وردود الصيدلية -->
            <div class="glass-panel rounded-2xl overflow-hidden border border-slate-800/80 shadow-2xl">
                <div class="overflow-x-auto">
                    <table class="w-full text-right border-collapse">
                        <thead>
                            <tr class="bg-slate-900/95 text-slate-400 text-xs font-bold border-b border-slate-800">
                                <th class="p-4">الدواء / العلاج المطلوب</th>
                                <th class="p-4">العيادة الفرعية</th>
                                <th class="p-4">حالة التوفر</th>
                                <th class="p-4">رد الصيدلية والتوجيهات</th>
                            </tr>
                        </thead>
                        <tbody id="dataTable" class="divide-y divide-slate-800/60 text-sm font-medium">
                            <tr>
                                <td colspan="4" class="p-8 text-center text-slate-500">
                                    جاري الاتصال المباشر بقاعدة بيانات Google Sheet...
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>
        </main>

        <!-- التذييل التوثيقي الرسمي (Footer) -->
        <footer class="bg-slate-950 text-slate-400 py-8 border-t border-slate-900/80 mt-12">
            <div class="max-w-7xl mx-auto px-4 text-center space-y-5">
                
                <div class="space-y-1">
                    <p class="text-sm font-black text-teal-400 tracking-wide">
                        مستشفى الحسين السلط الحكومي &bull; قسم الصيدلة
                    </p>
                    <p class="text-xs text-slate-500">صيدلية العيادات الفرعية - المنظومة الإلكترونية المباشرة</p>
                </div>

                <div class="flex flex-col sm:flex-row justify-center items-center gap-3 text-xs sm:text-sm">
                    <div class="flex items-center gap-2 bg-slate-900 px-4 py-2.5 rounded-xl border border-slate-800">
                        <i class="fa-solid fa-laptop-code text-teal-400"></i>
                        <span><strong class="text-white">إعداد وتصميم:</strong> د. عز الدين الخرابشه | د. سوسن الزيادات</span>
                    </div>

                    <div class="flex items-center gap-2 bg-slate-900 px-4 py-2.5 rounded-xl border border-slate-800">
                        <i class="fa-solid fa-user-shield text-cyan-400"></i>
                        <span><strong class="text-white">تحت إشراف:</strong> عطوفة الدكتور مهند الحديدي</span>
                    </div>
                </div>

                <p class="text-[11px] text-slate-600 pt-3 border-t border-slate-900/60">
                    جميع الحقوق محفوظة © مستشفى الحسين السلط الحكومي 2026
                </p>
            </div>
        </footer>
    </div>

    <!-- البرمجة والربط بـ Google Sheet -->
    <script>
        const SECRET_PASS = "1234"; // الرمز السري الثابت

        // رابط شيت Google Sheet الخاص بك
        const SHEET_ID = "12Xl7xmX7Ib9izW4csDis_3GRjaH3qYppbgqAYdHLyac";
        const GOOGLE_SHEET_URL = `https://docs.google.com/spreadsheets/d/${SHEET_ID}/export?format=csv`;

        // قائمة العيادات الـ 15 المحددة
        const clinicsList = [
            { id: 'all', name: 'الكل', icon: 'fa-border-all' },
            { id: 'الجلدية', name: 'الجلدية', icon: 'fa-hand-dots' },
            { id: 'النسائية', name: 'النسائية', icon: 'fa-person-pregnant' },
            { id: 'أنف وأذن وحنجرة', name: 'أنف وأذن وحنجرة', icon: 'fa-head-side-cough' },
            { id: 'الأطفال', name: 'الأطفال', icon: 'fa-baby' },
            { id: 'العظام', name: 'العظام', icon: 'fa-bone' },
            { id: 'هضمي أطفال', name: 'هضمي أطفال', icon: 'fa-stomach' },
            { id: 'غدد أطفال', name: 'غدد أطفال', icon: 'fa-child' },
            { id: 'النفسية', name: 'النفسية', icon: 'fa-brain' },
            { id: 'جراحة وجه وفكين', name: 'جراحة وجه وفكين', icon: 'fa-tooth' },
            { id: 'صدرية أطفال', name: 'صدرية أطفال', icon: 'fa-lungs' },
            { id: 'الأسنان', name: 'الأسنان', icon: 'fa-teeth' },
            { id: 'اللثة', name: 'اللثة', icon: 'fa-teeth-open' },
            { id: 'العيون (الاستفسار عن العدسات)', name: 'العيون (العدسات)', icon: 'fa-eye' },
            { id: 'جراحة أطفال', name: 'جراحة أطفال', icon: 'fa-user-nurse' },
            { id: 'عمليات نهارية', name: 'عمليات نهارية', icon: 'fa-bed-pulse' }
        ];

        let rawData = [];
        let selectedClinic = 'all';

        window.onload = function() {
            renderClinicsButtons();
            updateLiveStatus();
            setInterval(updateLiveStatus, 60000); // تحديث مؤشر الوقت كل دقيقة

            if (localStorage.getItem("isLoggedIn") === "true") {
                unlockApp();
            }
        };

        // إنشاء أزرار العيادات
        function renderClinicsButtons() {
            const container = document.getElementById("clinicsGrid");
            container.innerHTML = "";
            clinicsList.forEach(c => {
                const btn = document.createElement("button");
                btn.className = `clinic-pill px-3 py-2 rounded-xl text-xs font-bold flex items-center justify-center gap-1.5 ${c.id === 'all' ? 'active' : ''}`;
                btn.onclick = () => filterByClinic(c.id, btn);
                btn.innerHTML = `<i class="fa-solid ${c.icon} text-teal-400"></i><span>${c.name}</span>`;
                container.appendChild(btn);
            });
        }

        function filterByClinic(clinicId, btnElement) {
            selectedClinic = clinicId;
            document.querySelectorAll('.clinic-pill').forEach(b => b.classList.remove('active'));
            if (btnElement) btnElement.classList.add('active');
            else document.querySelector('.clinic-pill').classList.add('active');
            filterData();
        }

        // تحديد ما إذا كنا حالياً ضمن أوقات الرد الرسمية
        function updateLiveStatus() {
            const now = new Date();
            const hours = now.getHours();
            const minutes = now.getMinutes();
            const totalMin = hours * 60 + minutes;

            // 8:30 AM (510 min) to 9:30 AM (570 min)
            // 2:30 PM (870 min) to 3:30 PM (930 min)
            const isSlot1 = totalMin >= 510 && totalMin <= 570;
            const isSlot2 = totalMin >= 870 && totalMin <= 930;

            const badge = document.getElementById("statusBadge");
            if (isSlot1 || isSlot2) {
                badge.className = "px-3.5 py-1.5 rounded-full text-xs font-black flex items-center gap-2 badge-avail";
                badge.innerHTML = `<span class="w-2 h-2 rounded-full bg-emerald-400 pulse-live"></span> <span>استقبال الردود نشط الآن</span>`;
            } else {
                badge.className = "px-3.5 py-1.5 rounded-full text-xs font-black flex items-center gap-2 bg-slate-800 text-slate-400 border border-slate-700";
                badge.innerHTML = `<span class="w-2 h-2 rounded-full bg-slate-500"></span> <span>خارج أوقات الرد الرسمية</span>`;
            }
        }

        function checkPassword() {
            const val = document.getElementById("passInput").value;
            const err = document.getElementById("loginError");
            if (val === SECRET_PASS) {
                localStorage.setItem("isLoggedIn", "true");
                unlockApp();
            } else {
                err.classList.remove("hidden");
            }
        }

        function unlockApp() {
            document.getElementById("loginModal").classList.add("hidden");
            const app = document.getElementById("mainApp");
            app.classList.remove("opacity-20", "pointer-events-none");
            fetchSheetData();
        }

        function logout() {
            localStorage.removeItem("isLoggedIn");
            location.reload();
        }

        // جلب البيانات من Google Sheet
        async function fetchSheetData() {
            const icon = document.getElementById("syncIcon");
            if (icon) icon.classList.add("fa-spin");

            try {
                const response = await fetch(GOOGLE_SHEET_URL);
                if (!response.ok) throw new Error("تعذر جلب البيانات");

                const csvText = await response.text();
                rawData = parseCSV(csvText);
                filterData();

            } catch (error) {
                console.error("Fetch error:", error);
                // بيانات تجريبية احتياطية
                rawData = [
                    { drug: "Tablet Y", clinic: "النسائية", status: "AVAILABLE", reply: "متوفر بالصيدلية الفرعية" },
                    { drug: "Ointment X", clinic: "الجلدية", status: "PENDING", reply: "قيد التحضير والتدقيق" },
                    { drug: "شراب أدول للأطفال + بخاخ فنتولين", clinic: "الأطفال", status: "AVAILABLE", reply: "متوفر في صيدلية العيادات الفرعية" },
                    { drug: "فحص ومراجعة العدسات", clinic: "العيون (الاستفسار عن العدسات)", status: "AVAILABLE", reply: "يرجى مراجعة الصيدلية لاستلام العدسات" }
                ];
                filterData();
            } finally {
                if (icon) setTimeout(() => icon.classList.remove("fa-spin"), 600);
            }
        }

        function parseCSV(text) {
            const lines = text.trim().split(/\r?\n/);
            if (lines.length < 2) return [];

            const parseLine = line => {
                const values = [];
                let current = '';
                let inQuotes = false;
                for (let i = 0; i < line.length; i++) {
                    const char = line[i];
                    if (char === '"') inQuotes = !inQuotes;
                    else if (char === ',' && !inQuotes) {
                        values.push(current.trim().replace(/^"|"$/g, ''));
                        current = '';
                    } else current += char;
                }
                values.push(current.trim().replace(/^"|"$/g, ''));
                return values;
            };

            const headers = parseLine(lines[0]);
            const results = [];

            for (let i = 1; i < lines.length; i++) {
                if (!lines[i].trim()) continue;
                const row = parseLine(lines[i]);
                const obj = {};
                headers.forEach((h, idx) => {
                    obj[h.trim()] = row[idx] ? row[idx].trim() : '';
                });

                const drug = obj['الدواء المطلوب'] || obj['Drug'] || 'غير محدد';
                const clinic = obj['العيادة'] || obj['العيادة الفرعية'] || 'عامة';
                const status = obj['الحالة'] || obj['حالة التوفر'] || 'AVAILABLE';
                const reply = obj['رد الصيدلية'] || obj['رد الصيدلي'] || 'متوفر بالصيدلية';

                results.push({ drug, clinic, status, reply });
            }
            return results;
        }

        function renderTable(data) {
            const tbody = document.getElementById("dataTable");
            const total = document.getElementById("totalCount");
            const avail = document.getElementById("availCount");

            tbody.innerHTML = "";
            total.textContent = data.length;
            let availCounter = 0;

            if (data.length === 0) {
                tbody.innerHTML = `<tr><td colspan="4" class="p-8 text-center text-slate-500 font-bold">لا توجد نتائج مطابقة لخيارات البحث أو التصفية الحالية</td></tr>`;
                avail.textContent = 0;
                return;
            }

            data.forEach(item => {
                const statusUpper = item.status.toUpperCase();
                const isAvail = statusUpper.includes("AVAILABLE") || (item.status.includes("متوفر") && !item.status.includes("غير"));
                const isPending = statusUpper.includes("PENDING") || item.status.includes("قيد");

                if (isAvail) availCounter++;

                let badgeClass = "badge-out";
                let badgeText = item.status;

                if (isAvail) {
                    badgeClass = "badge-avail";
                    badgeText = "متوفر بالصيدلية";
                } else if (isPending) {
                    badgeClass = "badge-pending";
                    badgeText = "قيد المراجعة / التجهيز";
                }

                const tr = document.createElement("tr");
                tr.className = "hover:bg-slate-800/40 transition-colors border-b border-slate-800/40";
                tr.innerHTML = `
                    <td class="p-4 font-bold text-slate-100">${item.drug}</td>
                    <td class="p-4 text-teal-300 font-semibold">${item.clinic}</td>
                    <td class="p-4">
                        <span class="px-3.5 py-1 rounded-full text-xs font-black inline-block ${badgeClass}">
                            ${badgeText}
                        </span>
                    </td>
                    <td class="p-4 text-slate-400 text-xs font-medium">${item.reply || '-'}</td>
                `;
                tbody.appendChild(tr);
            });

            avail.textContent = availCounter;
        }

        function filterData() {
            const q = document.getElementById("searchInput").value.toLowerCase();
            const filtered = rawData.filter(d => {
                const matchesClinic = (selectedClinic === 'all') || d.clinic.includes(selectedClinic) || (selectedClinic === 'العيون (الاستفسار عن العدسات)' && d.clinic.includes('العيون'));
                const matchesSearch = d.drug.toLowerCase().includes(q) || 
                                      d.clinic.toLowerCase().includes(q) || 
                                      d.status.toLowerCase().includes(q) ||
                                      d.reply.toLowerCase().includes(q);
                return matchesClinic && matchesSearch;
            });
            renderTable(filtered);
        }
    </script>
</body>
</html>
