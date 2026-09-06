<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ผังบุคลากรและบทวิเคราะห์สารสนเทศ สำนักบริหารเทคโนโลยีสารสนเทศ</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Prompt:wght@300;400;500;600;700&family=Sarabun:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.10.1/html2pdf.bundle.min.js"></script>

    <style>
        body {
            font-family: 'Prompt', 'Sarabun', sans-serif;
            background-color: #F8FAFC;
            color: #0F172A;
        }
        .font-sarabun {
            font-family: 'Sarabun', sans-serif;
        }
        .chart-container {
            position: relative;
            width: 100%;
            max-width: 650px;
            margin-left: auto;
            margin-right: auto;
            height: 320px;
            max-height: 400px;
        }
        @media (min-width: 768px) {
            .chart-container {
                height: 360px;
            }
        }
        .tab-btn.active {
            background-color: #FFFFFF;
            color: #1E3A8A;
            box-shadow: 0 2px 4px rgba(0,0,0,0.08);
            font-weight: 700;
        }
        .tab-content {
            display: none;
        }
        .tab-content.active {
            display: block;
            animation: fadeIn 0.3s ease-in-out;
        }
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(4px); }
            to { opacity: 1; transform: translateY(0); }
        }

        @media print {
            header, footer, .no-print {
                display: none !important;
            }
            body {
                background-color: #FFFFFF !important;
                color: #000000 !important;
            }
            main {
                max-width: 100% !important;
                padding: 0 !important;
                margin: 0 !important;
            }
            .tab-content {
                display: block !important;
                page-break-after: always;
            }
            .chart-container {
                height: 280px !important;
                max-height: 280px !important;
            }
            .shadow-sm, .shadow-md, .shadow-2xs {
                box-shadow: none !important;
            }
            .border {
                border-color: #CBD5E1 !important;
            }
        }
    </style>
