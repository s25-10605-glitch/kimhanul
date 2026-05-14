# kimhanul
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>RecordPro - 학생 생활기록부 관리 시스템</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://unpkg.com/lucide@latest"></script>
    <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@300;400;500;700;900&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Noto Sans KR', sans-serif; background-color: #f8fafc; }
        .paper-shadow { shadow-lg shadow-indigo-200/50; }
        @media print {
            .no-print { display: none !important; }
            .print-only { display: block !important; }
            body { background-color: white; }
        }
    </style>
</head>
<body>
    <div id="app">
        <!-- Login Screen -->
        <div id="login-screen" class="min-h-screen flex items-center justify-center p-4">
            <div class="max-w-md w-full bg-white rounded-3xl shadow-2xl p-10 border border-slate-100 text-center animate-in fade-in duration-700">
                <div class="bg-indigo-600 w-16 h-16 rounded-2xl flex items-center justify-center mx-auto mb-6 shadow-lg shadow-indigo-200">
                    <i data-lucide="graduation-cap" class="text-white w-8 h-8"></i>
                </div>
                <h1 class="text-2xl font-bold text-slate-800 mb-2">학생 기록 관리 시스템</h1>
                <p class="text-slate-500 mb-8 text-sm">본인의 이름을 입력하여 시작하세요.</p>
                <div class="space-y-4">
                    <input type="text" id="login-name" placeholder="이름 입력 (예: 홍길동)" 
                        class="w-full px-5 py-4 bg-slate-50 border border-slate-200 rounded-2xl outline-none focus:ring-2 focus:ring-indigo-500 transition-all text-center font-bold">
                    <button onclick="handleLogin()" class="w-full bg-indigo-600 text-white py-4 rounded-2xl font-bold hover:bg-indigo-700 transition shadow-lg shadow-indigo-100">
                        로그인하기
                    </button>
                </div>
                <p class="mt-8 text-[10px] text-slate-400 uppercase tracking-widest">Powered by RecordPro Engine</p>
            </div>
        </div>

        <!-- Main Dashboard -->
        <div id="main-content" class="hidden min-h-screen flex text-slate-900">
            <!-- Sidebar -->
            <aside class="w-72 bg-white border-r border-slate-200 flex flex-col sticky top-0 h-screen hidden lg:flex no-print">
                <div class="p-8">
                    <div class="flex items-center gap-3 text-indigo-600">
                        <div class="bg-indigo-600 p-1.5 rounded-lg text-white"><i data-lucide="file-text" class="w-5 h-5"></i></div>
                        <span class="font-black text-xl tracking-tight uppercase">RecordPro</span>
                    </div>
                </div>

                <nav class="flex-grow px-4 space-y-1">
                    <p class="px-4 text-[10px] font-bold text-slate-400 uppercase tracking-widest mb-2">Main Menu</p>
                    <button onclick="switchTab('dashboard')" id="btn-dashboard" class="nav-btn w-full flex items-center gap-3 px-4 py-3 rounded-xl transition font-bold bg-indigo-50 text-indigo-600">
                        <i data-lucide="layout-dashboard" class="w-[18px] h-[18px]"></i> 대시보드
                    </button>
                    <button onclick="openEditor()" id="btn-editor" class="nav-btn w-full flex items-center gap-3 px-4 py-3 rounded-xl transition font-bold text-slate-500 hover:bg-slate-50">
                        <i data-lucide="plus" class="w-[18px] h-[18px]"></i> 새 기록 작성
                    </button>
                    <button onclick="switchTab('preview')" id="btn-preview" class="nav-btn w-full flex items-center gap-3 px-4 py-3 rounded-xl transition font-bold text-slate-500 hover:bg-slate-50">
                        <i data-lucide="eye" class="w-[18px] h-[18px]"></i> 생기부 미리보기
                    </button>

                    <div class="mt-10">
                        <p class="px-4 text-[10px] font-bold text-slate-400 uppercase tracking-widest mb-2">Categories</p>
                        <div class="space-y-1">
                            <div class="flex items-center gap-3 px-4 py-3 rounded-xl text-slate-400 font-medium"><i data-lucide="graduation-cap" class="w-[18px] h-[18px]"></i> 교과 학습</div>
                            <div class="flex items-center gap-3 px-4 py-3 rounded-xl text-slate-400 font-medium"><i data-lucide="users" class="w-[18px] h-[18px]"></i> 창체 활동</div>
                            <div class="flex items-center gap-3 px-4 py-3 rounded-xl text-slate-400 font-medium"><i data-lucide="book-open" class="w-[18px] h-[18px]"></i> 독서 활동</div>
                            <div class="flex items-center gap-3 px-4 py-3 rounded-xl text-slate-400 font-medium"><i data-lucide="calendar" class="w-[18px] h-[18px]"></i> 출결 상황</div>
                        </div>
                    </div>
                </nav>

                <div class="p-6 border-t border-slate-100 mt-auto">
                    <div class="bg-slate-50 rounded-2xl p-4 flex items-center justify-between">
                        <div class="flex items-center gap-3">
                            <div id="user-avatar" class="w-10 h-10 bg-indigo-100 rounded-full flex items-center justify-center text-indigo-600 font-bold"></div>
                            <div>
                                <p id="user-display-name" class="text-sm font-bold"></p>
                                <p class="text-[10px] text-slate-400">Student</p>
                            </div>
                        </div>
                        <button onclick="handleLogout()" class="text-slate-400 hover:text-red-500 transition"><i data-lucide="log-out" class="w-[18px] h-[18px]"></i></button>
                    </div>
                </div>
            </aside>

            <!-- Content -->
            <main class="flex-grow p-6 lg:p-12 overflow-y-auto">
                <!-- Dashboard Tab -->
                <div id="tab-dashboard" class="tab-content">
                    <header class="flex justify-between items-center mb-10">
                        <div>
                            <h2 class="text-3xl font-black text-slate-800">환영합니다!</h2>
                            <p class="text-slate-500 text-sm mt-1">오늘의 활동을 세세하게 기록해 보세요.</p>
                        </div>
                        <button class="bg-white p-2.5 rounded-xl border border-slate-200 text-slate-500 hover:bg-slate-50 transition"><i data-lucide="award" class="w-5 h-5"></i></button>
                    </header>
                    
                    <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mb-8">
                        <div class="bg-white p-6 rounded-3xl border border-slate-100 shadow-sm">
                            <p class="text-slate-400 text-xs font-bold uppercase mb-2">총 기록 수</p>
                            <p id="stat-count" class="text-3xl font-black text-indigo-600">0</p>
                        </div>
                        <div class="bg-white p-6 rounded-3xl border border-slate-100 shadow-sm">
                            <p class="text-slate-400 text-xs font-bold uppercase mb-2">전체 바이트</p>
                            <p id="stat-bytes" class="text-3xl font-black text-orange-500">0</p>
                        </div>
                        <div class="bg-white p-6 rounded-3xl border border-slate-100 shadow-sm">
                            <p class="text-slate-400 text-xs font-bold uppercase mb-2">활동 학년</p>
                            <p class="text-3xl font-black text-emerald-500">전학년</p>
                        </div>
                    </div>

                    <div class="bg-white rounded-3xl border border-slate-200 overflow-hidden shadow-sm">
                        <div class="p-6 border-b border-slate-100 flex justify-between items-center">
                            <h3 class="font-bold text-lg">최근 작성 목록</h3>
                        </div>
                        <div id="records-list" class="divide-y divide-slate-50">
                            <!-- Records will be injected here -->
                        </div>
                    </div>
                </div>

                <!-- Editor Tab -->
                <div id="tab-editor" class="tab-content hidden">
                    <div class="grid grid-cols-1 lg:grid-cols-3 gap-8">
                        <div class="lg:col-span-2 space-y-6">
                            <div class="bg-white rounded-[2rem] border border-slate-200 shadow-xl overflow-hidden">
                                <div class="px-8 py-6 border-b border-slate-100 flex justify-between items-center bg-slate-50/30">
                                    <h3 class="font-black text-xl text-slate-800 flex items-center gap-2">
                                        <i data-lucide="pencil-line" class="text-indigo-600 w-6 h-6"></i> 상세 기록 작성
                                    </h3>
                                    <button onclick="saveRecord()" class="bg-indigo-600 text-white px-6 py-2.5 rounded-xl font-bold text-sm shadow-lg hover:bg-indigo-700 transition flex items-center gap-2">
                                        <i data-lucide="save" class="w-[18px] h-[18px]"></i> 저장
                                    </button>
                                </div>
                                <div class="p-8 space-y-6">
                                    <input type="hidden" id="edit-id">
                                    <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                                        <div class="space-y-2">
                                            <label class="text-xs font-black text-slate-400 uppercase tracking-widest">학년</label>
                                            <select id="field-grade" class="w-full bg-slate-50 border border-slate-100 rounded-xl px-4 py-3 font-bold outline-none focus:ring-2 focus:ring-indigo-500 transition-all">
                                                <option value="1">1학년</option><option value="2">2학년</option><option value="3">3학년</option>
                                            </select>
                                        </div>
                                        <div class="md:col-span-2 space-y-2">
                                            <label class="text-xs font-black text-slate-400 uppercase tracking-widest">항목 유형</label>
                                            <select id="field-type" class="w-full bg-slate-50 border border-slate-100 rounded-xl px-4 py-3 font-bold outline-none focus:ring-2 focus:ring-indigo-500 transition-all">
                                                <option value="subject_spec">교과 세특 (1500B)</option>
                                                <option value="autonomous">자율 활동 (1500B)</option>
                                                <option value="club">동아리 활동 (1500B)</option>
                                                <option value="career">진로 활동 (2100B)</option>
                                                <option value="book_record">독서 기록 (1000B)</option>
                                                <option value="behavior_spec">행동 특성 종합의견 (1500B)</option>
                                            </select>
                                        </div>
                                    </div>
                                    <div class="space-y-2">
                                        <label class="text-xs font-black text-slate-400 uppercase tracking-widest">과목 / 활동 제목</label>
                                        <input type="text" id="field-title" placeholder="어떤 활동인가요?" class="w-full bg-slate-50 border border-slate-100 rounded-xl px-5 py-4 font-black text-lg outline-none focus:ring-2 focus:ring-indigo-500 transition-all">
                                    </div>
                                    <div class="space-y-2">
                                        <div class="flex justify-between items-end mb-1">
                                            <label class="text-xs font-black text-slate-400 uppercase tracking-widest">내용 기술</label>
                                            <span id="byte-counter" class="text-[10px] font-bold px-3 py-1 rounded-full bg-indigo-50 text-indigo-600">0 BYTE</span>
                                        </div>
                                        <textarea id="field-content" oninput="updateByteCount()" placeholder="동기, 과정, 결과 및 소감을 구체적으로 적으세요." class="w-full h-[350px] bg-slate-50 border border-slate-100 rounded-[2rem] px-6 py-5 outline-none focus:ring-2 focus:ring-indigo-500 transition-all resize-none leading-relaxed text-slate-700"></textarea>
                                    </div>
                                </div>
                            </div>
                        </div>
                        <div class="space-y-6 no-print">
                            <div class="bg-slate-900 rounded-[2rem] p-8 text-white shadow-xl">
                                <h4 class="font-black text-lg mb-6 flex items-center gap-2"><i data-lucide="alert-circle" class="text-indigo-400 w-5 h-5"></i> 작성 가이드</h4>
                                <ul class="space-y-4 text-sm text-slate-400">
                                    <li class="border-l-2 border-indigo-500 pl-4"><b class="text-indigo-300">구체적 사례</b>: 활동 내역을 나열하기보다 구체적인 에피소드를 적으세요.</li>
                                    <li class="border-l-2 border-emerald-500 pl-4"><b class="text-emerald-300">성장 중심</b>: 활동을 통해 무엇이 변화되었는지 강조하세요.</li>
                                </ul>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Preview Tab -->
                <div id="tab-preview" class="tab-content hidden">
                    <div class="w-full max-w-4xl mx-auto">
                        <div class="flex justify-end mb-6 no-print">
                            <button onclick="window.print()" class="bg-slate-800 text-white px-6 py-2.5 rounded-xl font-bold flex items-center gap-2 hover:bg-black transition">
                                <i data-lucide="download" class="w-[18px] h-[18px]"></i> PDF 출력
                            </button>
                        </div>
                        <div class="bg-white shadow-2xl border border-slate-200 min-h-[1000px] p-16 font-serif print:shadow-none print:p-0 print:border-none">
                            <h1 class="text-center text-4xl font-bold tracking-[0.5em] mb-16 border-b-4 border-black inline-block mx-auto w-full pb-4">학교생활기록부</h1>
                            <div class="grid grid-cols-4 border-2 border-black text-sm mb-12">
                                <div class="bg-slate-50 p-3 border-r border-b border-black font-bold text-center">성 명</div>
                                <div id="preview-name" class="p-3 border-r border-b border-black text-center"></div>
                                <div class="bg-slate-50 p-3 border-r border-b border-black font-bold text-center">학 년</div>
                                <div class="p-3 border-b border-black text-center">제 3학년</div>
                            </div>
                            <div id="preview-content-area" class="space-y-12">
                                <!-- Grouped records go here -->
                            </div>
                        </div>
                    </div>
                </div>
            </main>
        </div>
    </div>

    <script>
        let currentUser = null;
        let records = [];

        const byteLimits = {
            subject_spec: 1500, autonomous: 1500, club: 1500, career: 2100, book_record: 1000, behavior_spec: 1500
        };

        const typeLabels = {
            subject_spec: '교과 세특', autonomous: '자율 활동', club: '동아리 활동', career: '진로 활동', book_record: '독서 기록', behavior_spec: '행동 종합의견'
        };

        // Initialize Lucide Icons
        window.onload = () => {
            const savedUser = localStorage.getItem('srms_user');
            if (savedUser) {
                login(savedUser);
            }
            lucide.createIcons();
        };

        function handleLogin() {
            const name = document.getElementById('login-name').value.trim();
            if (!name) return alert('이름을 입력해주세요.');
            localStorage.setItem('srms_user', name);
            login(name);
        }

        function login(name) {
            currentUser = name;
            document.getElementById('login-screen').classList.add('hidden');
            document.getElementById('main-content').classList.remove('hidden');
            document.getElementById('user-display-name').innerText = name;
            document.getElementById('user-avatar').innerText = name.charAt(0);
            loadRecords();
            lucide.createIcons();
        }

        function handleLogout() {
            localStorage.removeItem('srms_user');
            location.reload();
        }

        function loadRecords() {
            const saved = localStorage.getItem(`records_${currentUser}`);
            records = saved ? JSON.parse(saved) : [];
            renderDashboard();
        }

        function saveRecord() {
            const id = document.getElementById('edit-id').value || Date.now();
            const record = {
                id: parseInt(id),
                grade: document.getElementById('field-grade').value,
                type: document.getElementById('field-type').value,
                title: document.getElementById('field-title').value,
                content: document.getElementById('field-content').value,
                date: new Date().toLocaleDateString()
            };

            if (!record.content) return alert('내용을 입력하세요.');

            const index = records.findIndex(r => r.id === record.id);
            if (index > -1) records[index] = record;
            else records.unshift(record);

            localStorage.setItem(`records_${currentUser}`, JSON.stringify(records));
            alert('저장되었습니다.');
            switchTab('dashboard');
            renderDashboard();
        }

        function deleteRecord(id) {
            if (!confirm('정말 삭제하시겠습니까?')) return;
            records = records.filter(r => r.id !== id);
            localStorage.setItem(`records_${currentUser}`, JSON.stringify(records));
            renderDashboard();
        }

        function openEditor(id = null) {
            switchTab('editor');
            const fieldId = document.getElementById('edit-id');
            const fieldGrade = document.getElementById('field-grade');
            const fieldType = document.getElementById('field-type');
            const fieldTitle = document.getElementById('field-title');
            const fieldContent = document.getElementById('field-content');

            if (id) {
                const r = records.find(record => record.id === id);
                fieldId.value = r.id;
                fieldGrade.value = r.grade;
                fieldType.value = r.type;
                fieldTitle.value = r.title;
                fieldContent.value = r.content;
            } else {
                fieldId.value = "";
                fieldTitle.value = "";
                fieldContent.value = "";
            }
            updateByteCount();
        }

        function renderDashboard() {
            const list = document.getElementById('records-list');
            const countStat = document.getElementById('stat-count');
            const byteStat = document.getElementById('stat-bytes');
            
            let totalBytes = 0;
            countStat.innerText = records.length;

            if (records.length === 0) {
                list.innerHTML = `<div class="p-20 text-center text-slate-400">기록이 없습니다.</div>`;
                byteStat.innerText = 0;
                return;
            }

            list.innerHTML = records.map(r => {
                const b = getByteLen(r.content);
                totalBytes += b;
                return `
                    <div class="p-6 hover:bg-slate-50 transition flex justify-between items-center">
                        <div class="flex gap-4 items-center">
                            <div class="w-12 h-12 rounded-2xl bg-slate-100 flex items-center justify-center text-slate-500">
                                <i data-lucide="file-text" class="w-5 h-5"></i>
                            </div>
                            <div>
                                <p class="text-xs font-black text-indigo-600 uppercase mb-0.5">${typeLabels[r.type]}</p>
                                <h4 class="font-bold text-slate-800">${r.title || '제목 없음'}</h4>
                                <p class="text-xs text-slate-400">${r.date}</p>
                            </div>
                        </div>
                        <div class="flex items-center gap-4">
                            <span class="text-xs font-bold text-slate-400 bg-slate-100 px-3 py-1.5 rounded-full">${b} Byte</span>
                            <div class="flex gap-2">
                                <button onclick="openEditor(${r.id})" class="p-2 text-slate-400 hover:text-indigo-600"><i data-lucide="pencil-line" class="w-4 h-4"></i></button>
                                <button onclick="deleteRecord(${r.id})" class="p-2 text-slate-400 hover:text-red-500"><i data-lucide="trash-2" class="w-4 h-4"></i></button>
                            </div>
                        </div>
                    </div>
                `;
            }).join('');
            
            byteStat.innerText = totalBytes.toLocaleString();
            lucide.createIcons();
        }

        function renderPreview() {
            document.getElementById('preview-name').innerText = currentUser;
            const area = document.getElementById('preview-content-area');
            
            if (records.length === 0) {
                area.innerHTML = `<p class="text-center text-slate-300">표시할 데이터가 없습니다.</p>`;
                return;
            }

            area.innerHTML = Object.keys(typeLabels).map(type => {
                const rList = records.filter(r => r.type === type);
                if (rList.length === 0) return '';
                return `
                    <section>
                        <h3 class="font-bold text-xl mb-4">▣ ${typeLabels[type]}</h3>
                        <div class="border border-black p-6 space-y-4 text-sm leading-8 min-h-[100px]">
                            ${rList.map(r => `<p><b>(${r.title})</b> ${r.content}</p>`).join('')}
                        </div>
                    </section>
                `;
            }).join('');
        }

        function switchTab(tabId) {
            document.querySelectorAll('.tab-content').forEach(t => t.classList.add('hidden'));
            document.getElementById(`tab-${tabId}`).classList.remove('hidden');
            
            document.querySelectorAll('.nav-btn').forEach(b => {
                b.classList.remove('bg-indigo-50', 'text-indigo-600');
                b.classList.add('text-slate-500', 'hover:bg-slate-50');
            });
            document.getElementById(`btn-${tabId}`).classList.add('bg-indigo-50', 'text-indigo-600');

            if (tabId === 'preview') renderPreview();
            if (tabId === 'dashboard') renderDashboard();
        }

        function getByteLen(str) {
            let b = 0;
            for (let i = 0; i < str.length; i++) {
                const code = str.charCodeAt(i);
                b += (code > 128) ? 3 : 1;
            }
            return b;
        }

        function updateByteCount() {
            const content = document.getElementById('field-content').value;
            const type = document.getElementById('field-type').value;
            const len = getByteLen(content);
            const counter = document.getElementById('byte-counter');
            counter.innerText = `${len} / ${byteLimits[type]} BYTE`;
            if (len > byteLimits[type]) {
                counter.classList.replace('bg-indigo-50', 'bg-red-50');
                counter.classList.replace('text-indigo-600', 'text-red-600');
            } else {
                counter.classList.replace('bg-red-50', 'bg-indigo-50');
                counter.classList.replace('text-red-600', 'text-indigo-600');
            }
        }
    </script>
</body>
</html>
