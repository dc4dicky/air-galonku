# air-galonku
Web app untuk memantau kebutuhan air galon tiap departemen

<!DOCTYPE html>
<html lang="id" class="h-full">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <meta name="theme-color" content="#0ea5e9">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <title>AIR GALONKU - Sistem Manajemen Air Galon</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['Inter', 'sans-serif'],
                    },
                    colors: {
                        water: {
                            50: '#f0f9ff',
                            100: '#e0f2fe',
                            200: '#bae6fd',
                            300: '#7dd3fc',
                            400: '#38bdf8',
                            500: '#0ea5e9',
                            600: '#0284c7',
                            700: '#0369a1',
                            800: '#075985',
                            900: '#0c4a6e',
                        }
                    }
                }
            }
        }
    </script>
    <style>
        body { 
            font-family: 'Inter', sans-serif; 
            -webkit-tap-highlight-color: transparent;
            touch-action: manipulation; }
        /* Custom scrollbar */
        ::-webkit-scrollbar { width: 6px; height: 6px; }
        ::-webkit-scrollbar-track { background: #f1f1f1; }
        ::-webkit-scrollbar-thumb { background: #0ea5e9; border-radius: 9999px; }
        ::-webkit-scrollbar-thumb:hover { background: #0284c7; }
        /* Smooth transitions & scale active taps */
        .btn-active-state {
            transition: transform 0.1s ease, background-color 0.2s ease;
        }
        .btn-active-state:active {
            transform: scale(0.96);
        }
        /* Prevent text selection on quick double taps */
        .no-select {
            -webkit-user-select: none;
            user-select: none;
        }
        /* Toast animation */
        @keyframes slideInRight {
            from { transform: translateY(100px); opacity: 0; }
            to { transform: translateY(0); opacity: 1; }
        }
        @media (min-width: 640px) {
            @keyframes slideInRight {
                from { transform: translateX(100%); opacity: 0; }
                to { transform: translateX(0); opacity: 1; }
            }
        }
        .toast-enter { animation: slideInRight 0.3s cubic-bezier(0.16, 1, 0.3, 1) forwards; }
    </style>
</head>
<body class="bg-water-50 text-slate-800 min-h-screen flex flex-col pb-20 sm:pb-0">
    <!-- Top Header Navigation (Visible on Desktop / Clean Logo Banner on Mobile) -->
    <header class="bg-white shadow-md border-b-4 border-water-500 sticky top-0 z-50">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex justify-between h-16 items-center">
                <div class="flex items-center mx-auto sm:mx-0">
                    <div class="flex-shrink-0 flex items-center text-water-600">
                        <i class="fa-solid fa-bottle-water text-3xl mr-2 animate-bounce-slow"></i>
                        <span class="font-bold text-2xl tracking-tight">AIR GALON<span class="text-water-400">KU</span></span>
                    </div>
                </div>
                <!-- Desktop Only Menu -->
                <div class="hidden sm:flex items-center space-x-1 sm:space-x-4">
                    <button onclick="switchTab('dashboard')" id="nav-dashboard" class="nav-btn px-4 py-2 rounded-xl text-sm font-semibold bg-water-100 text-water-800 transition-colors btn-active-state">
                        <i class="fa-solid fa-chart-pie mr-1.5"></i> Dashboard
                    </button>
                    <button onclick="switchTab('department')" id="nav-department" class="nav-btn px-4 py-2 rounded-xl text-sm font-semibold text-slate-600 hover:bg-water-50 hover:text-water-700 transition-colors btn-active-state">
                        <i class="fa-solid fa-building-user mr-1.5"></i> Departemen
                    </button>
                    <button onclick="switchTab('supplier')" id="nav-supplier" class="nav-btn px-4 py-2 rounded-xl text-sm font-semibold text-slate-600 hover:bg-water-50 hover:text-water-700 transition-colors btn-active-state">
                        <i class="fa-solid fa-truck-droplet mr-1.5"></i> Supplier
                    </button>
                    <button onclick="switchTab('report')" id="nav-report" class="nav-btn px-4 py-2 rounded-xl text-sm font-semibold text-slate-600 hover:bg-water-50 hover:text-water-700 transition-colors btn-active-state">
                        <i class="fa-solid fa-file-excel mr-1.5"></i> Laporan
                    </button>
                </div>
            </div>
        </div>
    </header>
    <!-- Mobile Sticky Bottom Navigation (Hidden on Desktop) -->
    <nav class="sm:hidden fixed bottom-0 inset-x-0 bg-white border-t border-slate-200 shadow-[0_-4px_12px_rgba(0,0,0,0.05)] z-50 px-4 pb-[safe-area-inset-bottom] no-select">
        <div class="flex justify-around items-center h-16">
            <button onclick="switchTab('dashboard')" id="mob-nav-dashboard" class="mob-nav-btn flex flex-col items-center justify-center w-16 h-12 text-water-600 transition-colors">
                <i class="fa-solid fa-chart-pie text-xl"></i>
                <span class="text-[10px] mt-1 font-semibold">Home</span>
            </button>
            <button onclick="switchTab('department')" id="mob-nav-department" class="mob-nav-btn flex flex-col items-center justify-center w-16 h-12 text-slate-400 transition-colors">
                <i class="fa-solid fa-building-user text-xl"></i>
                <span class="text-[10px] mt-1 font-semibold">Lapor</span>
            </button>
            <button onclick="switchTab('supplier')" id="mob-nav-supplier" class="mob-nav-btn flex flex-col items-center justify-center w-16 h-12 text-slate-400 transition-colors relative">
                <i class="fa-solid fa-truck-droplet text-xl"></i>
                <span class="text-[10px] mt-1 font-semibold">Supplier</span>
                <span id="badge-count-mobile" class="absolute top-1 right-2 bg-red-500 text-white text-[9px] font-bold px-1.5 py-0.5 rounded-full hidden">0</span>
            </button>
            <button onclick="switchTab('report')" id="mob-nav-report" class="mob-nav-btn flex flex-col items-center justify-center w-16 h-12 text-slate-400 transition-colors">
                <i class="fa-solid fa-file-invoice text-xl"></i>
                <span class="text-[10px] mt-1 font-semibold">Laporan</span>
            </button>
        </div>
    </nav>
    <!-- Main Content Area -->
    <main class="flex-grow max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-6 w-full mb-6"> 
        <!-- VIEW: DASHBOARD -->
        <div id="view-dashboard" class="view-section block animate-fade-in">
            <div class="mb-6 flex flex-col sm:flex-row sm:justify-between sm:items-end gap-2">
                <div>
                    <h2 class="text-xl sm:text-2xl font-bold text-slate-800">Status Plant Keseluruhan</h2>
                    <p class="text-slate-500 text-xs sm:text-sm mt-0.5">Pantau ketersediaan air galon di setiap area.</p>
                </div>
                <div class="flex space-x-4 text-xs font-semibold bg-white p-2.5 rounded-lg shadow-sm border border-slate-100 self-start sm:self-auto">
                    <div class="flex items-center"><span class="w-3 h-3 rounded-full bg-green-500 mr-1.5"></span> Aman</div>
                    <div class="flex items-center"><span class="w-3 h-3 rounded-full bg-red-500 mr-1.5"></span> Butuh Air</div>
                </div>
            </div>
            <!-- CARD CUACA & SIMULASI -->
            <div id="weather-card" class="bg-gradient-to-r from-sky-400 to-blue-500 rounded-2xl p-4 text-white shadow-md mb-6 flex flex-col sm:flex-row items-start sm:items-center justify-between gap-4 transition-all duration-500">
                <div class="flex items-center space-x-4">
                    <div id="weather-icon" class="text-4xl w-12 h-12 flex items-center justify-center bg-white/10 rounded-xl">
                        <i class="fa-solid fa-sun text-yellow-300 animate-spin-slow"></i>
                    </div>
                    <div>
                        <div class="flex items-center space-x-2">
                            <span class="text-[10px] font-bold uppercase tracking-wider bg-white/20 px-2 py-0.5 rounded-full">Weather Station Plant</span>
                            <span id="weather-badge-panas" class="hidden text-[10px] font-extrabold uppercase bg-red-600 text-white px-2 py-0.5 rounded-full animate-pulse"><i class="fa-solid fa-fire mr-1"></i>Ekstrem Panas</span>
                        </div>
                        <p id="weather-status-text" class="text-base sm:text-lg font-bold mt-1">Cerah - Siang Hari</p>
                        <p id="weather-temp" class="text-xs opacity-90">Suhu: 31°C</p>
                    </div>
                </div>
                <div class="flex items-center space-x-2 self-end sm:self-auto">
                    <label for="weather-select" class="text-xs font-semibold text-white/90">Ubah Cuaca:</label>
                    <select id="weather-select" onchange="manualWeatherChange(this.value)" class="bg-white/20 border border-white/30 text-white rounded-xl px-3 py-1.5 text-xs outline-none cursor-pointer hover:bg-white/30 transition-colors">
                        <option value="auto" class="text-slate-800">Masa Waktu Nyata</option>
                        <option value="cerah" class="text-slate-800">Siang - Cerah</option>
                        <option value="berawan" class="text-slate-800">Siang - Berawan</option>
                        <option value="hujan" class="text-slate-800">Siang/Malam - Hujan</option>
                        <option value="panas" class="text-slate-800">Siang - Terik Sangat Panas 🔥</option>
                        <option value="malam" class="text-slate-800">Malam - Bulan 🌙</option>
                    </select>
                </div>
            </div>
            <!-- PERINGATAN CUACA SANGAT PANAS BANNER -->
            <div id="weather-alert-banner" class="hidden mb-6 bg-red-100 border-l-4 border-red-600 p-4 rounded-2xl shadow-sm animate-bounce-subtle flex items-start space-x-3">
                <div class="text-red-600 text-xl pt-0.5"><i class="fa-solid fa-circle-exclamation animate-pulse"></i></div>
                <div>
                    <h4 class="font-extrabold text-red-900 text-sm">⚠️ PERINGATAN: CUACA EKSTREM SANGAT PANAS TERIK!</h4>
                    <p class="text-xs text-red-700 mt-1">Suhu pabrik di luar batas normal. Tingkat dehidrasi pekerja sangat tinggi dan konsumsi air minum meningkat pesat. **Harap pastikan ketersediaan galon di semua area produksi agar selalu terisi penuh!**</p>
                </div>
            </div>
            <!-- Summary Cards -->
            <div class="grid grid-cols-3 gap-3 sm:gap-6 mb-6">
                <div class="bg-white rounded-xl shadow-sm p-3 sm:p-6 border-l-4 border-water-500">
                    <div class="flex items-center justify-between">
                        <div>
                            <p class="text-[10px] sm:text-sm font-medium text-slate-500">Departemen</p>
                            <p class="text-lg sm:text-3xl font-bold text-slate-800 mt-0.5" id="dash-total-dept">0</p>
                        </div>
                        <div class="p-2 sm:p-3 bg-water-50 rounded-lg text-water-600 hidden sm:block">
                            <i class="fa-solid fa-building text-xl"></i>
                        </div>
                    </div>
                </div>
                <div class="bg-white rounded-xl shadow-sm p-3 sm:p-6 border-l-4 border-red-500">
                    <div class="flex items-center justify-between">
                        <div>
                            <p class="text-[10px] sm:text-sm font-medium text-slate-500">Butuh Suplai</p>
                            <p class="text-lg sm:text-3xl font-bold text-red-600 mt-0.5" id="dash-total-empty">0</p>
                        </div>
                        <div class="p-2 sm:p-3 bg-red-50 rounded-lg text-red-600 hidden sm:block">
                            <i class="fa-solid fa-triangle-exclamation text-xl"></i>
                        </div>
                    </div>
                </div>
                <div class="bg-white rounded-xl shadow-sm p-3 sm:p-6 border-l-4 border-green-500">
                    <div class="flex items-center justify-between">
                        <div>
                            <p class="text-[10px] sm:text-sm font-medium text-slate-500">Disuplai Hari Ini</p>
                            <p class="text-lg sm:text-3xl font-bold text-green-600 mt-0.5" id="dash-total-supplied">0</p>
                        </div>
                        <div class="p-2 sm:p-3 bg-green-50 rounded-lg text-green-600 hidden sm:block">
                            <i class="fa-solid fa-check-double text-xl"></i>
                        </div>
                    </div>
                </div>
            </div>
            <!-- Grid Departemen -->
            <div class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 lg:grid-cols-5 gap-3 sm:gap-4" id="dashboard-grid">
                <!-- Diisi oleh JavaScript -->
            </div>
        </div>
        <!-- VIEW: DEPARTMENT -->
        <div id="view-department" class="view-section hidden animate-fade-in">
            <div class="max-w-md mx-auto">
                <div class="bg-white rounded-2xl shadow-md overflow-hidden border border-slate-100">
                    <div class="bg-water-600 text-white p-5 sm:p-6">
                        <h2 class="text-xl font-bold"><i class="fa-solid fa-bullhorn mr-2"></i>Lapor Galon Kosong</h2>
                        <p class="text-water-100 mt-0.5 text-xs sm:text-sm">Formulir bagi staf departemen untuk meminta pengisian ulang.</p>
                    </div>
                    <div class="p-5 sm:p-6">
                        <form id="form-request" onsubmit="handleRequestSubmit(event)">
                            <div class="mb-4">
                                <label for="dept-select" class="block text-xs sm:text-sm font-bold text-slate-700 mb-1.5">Pilih Departemen Anda</label>
                                <select id="dept-select" required class="w-full px-4 py-3 rounded-xl border border-slate-300 focus:ring-2 focus:ring-water-500 focus:border-water-500 outline-none bg-slate-50 text-sm">
                                    <option value="" disabled selected>-- Pilih Departemen --</option>
                                </select>
                            </div>
                            <div class="mb-5">
                                <label for="qty-empty" class="block text-xs sm:text-sm font-bold text-slate-700 mb-1.5">Jumlah Galon Kosong</label>
                                <div class="relative">
                                    <input type="number" id="qty-empty" min="1" required class="w-full px-4 py-3 rounded-xl border border-slate-300 focus:ring-2 focus:ring-water-500 focus:border-water-500 outline-none bg-slate-50 pl-12 text-sm" placeholder="Masukkan jumlah...">
                                    <div class="absolute inset-y-0 left-0 pl-4 flex items-center pointer-events-none text-slate-400">
                                        <i class="fa-solid fa-bottle-droplet"></i>
                                    </div>
                                </div>
                            </div>
                            <button type="submit" class="w-full bg-water-600 hover:bg-water-700 active:scale-95 text-white font-bold py-3.5 px-4 rounded-xl shadow-md transition-all flex justify-center items-center text-sm btn-active-state">
                                <i class="fa-solid fa-paper-plane mr-2"></i> Kirim Permintaan Suplai
                            </button>
                        </form>
                    </div>
                </div>
            </div>
        </div>
        <!-- VIEW: SUPPLIER -->
        <div id="view-supplier" class="view-section hidden animate-fade-in">
            <div class="mb-5">
                <h2 class="text-xl sm:text-2xl font-bold text-slate-800">Daftar Permintaan Air</h2>
                <p class="text-slate-500 text-xs sm:text-sm mt-0.5">Daftar request aktif yang memerlukan pengiriman segera.</p>
            </div>
            <!-- Responsive Mobile-Optimized List for Supplier -->
            <div class="sm:hidden space-y-3" id="supplier-mobile-list">
                <!-- Populated by JS for small screens -->
            </div>
            <!-- Desktop-Optimized Table -->
            <div class="hidden sm:block bg-white rounded-2xl shadow-sm overflow-hidden border border-slate-200">
                <div class="overflow-x-auto">
                    <table class="min-w-full divide-y divide-slate-200">
                        <thead class="bg-slate-50">
                            <tr>
                                <th class="px-6 py-4 text-left text-xs font-semibold text-slate-500 uppercase tracking-wider">Waktu Request</th>
                                <th class="px-6 py-4 text-left text-xs font-semibold text-slate-500 uppercase tracking-wider">Departemen</th>
                                <th class="px-6 py-4 text-left text-xs font-semibold text-slate-500 uppercase tracking-wider">Kebutuhan (Galon)</th>
                                <th class="px-6 py-4 text-left text-xs font-semibold text-slate-500 uppercase tracking-wider">Status</th>
                                <th class="px-6 py-4 text-center text-xs font-semibold text-slate-500 uppercase tracking-wider">Aksi (Suplai)</th>
                            </tr>
                        </thead>
                        <tbody id="supplier-table-body" class="bg-white divide-y divide-slate-200">
                            <!-- Diisi oleh JS -->
                        </tbody>
                    </table>
                </div>
            </div>
            <!-- Empty State -->
            <div id="supplier-empty-state" class="hidden flex flex-col items-center justify-center py-16 text-slate-400 text-center">
                <i class="fa-solid fa-check-circle text-5xl mb-3 text-green-300"></i>
                <p class="text-base font-semibold text-slate-700">Semua Departemen Aman!</p>
                <p class="text-xs text-slate-400 mt-1">Belum ada request pengisian air galon baru.</p>
            </div>
        </div>
        <!-- VIEW: REPORT -->
        <div id="view-report" class="view-section hidden animate-fade-in">
            <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center mb-5 gap-4">
                <div>
                    <h2 class="text-xl sm:text-2xl font-bold text-slate-800">Riwayat Pengisian Galon</h2>
                    <p class="text-slate-500 text-xs sm:text-sm mt-0.5">Rekapitulasi aktivitas permintaan dan pengiriman air.</p>
                </div>
                <button onclick="exportToExcel()" class="w-full sm:w-auto bg-green-600 hover:bg-green-700 text-white font-bold py-2.5 px-4 rounded-xl shadow transition-colors flex items-center justify-center text-sm btn-active-state">
                    <i class="fa-solid fa-file-excel mr-2 text-lg"></i> Tarik Laporan (Excel/CSV)
                </button>
            </div>
            <!-- Responsive List for Reports on Mobile -->
            <div class="sm:hidden space-y-3" id="report-mobile-list">
                <!-- Populated by JS for small screens -->
            </div>
            <!-- Table for Desktop Reports -->
            <div class="hidden sm:block bg-white rounded-2xl shadow-sm overflow-hidden border border-slate-200">
                <div class="overflow-x-auto">
                    <table class="min-w-full divide-y divide-slate-200">
                        <thead class="bg-slate-50">
                            <tr>
                                <th class="px-6 py-3.5 text-left text-xs font-semibold text-slate-500 uppercase">ID Tiket</th>
                                <th class="px-6 py-3.5 text-left text-xs font-semibold text-slate-500 uppercase">Departemen</th>
                                <th class="px-6 py-3.5 text-left text-xs font-semibold text-slate-500 uppercase">Waktu Request</th>
                                <th class="px-6 py-3.5 text-left text-xs font-semibold text-slate-500 uppercase">Jml Request</th>
                                <th class="px-6 py-3.5 text-left text-xs font-semibold text-slate-500 uppercase">Waktu Suplai</th>
                                <th class="px-6 py-3.5 text-left text-xs font-semibold text-slate-500 uppercase">Jml Disuplai</th>
                                <th class="px-6 py-3.5 text-left text-xs font-semibold text-slate-500 uppercase">Foto Bukti</th>
                                <th class="px-6 py-3.5 text-left text-xs font-semibold text-slate-500 uppercase">Status</th>
                            </tr>
                        </thead>
                        <tbody id="report-table-body" class="bg-white divide-y divide-slate-200 text-sm">
                            <!-- Diisi oleh JS -->
                        </tbody>
                    </table>
                </div>
            </div>
        </div>
    </main>
    <!-- Modal Form Suplai -->
    <div id="modal-suplai" class="fixed inset-0 bg-slate-900 bg-opacity-60 z-[60] hidden flex items-end sm:items-center justify-center backdrop-blur-sm">
        <div class="bg-white rounded-t-2xl sm:rounded-2xl shadow-2xl max-w-md w-full overflow-hidden transform transition-all pb-[safe-area-inset-bottom] sm:pb-0">
            <div class="bg-water-600 p-4 flex justify-between items-center text-white">
                <h3 class="font-bold text-base sm:text-lg">Proses Pengiriman Air</h3>
                <button onclick="closeModal()" class="text-water-100 hover:text-white p-1 rounded-full hover:bg-water-700/50 transition-colors"><i class="fa-solid fa-times text-xl"></i></button>
            </div>
            <form id="form-fulfill" onsubmit="handleFulfillSubmit(event)" class="p-5 sm:p-6">
                <input type="hidden" id="fulfill-id">
                <div class="grid grid-cols-2 gap-4 mb-4">
                    <div>
                        <p class="text-xs text-slate-500">Departemen Tujuan:</p>
                        <p class="font-bold text-base text-slate-800 truncate" id="fulfill-dept-name">-</p>
                    </div>
                    <div>
                        <p class="text-xs text-slate-500">Jumlah Dibutuhkan:</p>
                        <p class="font-bold text-red-600 text-base" id="fulfill-qty-needed">0 <span class="text-xs font-normal text-slate-500">Galon</span></p>
                    </div>
                </div>
                <hr class="my-3 border-slate-100">
                <div class="mb-4">
                    <label class="block text-xs sm:text-sm font-bold text-slate-700 mb-1.5">Jumlah Diisi (Galon)</label>
                    <input type="number" id="fulfill-qty-supplied" min="1" required class="w-full px-4 py-2.5 rounded-xl border border-slate-300 focus:ring-2 focus:ring-green-500 focus:border-green-500 outline-none bg-slate-50 text-sm">
                </div>
                <div class="mb-5">
                    <label class="block text-xs sm:text-sm font-bold text-slate-700 mb-1.5">Unggah Foto Bukti Pengiriman</label>                 
                    <!-- Standard Mobile-Friendly File Selector (Supports native iOS & Android Camera / Photo Sheet Selection) -->
                    <div class="relative border-2 border-dashed border-slate-300 hover:border-water-500 rounded-xl p-4 transition-colors bg-slate-50/50 flex flex-col items-center justify-center text-center cursor-pointer group" onclick="document.getElementById('fulfill-proof-file').click()">
                        <!-- Removed capture="environment" to allow photo library selection which is standard on iPhone & Android -->
                        <input type="file" id="fulfill-proof-file" accept="image/*" required class="hidden">
                        <div class="p-2.5 bg-white rounded-full shadow-sm text-slate-400 group-hover:text-water-500 transition-colors mb-1.5">
                            <i class="fa-solid fa-camera text-xl"></i>
                        </div>
                        <p class="text-xs font-bold text-slate-700">Ambil Foto / Pilih Gambar</p>
                        <p class="text-[10px] text-slate-400 mt-0.5">Kompresi otomatis berjalan instan</p>
                    </div>                  
                    <!-- Pratinjau Foto Kompresi -->
                    <div id="preview-container" class="mt-3 hidden bg-slate-100 p-2 rounded-xl border border-slate-200 flex flex-col items-center">
                        <div class="flex justify-between w-full items-center mb-1.5 px-1">
                            <span class="text-[11px] font-bold text-slate-600"><i class="fa-solid fa-circle-check text-green-500 mr-1"></i> Foto Siap Kirim</span>
                            <button type="button" onclick="resetUploadedFile()" class="text-xs text-red-500 hover:text-red-700 font-bold"><i class="fa-solid fa-trash mr-1"></i> Hapus</button>
                        </div>
                        <img id="image-preview" src="" alt="Preview Bukti" class="h-32 w-full object-cover rounded-lg border border-slate-300 shadow-sm">
                    </div>
                </div>
                <div class="flex space-x-3">
                    <button type="button" onclick="closeModal()" class="w-1/2 py-3 rounded-xl text-slate-600 font-bold hover:bg-slate-100 transition-colors text-sm btn-active-state">Batal</button>
                    <button type="submit" class="w-1/2 py-3 rounded-xl bg-green-600 hover:bg-green-700 text-white font-bold transition-colors shadow-md flex justify-center items-center text-sm btn-active-state">
                        <i class="fa-solid fa-circle-check mr-1.5"></i> Kirim Suplai
                    </button>
                </div>
            </form>
        </div>
    </div>
    <!-- Lightbox Modal untuk Foto Bukti (Pop-up modern untuk Android & iOS) -->
    <div id="modal-lightbox" class="fixed inset-0 bg-slate-950 bg-opacity-95 z-[80] hidden flex flex-col items-center justify-center p-4 backdrop-blur-md">
        <button onclick="closeLightbox()" class="absolute top-4 right-4 text-white hover:text-red-400 transition-colors bg-slate-800/80 p-3 rounded-full shadow-lg z-[90]">
            <i class="fa-solid fa-times text-2xl"></i>
        </button>
        <div class="max-w-3xl w-full flex flex-col items-center">
            <p id="lightbox-title" class="text-white text-sm sm:text-base font-bold mb-4 bg-slate-900/80 px-4 py-2 rounded-full border border-slate-700 shadow-lg text-center"></p>
            <div class="relative w-full flex justify-center">
                <img id="lightbox-image" src="" alt="Bukti Foto" class="max-h-[75vh] max-w-full rounded-2xl shadow-2xl border-2 border-slate-700 object-contain">
            </div>
        </div>
    </div>
    <!-- Container Notifikasi -->
    <div id="toast-container" class="fixed bottom-20 sm:bottom-5 right-4 left-4 sm:left-auto sm:right-5 z-[70] flex flex-col gap-2"></div>
    <script>
        const departments = [
            "Project", "Kawasan berikat", "Pome", "Tank farm", "KCP", 
            "Solvent", "Biodiesel", "Refinery", "Office", "Store", 
            "Kantin", "SWRO", "Boiler", "HSE", "Maintenance"
        ];
        let tickets = [];
        let ticketCounter = 1000;
        let currentUploadedBase64 = "";
        let selectedWeatherMode = "auto"; // "auto", "cerah", "berawan", "hujan", "panas", "malam"
        document.addEventListener('DOMContentLoaded', () => {
            initApp();
        });
        function initApp() {
            const select = document.getElementById('dept-select');
            departments.forEach(dept => {
                const option = document.createElement('option');
                option.value = dept;
                option.textContent = dept;
                select.appendChild(option);
            });
            generateDummyData();
            document.getElementById('fulfill-proof-file').addEventListener('change', function(event) {
                const file = event.target.files[0];
                if (file) {
                    processImageFile(file).then(base64Str => {
                        currentUploadedBase64 = base64Str;
                        const previewContainer = document.getElementById('preview-container');
                        const previewImage = document.getElementById('image-preview');
                        previewImage.src = base64Str;
                        previewContainer.classList.remove('hidden');
                    }).catch(err => {
                        showToast("Gagal memproses gambar. Coba file lain.", "error");
                        resetUploadedFile();
                    });
                } else {
                    resetUploadedFile();
                }
            });
            // Jalankan deteksi cuaca otomatis berkala
            runWeatherEngine();
            setInterval(runWeatherEngine, 10000); // Periksa otomatis setiap 10 detik
            updateAllViews();
        }
        function resetUploadedFile() {
            document.getElementById('fulfill-proof-file').value = "";
            currentUploadedBase64 = "";
            document.getElementById('image-preview').src = "";
            document.getElementById('preview-container').classList.add('hidden');
        }
        // Kompresor Gambar Mobile-First untuk hemat memori
        function processImageFile(file) {
            return new Promise((resolve, reject) => {
                const reader = new FileReader();
                reader.onload = function(e) {
                    const img = new Image();
                    img.onload = function() {
                        const canvas = document.createElement('canvas');
                        let width = img.width;
                        let height = img.height;
                        const max_size = 750; // Resolusi optimal & ringan untuk browser seluler
                        if (width > height) {
                            if (width > max_size) {
                                height *= max_size / width;
                                width = max_size;
                            }
                        } else {
                            if (height > max_size) {
                                width *= max_size / height;
                                height = max_size;
                            }
                        }
                        canvas.width = width;
                        canvas.height = height;
                        const ctx = canvas.getContext('2d');
                        ctx.drawImage(img, 0, 0, width, height);     
                        resolve(canvas.toDataURL('image/jpeg', 0.70)); // Hemat memori s/d 80%
                    };
                    img.onerror = reject;
                    img.src = e.target.result;
                };
                reader.onerror = reject;
                reader.readAsDataURL(file);
            });
        }
        // WEATHER ENGINE UTILITY
        function runWeatherEngine() {
            if (selectedWeatherMode !== "auto") {
                applyWeatherStyle(selectedWeatherMode);
                return;
            }
            const hour = new Date().getHours(); 
            // Logika Deteksi Cuaca Alami Berdasarkan Jam Real-time
            if (hour >= 18 || hour < 6) {
                applyWeatherStyle("malam");
            } else if (hour >= 11 && hour <= 13) {
                applyWeatherStyle("panas"); // Jam makan siang/tengah hari sangat panas
            } else if (hour >= 14 && hour < 16) {
                applyWeatherStyle("berawan"); // Sore berawan
            } else {
                applyWeatherStyle("cerah"); // Pagi / siang biasa cerah
            }
        }
        function manualWeatherChange(val) {
            selectedWeatherMode = val;
            runWeatherEngine();
        }
        // Menerapkan gaya visual dan ikon cuaca berdasarkan status terpilih
        function applyWeatherStyle(type) {
            const card = document.getElementById('weather-card');
            const iconContainer = document.getElementById('weather-icon');
            const statusText = document.getElementById('weather-status-text');
            const tempText = document.getElementById('weather-temp');
            const alertBanner = document.getElementById('weather-alert-banner');
            const badgePanas = document.getElementById('weather-badge-panas');
            // Default Sembunyikan Alert Panas & Badge
            alertBanner.classList.add('hidden');
            badgePanas.classList.add('hidden');
            switch(type) {
                case "cerah":
                    card.className = "bg-gradient-to-r from-sky-400 to-blue-500 rounded-2xl p-4 text-white shadow-md mb-6 flex flex-col sm:flex-row items-start sm:items-center justify-between gap-4 transition-all duration-500";
                    iconContainer.innerHTML = `<i class="fa-solid fa-sun text-yellow-300 text-3xl animate-spin-slow"></i>`;
                    statusText.textContent = "Cerah - Siang Hari";
                    tempText.textContent = "Suhu: 31°C (Kondisi Produksi Optimal)";
                    break;
                case "berawan":
                    card.className = "bg-gradient-to-r from-sky-400 via-sky-500 to-slate-400 rounded-2xl p-4 text-white shadow-md mb-6 flex flex-col sm:flex-row items-start sm:items-center justify-between gap-4 transition-all duration-500";
                    iconContainer.innerHTML = `<i class="fa-solid fa-cloud-sun text-slate-100 text-3xl"></i>`;
                    statusText.textContent = "Cerah Berawan";
                    tempText.textContent = "Suhu: 29°C (Kelembapan Sedang)";
                    break;
                case "hujan":
                    card.className = "bg-gradient-to-r from-slate-600 via-slate-700 to-blue-800 rounded-2xl p-4 text-white shadow-md mb-6 flex flex-col sm:flex-row items-start sm:items-center justify-between gap-4 transition-all duration-500";
                    iconContainer.innerHTML = `<i class="fa-solid fa-cloud-showers-heavy text-cyan-300 text-3xl animate-bounce-subtle"></i>`;
                    statusText.textContent = "Hujan Ringan - Sedang";
                    tempText.textContent = "Suhu: 25°C (Waspada Area Licin)";
                    break;
                case "panas":
                    // Desain Menyala Sangat Terik
                    card.className = "bg-gradient-to-r from-amber-500 via-orange-600 to-red-600 rounded-2xl p-4 text-white shadow-md mb-6 flex flex-col sm:flex-row items-start sm:items-center justify-between gap-4 transition-all duration-500 ring-4 ring-orange-400/30";
                    iconContainer.innerHTML = `<i class="fa-solid fa-temperature-high text-yellow-200 text-3xl animate-pulse"></i>`;
                    statusText.textContent = "Terik Sangat Panas!";
                    tempText.textContent = "Suhu: 36°C - Panas Ekstrem";           
                    // Memunculkan Peringatan Panas Ekstrem
                    alertBanner.classList.remove('hidden');
                    badgePanas.classList.remove('hidden');
                    break;
                case "malam":
                    card.className = "bg-gradient-to-r from-slate-900 via-indigo-950 to-slate-900 rounded-2xl p-4 text-white shadow-md mb-6 flex flex-col sm:flex-row items-start sm:items-center justify-between gap-4 transition-all duration-500 border border-indigo-800/40";
                    iconContainer.innerHTML = `<i class="fa-solid fa-moon text-yellow-100 text-3xl"></i>`;
                    statusText.textContent = "Malam Hari - Cerah Berbintang";
                    tempText.textContent = "Suhu: 26°C (Operasional Shift Malam)";
                    break;
            }
        }
        function switchTab(tabId) {
            // Sembunyikan semua section
            document.querySelectorAll('.view-section').forEach(el => el.classList.add('hidden'));
            document.getElementById(`view-${tabId}`).classList.remove('hidden');
            // Reset desktop tab styling
            document.querySelectorAll('.nav-btn').forEach(btn => {
                btn.classList.remove('bg-water-100', 'text-water-800');
                btn.classList.add('text-slate-600');
            });
            const activeBtn = document.getElementById(`nav-${tabId}`);
            if (activeBtn) {
                activeBtn.classList.remove('text-slate-600');
                activeBtn.classList.add('bg-water-100', 'text-water-800');
            }
            // Reset mobile bottom nav styling
            document.querySelectorAll('.mob-nav-btn').forEach(btn => {
                btn.classList.remove('text-water-600');
                btn.classList.add('text-slate-400');
            });
            const activeMobBtn = document.getElementById(`mob-nav-${tabId}`);
            if (activeMobBtn) {
                activeMobBtn.classList.remove('text-slate-400');
                activeMobBtn.classList.add('text-water-600');
            }
            // Tutup modal jika terbuka saat navigasi tab
            closeModal();
            updateAllViews();
        }
        function showToast(message, type = 'success') {
            const container = document.getElementById('toast-container');
            const toast = document.createElement('div');   
            let bgClass = 'bg-white border-l-4 border-green-500';
            let iconClass = 'fa-check-circle text-green-500';   
            if (type === 'error') {
                bgClass = 'bg-white border-l-4 border-red-500';
                iconClass = 'fa-circle-xmark text-red-500';
            } else if (type === 'info') {
                bgClass = 'bg-white border-l-4 border-water-500';
                iconClass = 'fa-info-circle text-water-500';
            }
            toast.className = `flex items-center w-full max-w-sm p-4 space-x-3 text-slate-800 rounded-xl shadow-xl border border-slate-100 toast-enter ${bgClass}`;
            toast.innerHTML = `
                <i class="fa-solid ${iconClass} text-lg"></i>
                <div class="text-xs sm:text-sm font-semibold">${message}</div>
            `;
            container.appendChild(toast);
            setTimeout(() => {
                toast.style.opacity = '0';
                toast.style.transform = 'translateY(20px)';
                toast.style.transition = 'all 0.3s ease-in';
                setTimeout(() => container.removeChild(toast), 300);
            }, 3000);
        }
        function handleRequestSubmit(e) {
            e.preventDefault();
            const dept = document.getElementById('dept-select').value;
            const qty = parseInt(document.getElementById('qty-empty').value);
            const existingPending = tickets.find(t => t.dept === dept && t.status === 'pending');
            if (existingPending) {
                showToast(`Departemen ${dept} sedang dalam antrean suplai.`, 'error');
                return;
            }
            const newTicket = {
                id: `TKT-${ticketCounter++}`,
                dept: dept,
                requestedQty: qty,
                requestTime: new Date(),
                suppliedQty: 0,
                supplyTime: null,
                proof: "",
                status: 'pending'
            };
            tickets.unshift(newTicket);
            document.getElementById('form-request').reset();   
            showToast(`Request ${qty} galon untuk ${dept} berhasil!`, 'success');
            updateAllViews();    
            setTimeout(() => switchTab('dashboard'), 800);
        }
        let currentFulfillId = null;
        function openFulfillModal(ticketId) {
            const ticket = tickets.find(t => t.id === ticketId);
            if (!ticket) return;
            currentFulfillId = ticket.id;
            document.getElementById('fulfill-dept-name').textContent = ticket.dept;
            document.getElementById('fulfill-qty-needed').innerHTML = `${ticket.requestedQty} <span class="text-xs font-normal text-slate-500">Galon</span>`;
            document.getElementById('fulfill-qty-supplied').value = ticket.requestedQty;    
            resetUploadedFile();
            document.getElementById('modal-suplai').classList.remove('hidden');
        }
        function closeModal() {
            document.getElementById('modal-suplai').classList.add('hidden');
            currentFulfillId = null;
        }
        function handleFulfillSubmit(e) {
            e.preventDefault();
            const ticketIndex = tickets.findIndex(t => t.id === currentFulfillId);   
            if (ticketIndex > -1) {
                const suppliedQty = parseInt(document.getElementById('fulfill-qty-supplied').value);    
                if (!currentUploadedBase64) {
                    showToast("Harap upload/ambil foto bukti pengiriman!", "error");
                    return;
                }
                tickets[ticketIndex].status = 'completed';
                tickets[ticketIndex].suppliedQty = suppliedQty;
                tickets[ticketIndex].supplyTime = new Date();
                tickets[ticketIndex].proofImage = currentUploadedBase64;
                tickets[ticketIndex].proofFileName = "bukti_pengiriman.jpg";
                closeModal();
                showToast(`Berhasil menyuplai ${tickets[ticketIndex].dept}!`, 'success');
                updateAllViews();
            }
        }
        function openLightbox(imageSrc, title) {
            document.getElementById('lightbox-image').src = imageSrc;
            document.getElementById('lightbox-title').textContent = title;
            document.getElementById('modal-lightbox').classList.remove('hidden');
            document.body.style.overflow = 'hidden';
        }
        function closeLightbox() {
            document.getElementById('modal-lightbox').classList.add('hidden');
            document.getElementById('lightbox-image').src = '';
            document.body.style.overflow = '';
        }
        function updateAllViews() {
            renderDashboard();
            renderSupplierTable();
            renderReportTable();
        }
        function formatDate(date) {
            if (!date) return "-";
            const d = new Date(date);
            return `${d.getDate().toString().padStart(2,'0')}/${(d.getMonth()+1).toString().padStart(2,'0')} ${d.getHours().toString().padStart(2,'0')}:${d.getMinutes().toString().padStart(2,'0')}`;
        }
        function renderDashboard() {
            const grid = document.getElementById('dashboard-grid');
            grid.innerHTML = '';  
            let totalEmpty = 0;
            let totalSuppliedToday = 0;
            const todayStr = new Date().toDateString();
            document.getElementById('dash-total-dept').textContent = departments.length;
            departments.forEach(dept => {
                const pendingTicket = tickets.find(t => t.dept === dept && t.status === 'pending');
                const completedToday = tickets.filter(t => t.dept === dept && t.status === 'completed' && new Date(t.supplyTime).toDateString() === todayStr);
                totalSuppliedToday += completedToday.reduce((sum, t) => sum + t.suppliedQty, 0);
                let cardClass = "bg-white border-b-4 border-green-500 shadow-sm hover:shadow-md";
                let iconColor = "text-green-500";
                let badgeHtml = `<span class="px-1.5 py-0.5 bg-green-100 text-green-700 text-[10px] rounded-md font-semibold">Aman</span>`;
                let detailHtml = `<p class="text-[10px] text-slate-400 mt-1">Normal</p>`;
                if (pendingTicket) {
                    totalEmpty += pendingTicket.requestedQty;
                    cardClass = "bg-red-50 border-b-4 border-red-500 shadow animate-pulse-slow";
                    iconColor = "text-red-500";
                    badgeHtml = `<span class="px-1.5 py-0.5 bg-red-200 text-red-800 text-[10px] rounded-md font-bold">BUTUH!</span>`;
                    detailHtml = `<p class="text-xs font-bold text-red-600 mt-1">Req: ${pendingTicket.requestedQty} Gln</p>`;
                }
                const card = `
                    <div class="${cardClass} rounded-2xl p-3.5 flex flex-col items-center text-center relative overflow-hidden transition-all duration-200">
                        <div class="absolute top-1.5 right-1.5">${badgeHtml}</div>
                        <div class="w-10 h-10 rounded-full bg-slate-50 flex items-center justify-center mt-2.5 mb-2 shadow-inner">
                            <i class="fa-solid fa-bottle-water text-lg ${iconColor}"></i>
                        </div>
                        <h3 class="font-bold text-slate-800 text-xs sm:text-sm w-full truncate px-1" title="${dept}">${dept}</h3>
                        ${detailHtml}
                    </div>
                `;
                grid.innerHTML += card;
            });
            document.getElementById('dash-total-empty').textContent = totalEmpty;
            document.getElementById('dash-total-supplied').textContent = totalSuppliedToday;
        }
        function renderSupplierTable() {
            const tbody = document.getElementById('supplier-table-body');
            const mobileList = document.getElementById('supplier-mobile-list');
            const emptyState = document.getElementById('supplier-empty-state');
            tbody.innerHTML = '';
            mobileList.innerHTML = ''; 
            const pendingTickets = tickets.filter(t => t.status === 'pending');
            // Update badge notifikasi merah di navigasi bawah ponsel
            const badge = document.getElementById('badge-count-mobile');
            if (pendingTickets.length > 0) {
                badge.textContent = pendingTickets.length;
                badge.classList.remove('hidden');
            } else {
                badge.classList.add('hidden');
            }
            if (pendingTickets.length === 0) {
                tbody.parentElement.parentElement.classList.add('hidden');
                emptyState.classList.remove('hidden');
                return;
            }
            tbody.parentElement.parentElement.classList.remove('hidden');
            emptyState.classList.add('hidden');
            pendingTickets.forEach(t => {
                // Render Desktop Table
                const tr = document.createElement('tr');
                tr.className = "hover:bg-red-50/20 transition-colors bg-red-50/10";
                tr.innerHTML = `
                    <td class="px-6 py-4 whitespace-nowrap text-sm text-slate-500"><i class="fa-regular fa-clock mr-1"></i> ${formatDate(t.requestTime)}</td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm font-bold text-slate-800">${t.dept}</td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm text-red-600 font-bold text-lg">${t.requestedQty}</td>
                    <td class="px-6 py-4 whitespace-nowrap">
                        <span class="px-3 py-1 inline-flex text-xs leading-5 font-semibold rounded-full bg-red-100 text-red-800">
                            <i class="fa-solid fa-clock mr-1.5 mt-0.5"></i> Pending
                        </span>
                    </td>
                    <td class="px-6 py-4 whitespace-nowrap text-center text-sm font-medium">
                        <button onclick="openFulfillModal('${t.id}')" class="bg-water-600 hover:bg-water-700 text-white px-4 py-2 rounded-xl shadow-sm btn-active-state">
                            <i class="fa-solid fa-truck-fast mr-1"></i> Proses Suplai
                        </button>
                    </td>
                `;
                tbody.appendChild(tr);
                // Render Mobile List Card (Touch-Friendly)
                const mobCard = `
                    <div class="bg-white rounded-2xl p-4 shadow-sm border border-slate-100 flex justify-between items-center bg-gradient-to-r from-red-50/30 to-transparent">
                        <div>
                            <div class="flex items-center space-x-2">
                                <span class="px-2 py-0.5 bg-red-100 text-red-800 text-[10px] rounded-md font-bold">Pending</span>
                                <span class="text-[10px] text-slate-400 font-mono">${t.id}</span>
                            </div>
                            <h3 class="font-bold text-slate-800 text-base mt-1">${t.dept}</h3>
                            <p class="text-[11px] text-slate-500 mt-1"><i class="fa-regular fa-clock mr-1"></i>${formatDate(t.requestTime)}</p>
                        </div>
                        <div class="text-right flex flex-col items-end">
                            <p class="text-2xl font-black text-red-600">${t.requestedQty}<span class="text-[11px] font-semibold text-slate-400"> Gln</span></p>
                            <button onclick="openFulfillModal('${t.id}')" class="mt-2 bg-water-600 active:bg-water-700 text-white text-xs font-bold py-2 px-3.5 rounded-xl shadow btn-active-state">
                                <i class="fa-solid fa-truck-fast mr-1"></i> Suplai
                            </button>
                        </div>
                    </div>
                `;
                mobileList.innerHTML += mobCard;
            });
        }
        function renderReportTable() {
            const tbody = document.getElementById('report-table-body');
            const mobileList = document.getElementById('report-mobile-list');
            tbody.innerHTML = '';
            mobileList.innerHTML = '';   
            if (tickets.length === 0) {
                tbody.innerHTML = `<tr><td colspan="8" class="px-6 py-8 text-center text-slate-400">Belum ada data riwayat.</td></tr>`;
                mobileList.innerHTML = `<div class="text-center py-10 text-slate-400 text-xs">Belum ada data riwayat.</div>`;
                return;
            }
            tickets.forEach(t => {
                const isCompleted = t.status === 'completed';
                const statusBadge = isCompleted 
                    ? `<span class="px-2.5 py-1 inline-flex text-xs leading-5 font-semibold rounded-full bg-green-100 text-green-800"><i class="fa-solid fa-check mr-1 mt-0.5"></i> Selesai</span>`
                    : `<span class="px-2.5 py-1 inline-flex text-xs leading-5 font-semibold rounded-full bg-red-100 text-red-800"><i class="fa-solid fa-clock mr-1 mt-0.5"></i> Pending</span>`;
                let proofHtml = '-';
                let mobileProofHtml = '';   
                if (isCompleted && t.proofImage) {
                    proofHtml = `
                    <div onclick="openLightbox('${t.proofImage}', 'Bukti Foto: Departemen ${t.dept}')" class="cursor-pointer group flex items-center space-x-1.5" title="Klik untuk memperbesar">
                        <img src="${t.proofImage}" alt="Bukti" class="h-9 w-12 object-cover rounded-lg shadow-sm border border-slate-200 transition-transform group-hover:scale-105">
                        <span class="text-[11px] text-water-600 hover:underline"><i class="fa-solid fa-magnifying-glass-plus"></i> Lihat</span>
                    </div>`;
                    mobileProofHtml = `
                    <div class="mt-2 flex items-center justify-between bg-slate-50 p-2 rounded-xl border border-slate-100">
                        <span class="text-[10px] font-bold text-slate-500">Bukti Pengiriman:</span>
                        <button onclick="openLightbox('${t.proofImage}', 'Bukti Foto: Departemen ${t.dept}')" class="flex items-center space-x-1 bg-white hover:bg-slate-100 text-water-600 text-[10px] font-bold px-2 py-1 rounded-lg border border-slate-200 shadow-sm btn-active-state">
                            <i class="fa-solid fa-camera mr-1 text-slate-400"></i> Buka Foto
                        </button>
                    </div>`;
                }
                // Desktop View Row
                const tr = document.createElement('tr');
                tr.className = "hover:bg-slate-50 transition-colors";
                tr.innerHTML = `
                    <td class="px-6 py-3.5 whitespace-nowrap text-slate-500 font-mono text-xs">${t.id}</td>
                    <td class="px-6 py-3.5 whitespace-nowrap font-semibold text-slate-800">${t.dept}</td>
                    <td class="px-6 py-3.5 whitespace-nowrap text-slate-500">${formatDate(t.requestTime)}</td>
                    <td class="px-6 py-3.5 whitespace-nowrap font-bold text-red-500">${t.requestedQty}</td>
                    <td class="px-6 py-3.5 whitespace-nowrap text-slate-500">${formatDate(t.supplyTime)}</td>
                    <td class="px-6 py-3.5 whitespace-nowrap font-bold ${isCompleted ? 'text-green-600' : 'text-slate-400'}">${isCompleted ? t.suppliedQty : '-'}</td>
                    <td class="px-6 py-3.5 whitespace-nowrap text-slate-500">${proofHtml}</td>
                    <td class="px-6 py-3.5 whitespace-nowrap">${statusBadge}</td>
                `;
                tbody.appendChild(tr);
                // Mobile View Card (Touch-Friendly)
                const mobileCard = `
                    <div class="bg-white rounded-2xl p-4 shadow-sm border border-slate-100 text-xs space-y-2">
                        <div class="flex justify-between items-center">
                            <span class="font-mono text-[10px] text-slate-400">${t.id}</span>
                            ${statusBadge}
                        </div>
                        <h4 class="font-extrabold text-slate-800 text-sm">${t.dept}</h4>
                        <div class="grid grid-cols-2 gap-2 text-[11px] text-slate-500 pt-1">
                            <div>
                                <p class="text-[10px] text-slate-400">Waktu Request:</p>
                                <p class="font-semibold text-slate-600">${formatDate(t.requestTime)}</p>
                                <p class="text-[10px] text-red-500 font-bold mt-1">Req: ${t.requestedQty} Galon</p>
                            </div>
                            <div>
                                <p class="text-[10px] text-slate-400">Waktu Suplai:</p>
                                <p class="font-semibold text-slate-600">${formatDate(t.supplyTime)}</p>
                                <p class="text-[10px] text-green-600 font-bold mt-1">Isi: ${isCompleted ? t.suppliedQty : '-'} Galon</p>
                            </div>
                        </div>
                        ${mobileProofHtml}
                    </div>
                `;
                mobileList.innerHTML += mobileCard;
            });
        }
        function exportToExcel() {
            if (tickets.length === 0) {
                showToast("Tidak ada data untuk diexport", "info");
                return;
            }
            let csvContent = "sep=,\n";
            csvContent += "ID Tiket,Departemen,Waktu Request,Jml Request (Galon),Status,Waktu Suplai,Jml Disuplai (Galon),Nama File Foto\n";
            tickets.forEach(t => {
                const id = t.id;
                const dept = `"${t.dept.replace(/"/g, '""')}"`;
                const reqTime = `"${formatDate(t.requestTime)}"`;
                const reqQty = t.requestedQty;
                const status = t.status === 'completed' ? 'Selesai' : 'Pending';
                const supTime = `"${formatDate(t.supplyTime)}"`;
                const supQty = t.status === 'completed' ? t.suppliedQty : 0;
                const proofStr = t.proofFileName || t.proof || '';
                const proof = proofStr ? `"${proofStr.replace(/"/g, '""')}"` : '""';
                csvContent += `${id},${dept},${reqTime},${reqQty},${status},${supTime},${supQty},${proof}\n`;
            });
            const blob = new Blob(["\ufeff" + csvContent], { type: 'text/csv;charset=utf-8;' });
            const link = document.createElement("a");
            const url = URL.createObjectURL(blob); 
            const dateStr = new Date().toISOString().slice(0,10);
            link.setAttribute("href", url);
            link.setAttribute("download", `Laporan_Air_Galonku_${dateStr}.csv`);
            link.style.visibility = 'hidden';  
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
            showToast("Laporan ter-export dengan rapi per kolom!", "success");
        }
        function generateDummyData() {
            const now = new Date();
            const past1 = new Date(now.getTime() - (2 * 60 * 60 * 1000));
            const past2 = new Date(now.getTime() - (1 * 60 * 60 * 1000)); 
            tickets.push({
                id: `TKT-${ticketCounter++}`, dept: "Office", requestedQty: 5, requestTime: past1,
                suppliedQty: 5, supplyTime: past2, proof: "Tanda terima Pak Budi",
                proofImage: "https://placehold.co/400x300/e0f2fe/0ea5e9?text=Bukti+Foto+Office",
                proofFileName: "bukti_office.jpg",
                status: 'completed'
            });
            tickets.push({
                id: `TKT-${ticketCounter++}`, dept: "Kantin", requestedQty: 10, requestTime: new Date(now.getTime() - (24 * 60 * 60 * 1000)),
                suppliedQty: 10, supplyTime: new Date(now.getTime() - (22 * 60 * 60 * 1000)), proof: "DO-00123",
                proofImage: "https://placehold.co/400x300/e0f2fe/0ea5e9?text=Bukti+Foto+Kantin",
                proofFileName: "DO_00123.jpg",
                status: 'completed'
            });
            tickets.push({
                id: `TKT-${ticketCounter++}`, dept: "Boiler", requestedQty: 3, requestTime: new Date(now.getTime() - (30 * 60 * 1000)),
                suppliedQty: 0, supplyTime: null, proof: "", status: 'pending'
            });  
            tickets.push({
                id: `TKT-${ticketCounter++}`, dept: "HSE", requestedQty: 2, requestTime: new Date(now.getTime() - (10 * 60 * 1000)),
                suppliedQty: 0, supplyTime: null, proof: "", status: 'pending'
            });
        }
        const style = document.createElement('style');
        style.innerHTML = `
            .animate-pulse-slow {
                animation: pulse-slow 3.2s cubic-bezier(0.4, 0, 0.6, 1) infinite;
            }
            @keyframes pulse-slow {
                0%, 100% { opacity: 1; transform: scale(1); }
                50% { opacity: .93; transform: scale(1.02); box-shadow: 0 8px 16px -3px rgba(239, 68, 68, 0.25); }
            }
            .animate-bounce-slow {
                animation: bounce-slow 3s infinite;
            }
            @keyframes bounce-slow {
                0%, 100% { transform: translateY(-5%); animation-timing-function: cubic-bezier(0.8,0,1,1); }
                50% { transform: none; animation-timing-function: cubic-bezier(0,0,0.2,1); }
            }
            @keyframes fadeIn {
                from { opacity: 0; transform: translateY(5px); }
                to { opacity: 1; transform: translateY(0); }
            }
            .animate-fade-in {
                animation: fadeIn 0.25s ease-out forwards;
            }
            .animate-spin-slow {
                animation: spin-slow 15s linear infinite;
            }
            @keyframes spin-slow {
                0% { transform: rotate(0deg); }
                100% { transform: rotate(360deg); }
            }
            .animate-bounce-subtle {
                animation: bounce-subtle 3s ease-in-out infinite;
            }
            @keyframes bounce-subtle {
                0%, 100% { transform: translateY(0); }
                50% { transform: translateY(-3px); }
            }
        `;
        document.head.appendChild(style);
npm install firebase
import { initializeApp } from "firebase/app";
import { getFirestore } from "firebase/firestore";
const firebaseConfig = {
  apiKey: "API_KEY",
  authDomain: "PROJECT_ID.firebaseapp.com",
  projectId: "PROJECT_ID",
  storageBucket: "PROJECT_ID.appspot.com",
  messagingSenderId: "SENDER_ID",
  appId: "APP_ID"
};
const app = initializeApp(firebaseConfig);
export const db = getFirestore(app);
import { collection, addDoc } from "firebase/firestore";
import { db } from "./firebase";
async function addRequest(department, jumlah) {
  await addDoc(collection(db, "requests"), {
    department: department,
    jumlah: jumlah,
    timestamp: new Date()
  });
}
import { collection, onSnapshot } from "firebase/firestore";
import { db } from "./firebase";
onSnapshot(collection(db, "requests"), (snapshot) => {
  snapshot.docs.forEach(doc => {
    console.log(doc.data());
  });
});


    </script>
</body>
</html>