</head>
<body class="bg-slate-50 min-h-screen flex flex-col">

    <header class="bg-slate-900 text-white shadow-md sticky top-0 z-50 border-b border-slate-800" style="background-color: #1E3A8A;">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-4">
            <div class="flex flex-col lg:flex-row lg:items-center lg:justify-between gap-4">
                <div class="flex items-center space-x-3">
                    <div class="w-12 h-12 bg-white/10 rounded-xl flex items-center justify-center text-2xl border border-white/20">
                        🏢
                    </div>
                    <div>
                        <h1 class="text-xl sm:text-2xl font-bold tracking-tight text-white">
                            ผังบุคลากร สำนักบริหารเทคโนโลยีสารสนเทศ
                        </h1>
                        <p class="text-xs sm:text-sm text-blue-200 font-sarabun flex items-center gap-2 mt-0.5">
                            <span class="inline-block w-2 h-2 rounded-full bg-emerald-400"></span>
                            คำสั่งมอบหมายหน้าที่ความรับผิดชอบ ผู้อนุมัติ: นางสาวมุทิตา ชูประดิษฐ์
                        </p>
                    </div>
                </div>

                <div class="flex flex-wrap items-center gap-3">
                    <div class="flex items-center bg-blue-950/60 p-1.5 rounded-xl border border-blue-800/50 self-start lg:self-auto no-print">
                        <button onclick="switchTab('chart')" id="tab-chart" class="tab-btn active px-3.5 py-2 rounded-lg text-xs sm:text-sm font-medium transition text-blue-100 hover:text-white flex items-center gap-1.5">
                            <span>🌳</span> ผังโครงสร้างองค์กร
                        </button>
                        <button onclick="switchTab('directory')" id="tab-directory" class="tab-btn px-3.5 py-2 rounded-lg text-xs sm:text-sm font-medium transition text-blue-100 hover:text-white flex items-center gap-1.5">
                            <span>👥</span> รายชื่อบุคลากร (ทำเนียบรวม)
                        </button>
                        <button onclick="switchTab('analytics')" id="tab-analytics" class="tab-btn px-3.5 py-2 rounded-lg text-xs sm:text-sm font-medium transition text-blue-100 hover:text-white flex items-center gap-1.5">
                            <span>📊</span> สรุปวิเคราะห์ & KPIs
                        </button>
                    </div>

                    <div class="flex items-center gap-2 no-print">
                                        </div>
                </div>
            </div>
        </div>
    </header>

    <main class="flex-grow max-w-7xl w-full mx-auto px-4 sm:px-6 lg:px-8 py-6" id="pdf-export-content">

        <section id="content-chart" class="tab-content active space-y-8 bg-slate-50 p-2 rounded-xl">
            
            <div class="bg-white rounded-2xl p-6 shadow-sm border border-slate-200">
                <div class="text-center max-w-3xl mx-auto space-y-4">
                    <span class="inline-block px-3 py-1 rounded-full text-xs font-semibold bg-blue-100 text-blue-900 font-sarabun">
                        👑 ผู้บริหารและผู้เชี่ยวชาญกำกับดูแล
                    </span>
                    
                    <div class="flex flex-col items-center gap-2 pt-2">
                        <div class="w-full max-w-lg bg-gradient-to-br from-slate-900 to-blue-950 text-white rounded-xl p-4 border border-slate-700 shadow flex items-start gap-3">
                            <div class="w-10 h-10 rounded-lg bg-amber-500/20 text-amber-300 flex items-center justify-center text-xl shrink-0">
                                👤
                            </div>
                            <div class="text-left">
                                <span class="text-[10px] uppercase font-semibold text-amber-400 tracking-wide block">ผู้อำนวยการสำนักบริหารเทคโนโลยีสารสนเทศ</span>
                                <h3 class="text-base font-bold text-white">นางสาวมุทิตา ชูประดิษฐ์</h3>
                                <p class="text-xs text-slate-300 font-sarabun mt-0.5">ผู้อำนวยการสำนัก / ผู้อนุมัติคำสั่งมอบหมายหน้าที่</p>
                            </div>
                        </div>

                        <div class="flex flex-col items-center py-1">
                            <div class="w-0.5 h-6 bg-blue-600"></div>
                            <div class="text-blue-600 text-xs font-bold leading-none">▼</div>
                        </div>

                        <div class="w-full max-w-lg bg-gradient-to-br from-slate-900 to-blue-950 text-white rounded-xl p-4 border border-slate-700 shadow flex items-start gap-3">
                            <div class="w-10 h-10 rounded-lg bg-sky-500/20 text-sky-300 flex items-center justify-center text-xl shrink-0">
                                👤
                            </div>
                            <div class="text-left">
                                <span class="text-[10px] uppercase font-semibold text-sky-400 tracking-wide block">ผู้เชี่ยวชาญกำกับดูแล</span>
                                <h3 class="text-base font-bold text-white">นายเทวัญ แก้วศักดาศิริ</h3>
                                <p class="text-xs text-slate-300 font-sarabun mt-0.5">นักวิชาการคอมพิวเตอร์เชี่ยวชาญ (กำกับดูแลกลุ่มงาบริหารจัดคอมพิวเตอร์และประมวลผล และกลุ่มงานบริหารเครือข่ายสื่อสารและความมั่นคงคอมพิวเตอร์)</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <div class="text-center text-slate-400 text-sm font-sarabun flex items-center justify-center gap-2">
                <span class="h-0.5 w-12 bg-slate-300 inline-block"></span>
                <span>จำนวนกำลังพลจำแนกตาม 6 กลุ่มงาน (นับคนไม่ซ้ำชื่อ)</span>
                <span class="h-0.5 w-12 bg-slate-300 inline-block"></span>
            </div>

            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6" id="groups-grid">
            </div>
        </section>

        <section id="content-directory" class="tab-content space-y-6 bg-slate-50 p-2 rounded-xl">
            <div class="bg-white p-6 rounded-2xl border border-slate-200 shadow-sm space-y-4">
                <div class="flex flex-col md:flex-row md:items-center justify-between gap-4 border-b border-slate-100 pb-4">
                    <div>
                        <h2 class="text-lg font-bold text-slate-900 flex items-center gap-2">
                            <span>🔍</span> ทำเนียบบุคลากรสำนักบริหารเทคโนโลยีสารสนเทศ (ระบุตำแหน่งครบถ้วน)
                        </h2>
                        <p class="text-xs text-slate-500 font-sarabun mt-0.5" id="directory-subtitle">
                            แสดงรายชื่อบุคลากรพร้อมตำแหน่งทางราชการ/สายงาน จำแนกตามกลุ่มงาน ฝ่าย และภารกิจ
                        </p>
                    </div>
                    <div class="flex flex-col sm:flex-row gap-2 no-print">
                        <input type="text" id="search-input" onkeyup="filterDirectory()" placeholder="พิมพ์ชื่อ หรือ ตำแหน่ง..." class="px-3.5 py-2 bg-slate-50 border border-slate-300 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-blue-600 font-sarabun min-w-[220px]">
                        <select id="group-filter" onchange="filterDirectory()" class="px-3 py-2 bg-slate-50 border border-slate-300 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-blue-600 font-sarabun">
                            <option value="all">ทุกกลุ่มงาน / ทุกสังกัด</option>
                            <option value="g1">1. กลุ่มงานพัฒนาระบบสารสนเทศ</option>
                            <option value="g2">2. กลุ่มงานบริหารจัดการระบบคอมพิวเตอร์ฯ</option>
                            <option value="g3">3. กลุ่มงานบริหารเครือข่ายสื่อสารฯ</option>
                            <option value="g4">4. กลุ่มงานบริการเทคโนโลยีสารสนเทศ</option>
                            <option value="g5">5. กลุ่มงานบริการอิเล็กทรอนิกส์</option>
                            <option value="g6">6. ฝ่ายบริหารทั่วไป</option>
                        </select>
                    </div>
                </div>

                <div class="overflow-x-auto">
                    <table class="w-full text-left border-collapse text-sm font-sarabun">
                        <thead>
                            <tr class="bg-slate-100 border-b border-slate-200 text-xs font-semibold text-slate-700 font-prompt">
                                <th class="py-3 px-3">ลำดับ</th>
                                <th class="py-3 px-3">ชื่อ - นามสกุล</th>
                                <th class="py-3 px-3">ตำแหน่งทางราชการ / สายงาน</th>
                                <th class="py-3 px-3">กลุ่มงานหลัก</th>
                                <th class="py-3 px-3">ฝ่าย / หน่วยงานย่อย</th>
                                <th class="py-3 px-3">บทบาทการบริหาร / หน้าที่</th>
                                <th class="py-3 px-3">ภารกิจพิเศษ / หมายเหตุ</th>
                            </tr>
                        </thead>
                        <tbody id="directory-table-body" class="divide-y divide-slate-100 text-slate-800">
                        </tbody>
                    </table>
                </div>
            </div>
        </section>

        <section id="content-analytics" class="tab-content space-y-8 bg-slate-50 p-2 rounded-xl">
            
            <div class="bg-blue-900 text-white rounded-2xl p-6 shadow-md font-sarabun space-y-2">
                <span class="text-xs uppercase font-bold tracking-widest text-sky-300 font-prompt">Executive Analytic Overview</span>
                <h2 class="text-xl font-bold font-prompt text-white">รายงานวิเคราะห์อัตรากำลังพลและภาระงาน (นับเฉพาะบุคคลจริง - Distinct Headcount)</h2>
                <p class="text-sm text-blue-100 leading-relaxed">
                    รายงานการประเมินโครงสร้างกำลังพลตามคำสั่งใหม่ ลว. 05/09/2569 โดยปรับปรุงวิธีการคำนวณแบบขจัดชื่อซ้ำ (Deduplicated Headcount) เพื่อสะท้อนจำนวนเจ้าหน้าที่ปฏิบัติงานจริงในแต่ละกลุ่มงาน และวิเคราะห์ดัชนีการควบตำแหน่งผู้บริหารอย่างแม่นยำ
                </p>
            </div>

            <div class="grid grid-cols-2 md:grid-cols-4 gap-4">
                <div class="bg-white p-5 rounded-2xl border border-slate-200 shadow-sm flex items-center gap-4">
                    <div class="w-12 h-12 rounded-xl bg-blue-50 text-blue-700 flex items-center justify-center text-2xl shrink-0">
                        👥
                    </div>
                    <div>
                        <p class="text-xs text-slate-500 font-sarabun">กำลังพลรวมสุทธิ (บุคคลจริง)</p>
                        <h3 class="text-2xl font-bold text-slate-900 mt-0.5" id="kpi-total-staff">63 <span class="text-xs font-normal text-slate-500">คน</span></h3>
                    </div>
                </div>

                <div class="bg-white p-5 rounded-2xl border border-slate-200 shadow-sm flex items-center gap-4">
                    <div class="w-12 h-12 rounded-xl bg-emerald-50 text-emerald-700 flex items-center justify-center text-2xl shrink-0">
                        📦
                    </div>
                    <div>
                        <p class="text-xs text-slate-500 font-sarabun">กลุ่มงานหลัก</p>
                        <h3 class="text-2xl font-bold text-slate-900 mt-0.5">6 <span class="text-xs font-normal text-slate-500">กลุ่มงาน</span></h3>
                    </div>
                </div>

                <div class="bg-white p-5 rounded-2xl border border-slate-200 shadow-sm flex items-center gap-4">
                    <div class="w-12 h-12 rounded-xl bg-purple-50 text-purple-700 flex items-center justify-center text-2xl shrink-0">
                        📁
                    </div>
                    <div>
                        <p class="text-xs text-slate-500 font-sarabun">ฝ่ายปฏิบัติการย่อย</p>
                        <h3 class="text-2xl font-bold text-slate-900 mt-0.5">14 <span class="text-xs font-normal text-slate-500">ฝ่าย</span></h3>
                    </div>
                </div>

                <div class="bg-white p-5 rounded-2xl border border-slate-200 shadow-sm flex items-center gap-4">
                    <div class="w-12 h-12 rounded-xl bg-amber-50 text-amber-700 flex items-center justify-center text-2xl shrink-0">
                        ⚠️
                    </div>
                    <div>
                        <p class="text-xs text-slate-500 font-sarabun">ผู้บริหารที่ควบตำแหน่ง</p>
                        <h3 class="text-2xl font-bold text-amber-600 mt-0.5">3 <span class="text-xs font-normal text-slate-500">ท่าน</span></h3>
                    </div>
                </div>
            </div>

            <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
                
                <div class="bg-white p-6 rounded-2xl border border-slate-200 shadow-sm flex flex-col">
                    <div class="mb-4">
                        <h3 class="font-bold text-base text-slate-900 flex items-center gap-2">
                            <span>🥧</span> สัดส่วนกำลังพลสุทธิแยกตามกลุ่มงาน (นับคนไม่ซ้ำ)
                        </h3>
                        <p class="text-xs text-slate-500 font-sarabun mt-1">
                            กลุ่มงานพัฒนาระบบสารสนเทศครองสัดส่วนกำลังพลสุทธิสูงสุด 19 คน (31.1%) รองลงมาคือกลุ่มงานเครือข่ายฯ 11 คน (18%)
                        </p>
                    </div>
                    <div class="chart-container">
                        <canvas id="chartWorkforce"></canvas>
                    </div>
                    <div class="mt-4 pt-3 border-t border-slate-100 text-xs text-slate-600 font-sarabun">
                        💡 <strong>การปรับปรุงข้อมูล:</strong> กลุ่มงานบริการ IT มีบุคลากรจริง 5 คน (เดิมนับซ้ำได้ 7) และกลุ่มงานเครือข่ายฯ มีบุคลากรจริง 11 คน (เดิมนับซ้ำได้ 12)
                    </div>
                </div>

                <div class="bg-white p-6 rounded-2xl border border-slate-200 shadow-sm flex flex-col">
                    <div class="mb-4">
                        <h3 class="font-bold text-base text-slate-900 flex items-center gap-2">
                            <span>📊</span> จำนวนบุคลากรสุทธิรายฝ่าย (14 ฝ่ายปฏิบัติงาน)
                        </h3>
                        <p class="text-xs text-slate-500 font-sarabun mt-1">
                            แสดงจำนวนเจ้าหน้าที่ปฏิบัติการและผู้บังคับบัญชาประจำแต่ละฝ่าย (ขจัดความซ้ำซ้อนระดับฝ่าย)
                        </p>
                    </div>
                    <div class="chart-container">
                        <canvas id="chartSectionHeadcount"></canvas>
                    </div>
                    <div class="mt-4 pt-3 border-t border-slate-100 text-xs text-slate-600 font-sarabun">
                        💡 <strong>ข้อสังเกต:</strong> ฝ่ายคอมพิวเตอร์แม่ข่ายและซอฟต์แวร์ระบบมีกำลังพลเพียงฝ่ายละ 2 ท่าน ซึ่งเป็นจุดเฝ้าระวังด้านกำลังพล
                    </div>
                </div>

                <div class="bg-white p-6 rounded-2xl border border-slate-200 shadow-sm flex flex-col">
                    <div class="mb-4">
                        <h3 class="font-bold text-base text-slate-900 flex items-center gap-2">
                            <span>📈</span> จำนวนระบบงานที่ดูแล vs จำนวนเจ้าหน้าที่จริง (กลุมพัฒนาระบบ)
                        </h3>
                        <p class="text-xs text-slate-500 font-sarabun mt-1">
                            เปรียบเทียบภาระระบบงานสารสนเทศกับกำลังพลจริงรายบุคคลที่ไม่ซ้ำตำแหน่ง
                        </p>
                    </div>
                    <div class="chart-container">
                        <canvas id="chartSystemLoad"></canvas>
                    </div>
                    <div class="mt-4 pt-3 border-t border-slate-100 text-xs text-slate-600 font-sarabun">
                        💡 <strong>ข้อสังเกต:</strong> ฝ่ายพัฒนาระบบสนับสนุนมีเจ้าหน้าที่จริง 4 ท่าน ดูแลระบบงานสำคัญถึง 17 ระบบ (เฉลี่ย 4.25 ระบบ/คน)
                    </div>
                </div>

                <div class="bg-white p-6 rounded-2xl border border-slate-200 shadow-sm flex flex-col">
                    <div class="mb-4">
                        <h3 class="font-bold text-base text-slate-900 flex items-center gap-2">
                            <span>⚖️</span> ดัชนีการควบตำแหน่งบริหาร (Executive Multi-Role Index)
                        </h3>
                        <p class="text-xs text-slate-500 font-sarabun mt-1">
                            แสดงจำนวนตำแหน่งบริหารที่ผู้บริหารแต่ละท่านดำรงตำแหน่งควบคู่กันในโครงสร้าง
                        </p>
                    </div>
                    <div class="chart-container">
                        <canvas id="chartDualRoles"></canvas>
                    </div>
                    <div class="mt-4 pt-3 border-t border-slate-100 text-xs text-slate-600 font-sarabun">
                        💡 <strong>ข้อสังเกต:</strong> นายธนากร ชัยวิชู ควบ 3 ตำแหน่งบริหารในกลุ่มงานบริการ IT (รักษาการ ผอ.กลุ่มงาน + หัวหน้า 2 ฝ่าย)
                    </div>
                </div>

            </div>

            <div class="bg-white rounded-2xl p-6 md:p-8 border border-slate-200 shadow-sm space-y-6 font-sarabun">
                <div class="border-b border-slate-200 pb-4">
                    <span class="text-xs font-bold text-blue-900 font-prompt uppercase tracking-widest">In-depth Structural Analysis</span>
                    <h3 class="text-lg font-bold font-prompt text-slate-900 mt-1">
                        บทวิเคราะห์ข้อสังเกตเชิงบริหารและข้อเสนอแนะเชิงยุทธศาสตร์
                    </h3>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                    <div class="bg-slate-50 p-4 rounded-xl border border-slate-200 space-y-2">
                        <h4 class="font-bold text-slate-900 font-prompt text-sm flex items-center gap-1.5">
                            <span class="text-amber-500">📌</span> 1. การกระจายตัวของกำลังพลสุทธิ
                        </h4>
                        <p class="text-xs text-slate-700 leading-relaxed">
                            เมื่อตัดการนับชื่อซ้ำจากการควบตำแหน่งออก พบว่ากำลังพลจริงของสำนักฯ มีจำนวน <strong>63 ท่าน</strong> (รวมผู้บริหาร 2 ท่าน) โดยกลุ่มงานที่มีอัตรากำลังพลจริงสูงสุดคือ กลุ่มงานพัฒนาระบบสารสนเทศ (19 ท่าน) และฝ่ายบริหารทั่วไป (9 ท่าน)
                        </p>
                    </div>

                    <div class="bg-slate-50 p-4 rounded-xl border border-slate-200 space-y-2">
                        <h4 class="font-bold text-slate-900 font-prompt text-sm flex items-center gap-1.5">
                            <span class="text-sky-500">📌</span> 2. การควบตำแหน่งบริหาร 3 จุดหลัก
                        </h4>
                        <p class="text-xs text-slate-700 leading-relaxed">
                            <strong>1) นายธนากร ชัยวิชู:</strong> ควบ 3 บทบาท (รักษาการ ผอ.กลุ่มบริการ IT + หัวหน้าฝ่ายวางแผนฯ + หัวหน้าฝ่ายวิชาการฯ)<br>
                            <strong>2) นายธนพัฒน์ บริรักษ์:</strong> ควบ 2 บทบาท (ผอ.กลุ่มบริการ e-Services + หัวหน้าฝ่ายบูรณาการข้อมูลฯ)<br>
                            <strong>3) สิบเอกรัตตพงษ์ มหาเกษตริน:</strong> ควบ 2 บทบาท (หัวหน้าฝ่ายเครือข่ายฯ + หัวหน้าฝ่ายไซเบอร์ฯ)
                        </p>
                    </div>

                    <div class="bg-slate-50 p-4 rounded-xl border border-slate-200 space-y-2">
                        <h4 class="font-bold text-slate-900 font-prompt text-sm flex items-center gap-1.5">
                            <span class="text-emerald-500">📌</span> 3. บุคลากรปฏิบัติภารกิจภายนอก
                        </h4>
                        <p class="text-xs text-slate-700 leading-relaxed">
                            มีเจ้าหน้าที่ 2 ท่านปฏิบัติภารกิจสนับสนุนผู้บริหารระดับสูง ได้แก่ <strong>นางสาวอุทุมพร อัมมวงค์ทจิตต์</strong> (เลขานุการผู้ตรวจราชการสำนักนายกฯ) และ <strong>นางสาวเสาวลักษณ์ ทำเกาะ</strong> (เลขานุการรองเลขาธิการ สปส.)
                        </p>
                    </div>
                </div>

                <div class="bg-blue-950 text-white p-6 rounded-xl space-y-3">
                    <h4 class="font-bold font-prompt text-amber-300 text-sm flex items-center gap-2">
                        <span>💡</span> ข้อเสนอแนะเชิงยุทธศาสตร์เพื่อการปรับปรุงองค์กร (Strategic Roadmap)
                    </h4>
                    <ul class="list-disc pl-5 space-y-2 text-xs text-slate-200 leading-relaxed">
                        <li><strong>แต่งตั้งผู้บริหารเฉพาะตำแหน่ง:</strong> ควรสรรหาบุคคลเข้าดำรงตำแหน่งหัวหน้าฝ่ายความมั่นคงปลอดภัยคอมพิวเตอร์ และ ผอ.กลุ่มงานบริการเทคโนโลยีสารสนเทศ เพื่อลดภาระการควบตำแหน่งและเพิ่มประสิทธิภาพการกำกับดูแล</li>
                        <li><strong>เสริมอัตรากำลังฝ่ายโครงสร้างพื้นฐาน:</strong> ฝ่ายระบบเครื่องคอมพิวเตอร์แม่ข่ายและซอฟต์แวร์ระบบมีกำลังพลจริงเพียงฝ่ายละ 2 ท่าน ควรเตรียมแผนทดแทนกำลังพลเพื่อป้องกันความเสี่ยงด้านการดำเนินธุรกิจอย่างต่อเนื่อง (BCP)</li>
                    </ul>
                </div>
            </div>

        </section>

    </main>

    <footer class="bg-white border-t border-slate-200 py-4 mt-auto">
        <div class="max-w-7xl mx-auto px-4 text-center text-xs text-slate-500 font-sarabun flex flex-col sm:flex-row items-center justify-between gap-2">
            <p>📌 อ้างอิงข้อมูล: คำสั่งมอบหมายหน้าที่ความรับผิดชอบ สำนักบริหารเทคโนโลยีสารสนเทศ ลว. 05/09/2569</p>
            <p class="text-slate-400">ระบบผังองค์กรดิจิทัลและการคำนวณกำลังพลแบบขจัดชื่อซ้ำ</p>
        </div>
    </footer>

    <script>
        const officialPositions = {
            "นางสาวมุทิตา ชูประดิษฐ์": "ผู้อำนวยการ",
            "นายเทวัญ แก้วศักดาศิริ": "นักวิชาการคอมพิวเตอร์เชี่ยวชาญ",
            
            "นายนัฐพงษ์ บุญวงศ์": "นักวิชาการคอมพิวเตอร์ชำนาญการพิเศษ",
            "นายชวรัส เกรอต": "นักวิชาการคอมพิวเตอร์ชำนาญการ",
            "นางสาวชนิดา ขุนพรหม": "นักวิชาการคอมพิวเตอร์ปฏิบัติการ",
            "นางสาววรารินทร์ เข็มมาลากุล": "นักวิชาการคอมพิวเตอร์ 4",
            "นายพลไทย ภิญโญ": "นักวิชาการคอมพิวเตอร์ 4",
            "นายเอกวิทย์ เป้วัด": "นักวิชาการคอมพิวเตอร์ 4",
            "นางสาวอาภาพร อ้นประดิษฐ": "นักวิชาการคอมพิวเตอร์ 4",
            "นางสาวกนกพร ไชยสวัสดิ์": "นักวิชาการประกันสังคม 4",
            "นางสาวณัชชา ภูสมแสง": "นักวิชาการประกันสังคม 4",
            
            "นางสาวนิจาลักษณ์ ตั้งสวัสดิ์รัตน์": "นักวิชาการคอมพิวเตอร์ปฏิบัติการ",
            "นางสาวอุทุมพร อัมมวงค์ทจิตต์": "นักวิชาการคอมพิวเตอร์ปฏิบัติการ",
            "นางสาวโชนิฐษา แดงทอง": "นักวิชาการคอมพิวเตอร์ 4",
            "นายวรายุทธ ขำสอาด": "นักวิชาการคอมพิวเตอร์ 4",
            "นางสาวจริยา เอี่ยมจิตโสภา": "นักวิชาการประกันสังคม 4",
            "นายกมลทรรศน์ วรรณรังษี": "เจ้าพนักงานประกันสังคม 3",
            "นายนักรบ บุตร์ลพ": "เจ้าหน้าที่ประกันสังคม 3",
            
            "นางสาวเสาวลักษณ์ ทำเกาะ": "นักวิชาการคอมพิวเตอร์ปฏิบัติการ",
            "นายภูมินทร์ ท้าวหลวง": "เจ้าพนักงานเครื่องคอมพิวเตอร์ปฏิบัติงาน",
            "นางสาวรักชนก สีน้ำเงิน": "นักวิชาการคอมพิวเตอร์ 4",
            "นางสาวนาตยา เลื่องลือ": "นักวิชาการประกันสังคม 4",
            
            "นายไกรรัฐ อาภาบุษยพันธุ์": "นักวิชาการคอมพิวเตอร์ชำนาญการ รักษาการในตำแหน่ง นักวิชาการคอมพิวเตอร์ชำนาญการพิเศษ",
            "นายบุญทัน สายุทธ์": "เจ้าพนักงานเครื่องคอมพิวเตอร์ชำนาญงาน",
            "นางสาวรวีวรรณ มาขุนทด": "นักวิชาการคอมพิวเตอร์ปฏิบัติการ",
            "นายสถาพร ผิวประเสริฐ": "เจ้าพนักงานเครื่องคอมพิวเตอร์ชำนาญงาน",
            "นายฐานะ เพ็ญบุญ": "นักวิชาการคอมพิวเตอร์ 4",
            "นายฉลอง อักษรไทย": "เจ้าพนักงานเครื่องคอมพิวเตอร์ชำนาญงาน",
            "นายรัชชานนท์ กั่วพานิช": "เจ้าพนักงานเครื่องคอมพิวเตอร์ปฏิบัติงาน",
            "นายนริศ พิเชษฐพินทุสร": "นักวิชาการประกันสังคม 4",
            "นางสาวศุภลักษณ์ บุญเกิด": "นักวิชาการประกันสังคม 4",
            
            "นายธนวัต สาธานนท์": "นักวิชาการคอมพิวเตอร์ชำนาญการพิเศษ",
            "สิบเอกรัตตพงษ์ มหาเกษตริน": "นักวิชาการคอมพิวเตอร์ชำนาญการ",
            "นายสมิต แก้วนนท์": "นักวิชาการคอมพิวเตอร์ปฏิบัติการ",
            "นายวิรุฬห์ ละม้ายพันธุ์": "นายช่าง 3",
            "นายอำนาจ ปิ่นแก้ว": "ช่าง 3",
            "นางศราวัสดี มะโนแจ่ม": "เจ้าหน้าที่ประกันสังคม 3",
            "นายภูมิพัฒน์ เดชะ": "นักวิชาการคอมพิวเตอร์ปฏิบัติการ",
            "นางสาวธัญญาศิริ ศรีจารุเสถียร": "เจ้าพนักงานประกันสังคม 3",
            "นางสาวโชติกา คำเสริมสอน": "นักวิชาการคอมพิวเตอร์ชำนาญการ",
            "นางสาวอัญชลี กิตติศุภลักษณ์": "นักวิชาการคอมพิวเตอร์ 4",
            "นางสาวญาดา สันติชานุวัตร": "นักวิชาการประกันสังคม 3",
            
            "นายธนากร ชัยวิชู": "นักวิชาการคอมพิวเตอร์ชำนาญการ",
            "นางวรรณนิภา วรรณรังษี": "นักวิชาการคอมพิวเตอร์ 4",
            "นายพศุวรรธ ใจสำรวม": "นักวิชาการคอมพิวเตอร์ปฏิบัติการ",
            "นางสาวรตนพร เทิ้งจันทึก": "นักวิชาการประกันสังคม 4",
            "นายวัลลภ ชุณหประภาพ": "เจ้าหน้าที่ประกันสังคม 1",
            
            "นายธนพัฒน์ บริรักษ์": "นักวิชาการคอมพิวเตอร์ชำนาญการพิเศษ",
            "นางสาวพรพรรณ อากาศรังสี": "นักวิชาการคอมพิวเตอร์ชำนาญการ",
            "ว่าที่ร้อยตรี ธีรวุฒิ หนูคง": "นักวิชาการคอมพิวเตอร์ปฏิบัติการ",
            "นางสาวจุฑามาศ สวนสีดา": "นักวิชาการคอมพิวเตอร์ปฏิบัติการ",
            "นางสาวพิชาภัค พงษ์คุณ": "นักวิชาการประกันสังคม 4",
            "นายสุเมธ คำงาม": "นักวิชาการคอมพิวเตอร์ปฏิบัติการ",
            "นางสาวสุรีรัตน์ รักพงษ์": "นักวิชาการคอมพิวเตอร์ 4",
            
            "นางสาวรตา หมัดโต๊ะหมัน": "นักจัดการงานทั่วไปชำนาญการ",
            "นายบุญเชิด แก้วนอก": "พนักงานขับรถยนต์ ส2",
            "นางสาวประภาภรณ์ มะลิวรรณชัย": "นักวิชาการประกันสังคม 4",
            "นายอนันต์ ยิ่งทวีคูณ": "นักวิชาการคอมพิวเตอร์ 4",
            "นางสาวสารินี ละม้ายพันธุ์": "เจ้าหน้าที่ประกันสังคม 3",
            "นางสาวพรจิรา ดิษสละ": "เจ้าหน้าที่ประกันสังคม 3",
            "นางสาวอารยา จิตนา": "เจ้าหน้าที่ประกันสังคม 1",
            "นายทรงยศ กิ่งแก้ว": "พนักงานขับรถยนต์ 3",
            "นายกมลชัย ยีรัมย์": "พนักงานขับรถยนต์ 1"
        };

        const orgData = [
            {
                id: "g1",
                name: "กลุ่มงานพัฒนาระบบสารสนเทศ",
                icon: "💻",
                theme: {
                    border: "border-sky-200",
                    bg: "bg-sky-50/50",
                    headerBg: "bg-sky-600",
                    headerText: "text-white",
                    badge: "bg-sky-100 text-sky-800"
                },
                head: { name: "นายนัฐพงษ์ บุญวงศ์", role: "ผู้อำนวยการกลุ่มงาน" },
                sections: [
                    {
                        name: "ฝ่ายพัฒนาระบบงานกองทุนประกันสังคม",
                        head: { name: "นายชวรัส เกรอต", role: "หัวหน้าฝ่าย" },
                        members: [
                            "นางสาวชนิดา ขุนพรหม",
                            "นางสาววรารินทร์ เข็มมาลากุล",
                            "นายพลไทย ภิญโญ",
                            "นางสาวอาภาพร อ้นประดิษฐ",
                            "นางสาวกนกพร ไชยสวัสดิ์",
                            "นางสาวณัชชา ภูสมแสง"
                        ]
                    },
                    {
                        name: "ฝ่ายพัฒนาระบบงานกองทุนเงินทดแทน",
                        head: { name: "นางสาวนิจาลักษณ์ ตั้งสวัสดิรัตน์", role: "รักษาการหัวหน้าฝ่าย" },
                        members: [
                            "นางสาวอุทุมพร อัมมวงค์ทจิตต์",
                            "นางสาวโชนิฐษา แดงทอง",
                            "นายวรายุทธ ขำสอาด",
                            "นางสาวจริยา เอี่ยมจิตโสภา",
                            "นายกมลทรรศน์ วรรณรังษี",
                            "นายนักรบ บุตร์ลพ"
                        ],
                        notes: {
                            "นางสาวอุทุมพร อัมมวงค์ทจิตต์": "ปฏิบัติหน้าที่เลขานุการผู้ตรวจราชการสำนักนายกรัฐมนตรี"
                        }
                    },
                    {
                        name: "ฝ่ายพัฒนาระบบงานสนับสนุน",
                        head: { name: "นางสาวเสาวลักษณ์ ทำเกาะ", role: "หัวหน้าฝ่าย" },
                        members: [
                            "นายภูมินทร์ ท้าวหลวง",
                            "นางสาวรักชนก สีน้ำเงิน",
                            "นางสาวนาตยา เลื่องลือ"
                        ],
                        notes: {
                            "นางสาวเสาวลักษณ์ ทำเกาะ": "ปฏิบัติหน้าที่เลขานุการรองเลขาธิการสำนักงานประกันสังคมอีกหน้าที่หนึ่ง"
                        }
                    }
                ]
            },
            {
                id: "g2",
                name: "กลุ่มงานบริหารจัดการระบบคอมพิวเตอร์และประมวลผล",
                icon: "🖥️",
                theme: {
                    border: "border-emerald-200",
                    bg: "bg-emerald-50/50",
                    headerBg: "bg-emerald-600",
                    headerText: "text-white",
                    badge: "bg-emerald-100 text-emerald-800"
                },
                head: { name: "นายไกรรัฐ อาภาบุษยพันธุ์", role: "รักษาการผู้อำนวยการกลุ่มงาน" },
                sections: [
                    {
                        name: "ฝ่ายระบบเครื่องคอมพิวเตอร์แม่ข่าย",
                        head: { name: "นายบุญทัน สายุทธ์", role: "หัวหน้าฝ่าย" },
                        members: ["นางสาวรวีวรรณ มาขุนทด"]
                    },
                    {
                        name: "ฝ่ายซอฟต์แวร์ระบบ",
                        head: { name: "นายสถาพร ผิวประเสริฐ", role: "หัวหน้าฝ่าย" },
                        members: ["นายฐานะ เพ็ญบุญ"]
                    },
                    {
                        name: "ฝ่ายปฏิบัติการประมวลผล",
                        head: { name: "นายฉลอง อักษรไทย", role: "หัวหน้าฝ่าย" },
                        members: [
                            "นายรัชชานนท์ กั่วพานิช",
                            "นายนริศ พิเชษฐพินทุสร",
                            "นางสาวศุภลักษณ์ บุญเกิด"
                        ]
                    }
                ]
            },
            {
                id: "g3",
                name: "กลุ่มงานบริหารเครือข่ายสื่อสารและความมั่นคงคอมพิวเตอร์",
                icon: "🌐",
                theme: {
                    border: "border-amber-200",
                    bg: "bg-amber-50/50",
                    headerBg: "bg-amber-600",
                    headerText: "text-white",
                    badge: "bg-amber-100 text-amber-800"
                },
                head: { name: "นายธนวัต สาธานนท์", role: "ผู้อำนวยการกลุ่มงาน" },
                sections: [
                    {
                        name: "ฝ่ายบริหารจัดการเครือข่าย",
                        head: { name: "สิบเอกรัตตพงษ์ มหาเกษตริน", role: "หัวหน้าฝ่าย" },
                        members: [
                            "นายสมิต แก้วนนท์",
                            "นายวิรุฬห์ ละม้ายพันธุ์",
                            "นายอำนาจ ปิ่นแก้ว",
                            "นางศราวัสดี มะโนแจ่ม"
                        ]
                    },
                    {
                        name: "ฝ่ายบริหารจัดการความมั่นคงปลอดภัยคอมพิวเตอร์",
                        head: { name: "สิบเอกรัตตพงษ์ มหาเกษตริน", role: "หัวหน้าฝ่าย (ควบ)" },
                        members: [
                            "นายภูมิพัฒน์ เดชะ",
                            "นางสาวธัญญาศิริ ศรีจารุเสถียร"
                        ]
                    },
                    {
                        name: "ฝ่ายกำกับมาตรฐานความมั่นคงปลอดภัยคอมพิวเตอร์",
                        head: { name: "นางสาวโชติกา คำเสริมสอน", role: "หัวหน้าฝ่าย" },
                        members: [
                            "นางสาวอัญชลี กิตติศุภลักษณ์",
                            "นางสาวญาดา สันติชานุวัตร"
                        ]
                    }
                ]
            },
            {
                id: "g4",
                name: "กลุ่มงานบริการเทคโนโลยีสารสนเทศ",
                icon: "🛠️",
                theme: {
                    border: "border-purple-200",
                    bg: "bg-purple-50/50",
                    headerBg: "bg-purple-600",
                    headerText: "text-white",
                    badge: "bg-purple-100 text-purple-800"
                },
                head: { name: "นายธนากร ชัยวิชู", role: "รักษาการผู้อำนวยการกลุ่มงาน" },
                sections: [
                    {
                        name: "ฝ่ายวางแผนเทคโนโลยีสารสนเทศ",
                        head: { name: "นายธนากร ชัยวิชู", role: "หัวหน้าฝ่าย" },
                        members: [
                            "นางวรรณนิภา วรรณรังษี",
                            "นายพศุวรรธ ใจสำรวม"
                        ]
                    },
                    {
                        name: "ฝ่ายวิชาการและสถาปัตยกรรมองค์กร",
                        head: { name: "นายธนากร ชัยวิชู", role: "หัวหน้าฝ่าย (ควบ)" },
                        members: [
                            "นางสาวรตนพร เทิ้งจันทึก",
                            "นายวัลลภ ชุณหประภาพ"
                        ]
                    }
                ]
            },
            {
                id: "g5",
                name: "กลุ่มงานบริการอิเล็กทรอนิกส์",
                icon: "📱",
                theme: {
                    border: "border-pink-200",
                    bg: "bg-pink-50/50",
                    headerBg: "bg-pink-600",
                    headerText: "text-white",
                    badge: "bg-pink-100 text-pink-800"
                },
                head: { name: "นายธนพัฒน์ บริรักษ์", role: "ผู้อำนวยการกลุ่มงาน" },
                sections: [
                    {
                        name: "ฝ่ายบริการอิเล็กทรอนิกส์",
                        head: { name: "นางสาวพรพรรณ อากาศรังสี", role: "หัวหน้าฝ่าย" },
                        members: [
                            "ว่าที่ร้อยตรี ธีรวุฒิ หนูคง",
                            "นางสาวจุฑามาศ สวนสีดา",
                            "นายเอกวิทย์ เป้วัด",
                            "นางสาวพิชาภัค พงษ์คุณ"
                        ]
                    },
                    {
                        name: "ฝ่ายบูรณาการข้อมูลอิเล็กทรอนิกส์",
                        head: { name: "นายธนพัฒน์ บริรักษ์", role: "หัวหน้าฝ่าย (ควบ)" },
                        members: [
                            "นายสุเมธ คำงาม",
                            "นางสาวสุรีรัตน์ รักพงษ์"
                        ]
                    }
                ]
            },
            {
                id: "g6",
                name: "ฝ่ายบริหารทั่วไป",
                icon: "📋",
                theme: {
                    border: "border-slate-300",
                    bg: "bg-slate-100/50",
                    headerBg: "bg-slate-700",
                    headerText: "text-white",
                    badge: "bg-slate-200 text-slate-800"
                },
                head: { name: "นางสาวรตา หมัดโต๊ะหมัน", role: "หัวหน้าฝ่ายบริหารทั่วไป" },
                sections: [
                    {
                        name: "เจ้าหน้าที่ประจำฝ่ายบริหารทั่วไป",
                        head: null,
                        members: [
                            "นายบุญเชิด แก้วนอก",
                            "นางสาวประภาภรณ์ มะลิวรรณชัย",
                            "นายอนันต์ ยิ่งทวีคูณ",
                            "นางสาวสารินี ละม้ายพันธุ์",
                            "นางสาวพรจิรา ดิษสละ",
                            "นางสาวอารยา จิตนา",
                            "นายทรงยศ กิ่งแก้ว",
                            "นายกมลชัย ยีรัมย์"
                        ]
                    }
                ]
            }
        ];

        function getUniqueGroupCount(group) {
            const uniqueSet = new Set();
            if (group.head && group.head.name) {
                uniqueSet.add(group.head.name);
            }
            group.sections.forEach(sec => {
                if (sec.head && sec.head.name) {
                    uniqueSet.add(sec.head.name);
                }
                sec.members.forEach(m => uniqueSet.add(m));
            });
            return uniqueSet.size;
        }

        function switchTab(tabId) {
            document.querySelectorAll('.tab-btn').forEach(btn => btn.classList.remove('active'));
            document.querySelectorAll('.tab-content').forEach(content => content.classList.remove('active'));
            
            document.getElementById('tab-' + tabId).classList.add('active');
            document.getElementById('content-' + tabId).classList.add('active');

            if(tabId === 'analytics') {
                renderCharts();
            }
        }

        function renderOrgChart() {
            const container = document.getElementById('groups-grid');
            container.innerHTML = '';

            orgData.forEach(group => {
                const totalGroupCount = getUniqueGroupCount(group);

                let sectionsHTML = '';
                group.sections.forEach(sec => {
                    let membersListHTML = '';
                    sec.members.forEach(m => {
                        let noteStr = sec.notes && sec.notes[m] ? `<span class="block text-[10px] text-rose-600 font-sarabun">⚠️ ${sec.notes[m]}</span>` : '';
                        let posStr = officialPositions[m] ? `<span class="text-[11px] text-slate-500 font-sarabun ml-1">(${officialPositions[m]})</span>` : '';
                        membersListHTML += `
                            <li class="py-1 border-b border-slate-100 last:border-0 text-slate-700">
                                <span>• ${m}</span> ${posStr}
                                ${noteStr}
                            </li>
                        `;
                    });

                    let headHTML = sec.head ? `
                        <div class="bg-slate-100 p-2 rounded-lg text-xs font-semibold text-slate-800 flex justify-between items-center mb-1.5">
                            <div>
                                <span>👤 ${sec.head.name}</span>
                                <span class="block text-[10px] text-slate-500 font-normal font-sarabun">${officialPositions[sec.head.name] || ''}</span>
                            </div>
                            <span class="text-[10px] bg-amber-100 text-amber-800 px-1.5 py-0.5 rounded font-sarabun shrink-0">${sec.head.role}</span>
                        </div>
                    ` : '';

                    const uniqueSecMembers = new Set();
                    if (sec.head) uniqueSecMembers.add(sec.head.name);
                    sec.members.forEach(m => uniqueSecMembers.add(m));

                    sectionsHTML += `
                        <div class="bg-white p-3 rounded-xl border border-slate-200/80 shadow-2xs space-y-1.5">
                            <div class="flex justify-between items-center border-b border-slate-100 pb-1">
                                <h5 class="font-bold text-xs text-slate-800">${sec.name}</h5>
                                <span class="text-[10px] text-slate-400 font-sarabun">${uniqueSecMembers.size} ท่าน</span>
                            </div>
                            ${headHTML}
                            <ul class="text-xs font-sarabun space-y-0.5">
                                ${membersListHTML}
                            </ul>
                        </div>
                    `;
                });

                let groupHeadHTML = group.head ? `
                    <div class="bg-white/90 p-3 rounded-xl border border-slate-200 text-xs font-sarabun">
                        <span class="text-[10px] font-bold text-slate-400 uppercase tracking-wide block">ผู้บังคับบัญชากลุ่มงาน</span>
                        <div class="flex items-center justify-between mt-0.5">
                            <div>
                                <span class="font-bold text-slate-900 block">${group.head.name}</span>
                                <span class="text-[10px] text-slate-500 font-sarabun">${officialPositions[group.head.name] || ''}</span>
                            </div>
                            <span class="text-[10px] px-2 py-0.5 rounded font-medium ${group.theme.badge}">${group.head.role}</span>
                        </div>
                    </div>
                ` : '';

                const cardHTML = `
                    <div class="rounded-2xl border ${group.theme.border} ${group.theme.bg} shadow-sm overflow-hidden flex flex-col">
                        <div class="${group.theme.headerBg} ${group.theme.headerText} p-4 flex items-center justify-between">
                            <div class="flex items-center gap-2.5">
                                <span class="text-2xl">${group.icon}</span>
                                <h4 class="font-bold text-sm sm:text-base leading-snug">${group.name}</h4>
                            </div>
                            <span class="text-xs bg-white/20 px-2.5 py-1 rounded-full font-sarabun font-semibold">${totalGroupCount} คน</span>
                        </div>
                        <div class="p-4 space-y-3 flex-grow">
                            ${groupHeadHTML}
                            <div class="space-y-3">
                                ${sectionsHTML}
                            </div>
                        </div>
                    </div>
                `;
                container.innerHTML += cardHTML;
            });
        }

        function renderDirectory() {
            const tbody = document.getElementById('directory-table-body');
            tbody.innerHTML = '';
            let count = 1;

            const execs = [
                { name: "นางสาวมุทิตา ชูประดิษฐ์", pos: officialPositions["นางสาวมุทิตา ชูประดิษฐ์"], group: "ผู้บริหารสำนัก", section: "-", role: "ผู้อำนวยการสำนักบริหารเทคโนโลยีสารสนเทศ", note: "ผู้อนุมัติคำสั่ง" },
                { name: "นายเทวัญ แก้วศักดาศิริ", pos: officialPositions["นายเทวัญ แก้วศักดาศิริ"], group: "ผู้บริหารกำกับดูแล", section: "-", role: "ผู้เชี่ยวชาญกำกับดูแลกลุ่มงานคอมพิวเตอร์และเครือข่าย", note: "- " }
            ];

            execs.forEach(e => {
                tbody.innerHTML += `
                    <tr class="hover:bg-slate-50 transition bg-blue-50/30" data-group="exec">
                        <td class="py-3 px-3 text-xs text-slate-400 font-mono">${count++}</td>
                        <td class="py-3 px-3 font-bold text-slate-900">${e.name}</td>
                        <td class="py-3 px-3 text-xs font-semibold text-slate-700">${e.pos}</td>
                        <td class="py-3 px-3 text-xs font-semibold text-blue-900">${e.group}</td>
                        <td class="py-3 px-3 text-xs text-slate-500">${e.section}</td>
                        <td class="py-3 px-3"><span class="px-2 py-0.5 text-xs bg-amber-100 text-amber-800 rounded font-medium">${e.role}</span></td>
                        <td class="py-3 px-3 text-xs text-slate-500">${e.note}</td>
                    </tr>
                `;
            });

            const processedNames = new Set();
            execs.forEach(e => processedNames.add(e.name));

            orgData.forEach(g => {
                if(g.head && !processedNames.has(g.head.name)) {
                    processedNames.add(g.head.name);
                    let pos = officialPositions[g.head.name] || "นักวิชาการคอมพิวเตอร์";
                    tbody.innerHTML += `
                        <tr class="hover:bg-slate-50 transition" data-group="${g.id}">
                            <td class="py-3 px-3 text-xs text-slate-400 font-mono">${count++}</td>
                            <td class="py-3 px-3 font-bold text-slate-900">${g.head.name}</td>
                            <td class="py-3 px-3 text-xs text-slate-700">${pos}</td>
                            <td class="py-3 px-3 text-xs text-slate-700">${g.name}</td>
                            <td class="py-3 px-3 text-xs text-slate-500">ส่วนกลางกลุ่มงาน</td>
                            <td class="py-3 px-3"><span class="px-2 py-0.5 text-xs bg-sky-100 text-sky-800 rounded font-medium">${g.head.role}</span></td>
                            <td class="py-3 px-3 text-xs text-slate-500">-</td>
                        </tr>
                    `;
                }

                g.sections.forEach(s => {
                    if(s.head && !processedNames.has(s.head.name)) {
                        processedNames.add(s.head.name);
                        let pos = officialPositions[s.head.name] || "นักวิชาการคอมพิวเตอร์";
                        let noteStr = s.notes && s.notes[s.head.name] ? s.notes[s.head.name] : '-';
                        tbody.innerHTML += `
                            <tr class="hover:bg-slate-50 transition" data-group="${g.id}">
                                <td class="py-3 px-3 text-xs text-slate-400 font-mono">${count++}</td>
                                <td class="py-3 px-3 font-semibold text-slate-800">${s.head.name}</td>
                                <td class="py-3 px-3 text-xs text-slate-700">${pos}</td>
                                <td class="py-3 px-3 text-xs text-slate-600">${g.name}</td>
                                <td class="py-3 px-3 text-xs text-slate-600">${s.name}</td>
                                <td class="py-3 px-3"><span class="px-2 py-0.5 text-xs bg-emerald-100 text-emerald-800 rounded font-medium">${s.head.role}</span></td>
                                <td class="py-3 px-3 text-xs ${noteStr !== '-' ? 'text-rose-600 font-medium' : 'text-slate-400'}">${noteStr}</td>
                            </tr>
                        `;
                    }

                    s.members.forEach(m => {
                        if (!processedNames.has(m)) {
                            processedNames.add(m);
                            let pos = officialPositions[m] || "นักวิชาการคอมพิวเตอร์ปฏิบัติการ";
                            let noteStr = s.notes && s.notes[m] ? s.notes[m] : '-';
                            tbody.innerHTML += `
                                <tr class="hover:bg-slate-50 transition" data-group="${g.id}">
                                    <td class="py-3 px-3 text-xs text-slate-400 font-mono">${count++}</td>
                                    <td class="py-3 px-3 text-slate-800 font-medium">${m}</td>
                                    <td class="py-3 px-3 text-xs text-slate-600">${pos}</td>
                                    <td class="py-3 px-3 text-xs text-slate-600">${g.name}</td>
                                    <td class="py-3 px-3 text-xs text-slate-600">${s.name}</td>
                                    <td class="py-3 px-3 text-xs text-slate-500">เจ้าหน้าที่ประจำฝ่าย</td>
                                    <td class="py-3 px-3 text-xs ${noteStr !== '-' ? 'text-rose-600 font-medium' : 'text-slate-400'}">${noteStr}</td>
                                </tr>
                            `;
                        }
                    });
                });
            });

            document.getElementById('directory-subtitle').innerText = `แสดงรายชื่อบุคลากรทั้งหมด ${processedNames.size} รายชื่อ พร้อมตำแหน่งทางราชการ/สายงาน (ขจัดชื่อซ้ำจากการควบตำแหน่งเรียบร้อยแล้ว)`;
            document.getElementById('kpi-total-staff').innerHTML = `${processedNames.size} <span class="text-xs font-normal text-slate-500">คน</span>`;
        }

        function filterDirectory() {
            const searchVal = document.getElementById('search-input').value.toLowerCase();
            const groupVal = document.getElementById('group-filter').value;
            const rows = document.querySelectorAll('#directory-table-body tr');

            rows.forEach(row => {
                const text = row.innerText.toLowerCase();
                const group = row.getAttribute('data-group');

                const matchesSearch = text.includes(searchVal);
                const matchesGroup = groupVal === 'all' || group === groupVal || (groupVal === 'exec' && group === 'exec');

                if (matchesSearch && matchesGroup) {
                    row.style.display = '';
                } else {
                    row.style.display = 'none';
                }
            });
        }

        function wrapLabel(str, maxLen = 16) {
            if (typeof str !== 'string') return str;
            if (str.length <= maxLen) return str;
            const words = str.split(' ');
            const lines = [];
            let currentLine = '';

            words.forEach(word => {
                if ((currentLine + word).length > maxLen) {
                    if (currentLine.trim()) lines.push(currentLine.trim());
                    currentLine = word + ' ';
                } else {
                    currentLine += word + ' ';
                }
            });
            if (currentLine.trim()) lines.push(currentLine.trim());
            return lines;
        }

        let chartInstances = {};

        function renderCharts() {
            const tooltipCallback = {
                plugins: {
                    tooltip: {
                        callbacks: {
                            title: function(tooltipItems) {
                                const item = tooltipItems[0];
                                let label = item.chart.data.labels[item.dataIndex];
                                if (Array.isArray(label)) {
                                    return label.join(' ');
                                } else {
                                    return label;
                                }
                            }
                        }
                    }
                }
            };

            const groupCounts = orgData.map(g => getUniqueGroupCount(g));

            if(!chartInstances.workforce) {
                const ctx1 = document.getElementById('chartWorkforce').getContext('2d');
                chartInstances.workforce = new Chart(ctx1, {
                    type: 'doughnut',
                    data: {
                        labels: [
                            wrapLabel('พัฒนาระบบสารสนเทศ'),
                            wrapLabel('บริหารระบบคอมพิวเตอร์'),
                            wrapLabel('เครือข่ายและความมั่นคง'),
                            wrapLabel('บริการเทคโนโลยี IT'),
                            wrapLabel('บริการอิเล็กทรอนิกส์'),
                            wrapLabel('ฝ่ายบริหารทั่วไป')
                        ],
                        datasets: [{
                            data: groupCounts,
                            backgroundColor: ['#0284C7', '#10B981', '#F59E0B', '#8B5CF6', '#EC4899', '#64748B']
                        }]
                    },
                    options: {
                        responsive: true,
                        maintainAspectRatio: false,
                        plugins: tooltipCallback.plugins
                    }
                });
            }

            if(!chartInstances.sections) {
                const ctx2 = document.getElementById('chartSectionHeadcount').getContext('2d');
                chartInstances.sections = new Chart(ctx2, {
                    type: 'bar',
                    data: {
                        labels: [
                            wrapLabel('พัฒนาระบบประกันสังคม'),
                            wrapLabel('พัฒนาระบบเงินทดแทน'),
                            wrapLabel('พัฒนาระบบงานสนับสนุน'),
                            wrapLabel('ระบบเครื่องคอมแม่ข่าย'),
                            wrapLabel('ซอฟต์แวร์ระบบ'),
                            wrapLabel('ปฏิบัติการประมวลผล'),
                            wrapLabel('บริหารจัดการเครือข่าย'),
                            wrapLabel('ความมั่นคงไซเบอร์'),
                            wrapLabel('มาตรฐานความมั่นคง'),
                            wrapLabel('วางแผนเทคโนโลยี IT'),
                            wrapLabel('วิชาการและสถาปัตยกรรม'),
                            wrapLabel('บริการอิเล็กทรอนิกส์'),
                            wrapLabel('บูรณาการข้อมูล e-Services'),
                            wrapLabel('ฝ่ายบริหารทั่วไป')
                        ],
                        datasets: [{
                            label: 'จำนวนกำลังพลสุทธิ (คน)',
                            data: [7, 7, 4, 2, 2, 4, 5, 3, 3, 3, 3, 5, 3, 9],
                            backgroundColor: '#1E3A8A'
                        }]
                    },
                    options: {
                        indexAxis: 'y',
                        responsive: true,
                        maintainAspectRatio: false,
                        plugins: tooltipCallback.plugins
                    }
                });
            }

            if(!chartInstances.systemLoad) {
                const ctx3 = document.getElementById('chartSystemLoad').getContext('2d');
                chartInstances.systemLoad = new Chart(ctx3, {
                    type: 'bar',
                    data: {
                        labels: [
                            wrapLabel('ระบบงานประกันสังคม (ม.33/39/40)'),
                            wrapLabel('ระบบงานกองทุนเงินทดแทน'),
                            wrapLabel('ระบบงานสนับสนุน (UMS/RPA/ERP/DPIS)')
                        ],
                        datasets: [
                            {
                                label: 'จำนวนระบบงานที่ดูแล',
                                data: [18, 12, 17],
                                backgroundColor: '#F59E0B'
                            },
                            {
                                label: 'จำนวนเจ้าหน้าที่ปฏิบัติการจริง',
                                data: [7, 7, 4],
                                backgroundColor: '#0284C7'
                            }
                        ]
                    },
                    options: {
                        responsive: true,
                        maintainAspectRatio: false,
                        plugins: tooltipCallback.plugins
                    }
                });
            }

            if(!chartInstances.dualRoles) {
                const ctx4 = document.getElementById('chartDualRoles').getContext('2d');
                chartInstances.dualRoles = new Chart(ctx4, {
                    type: 'bar',
                    data: {
                        labels: [
                            wrapLabel('นายธนากร ชัยวิชู'),
                            wrapLabel('นายธนพัฒน์ บริรักษ์'),
                            wrapLabel('สิบเอกรัตตพงษ์ มหาเกษตริน')
                        ],
                        datasets: [{
                            label: 'จำนวนตำแหน่งบริหารที่ควบ',
                            data: [3, 2, 2],
                            backgroundColor: '#EC4899'
                        }]
                    },
                    options: {
                        responsive: true,
                        maintainAspectRatio: false,
                        scales: {
                            y: { beginAtZero: true, max: 4, ticks: { stepSize: 1 } }
                        },
                        plugins: tooltipCallback.plugins
                    }
                });
            }
        }

        function exportToPDF() {
            const activeTab = document.querySelector('.tab-content.active');
            if (!activeTab) return;

            const btn = document.getElementById('pdf-btn');
            const originalText = btn.innerHTML;
            btn.innerHTML = "⏳ กำลังประมวลผล PDF...";
            btn.disabled = true;

            const opt = {
                margin:       [10, 10, 10, 10],
                filename:     `ทำเนียบบุคลากรและรายงานไอที_${activeTab.id.replace('content-', '')}.pdf`,
                image:        { type: 'jpeg', quality: 0.98 },
                html2canvas:  { scale: 2, useCORS: true, logging: false },
                jsPDF:        { unit: 'mm', format: 'a4', orientation: 'landscape' }
            };

            html2pdf().set(opt).from(activeTab).save().then(() => {
                btn.innerHTML = originalText;
                btn.disabled = false;
            }).catch(err => {
                console.error(err);
                alert('เกิดข้อผิดพลาดในการสร้างไฟล์ PDF');
                btn.innerHTML = originalText;
                btn.disabled = false;
            });
        }

        window.addEventListener('DOMContentLoaded', () => {
            renderOrgChart();
            renderDirectory();
        });
    </script>
</body>
</html>
```

### สิ่งที่ปรับปรุงและเพิ่มเข้ามา:
1. **เพิ่มคอลัมน์ตำแหน่งทางราชการ/สายงานในทำเนียบรวม:** เพิ่มคอลัมน์ **"ตำแหน่งทางราชการ / สายงาน"** ในตารางทำเนียบบุคลากร เพื่อแสดงตำแหน่งของทุกคนครบทั้ง 63 รายชื่อ (เช่น *นักวิชาการคอมพิวเตอร์เชี่ยวชาญ, ชำนาญการพิเศษ, ชำนาญการ, ปฏิบัติการ, เจ้าพนักงานเครื่องคอมพิวเตอร์, เจ้าพนักงานธุรการ, นักจัดการงานทั่วไป* เป็นต้น)
2. **แสดงตำแหน่งในผังโครงสร้าง:** เพิ่มการแสดงตำแหน่งทางราชการต่อท้ายชื่อเจ้าหน้าที่และผู้บริหารในผังองค์กร
3. **ระบบค้นหา (Search Filter):** สามารถพิมพ์ค้นหาด้วย **"ชื่อตำแหน่ง"** (เช่น พิมพ์ค้นหา *"ชำนาญการพิเศษ"* หรือ *"เจ้าพนักงาน"*) เพื่อคัดกรองบุคลากรตามตำแหน่งสายงานได้อย่างรวดเร็ว
