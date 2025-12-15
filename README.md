<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Shanghai Travel Deck</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        /* Hide scrollbar but allow scrolling */
        .no-scrollbar::-webkit-scrollbar { display: none; }
        .no-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }
        
        /* Smooth transitions */
        .tab-btn { transition: all 0.2s ease; }
        .card-hover:hover { transform: scale(1.01); }
        
        /* Text selection for copying */
        .select-all-text { user-select: all; -webkit-user-select: all; }
    </style>
</head>
<body class="bg-[#F2F0E6] font-sans text-slate-800 antialiased p-4">

    <div class="max-w-3xl mx-auto">
        
        <!-- Header -->
        <div class="mb-6 text-center">
            <h1 class="text-3xl md:text-5xl font-black text-[#8B4513] uppercase tracking-tight mb-2">
                Shanghai <span class="text-[#D97706]">Logistics</span>
            </h1>
            <p class="text-xs font-bold tracking-widest text-[#5C4033] uppercase">Master Transport Deck</p>
        </div>

        <!-- Navigation Tabs -->
        <div class="flex overflow-x-auto gap-2 pb-4 mb-4 no-scrollbar" id="tabs-container">
            <!-- Tabs injected by JS -->
        </div>

        <!-- Content Area -->
        <div id="content-area" class="space-y-6 pb-12">
            <!-- Content injected by JS -->
        </div>

    </div>

    <!-- Data & Logic -->
    <script>
        // --- 1. DATA ---
        const activeTabId = 'guide';
        
        const days = [
            { id: 'guide', label: "📘 GUIDE", date: "READ ME", title: "How to Use" },
            { id: 0, label: "Day 0", date: "Dec 21", title: "Arrival" },
            { id: 1, label: "Day 1", date: "Dec 22", title: "French Concession" },
            { id: 2, label: "Day 2", date: "Dec 23", title: "Disneyland" },
            { id: 3, label: "Day 3", date: "Dec 24", title: "Nanjing Rd" },
            { id: 4, label: "Day 4", date: "Dec 25", title: "Christmas" },
            { id: 5, label: "Day 5", date: "Dec 26", title: "Futuristic" },
            { id: 6, label: "Day 6", date: "Dec 27", title: "Water Town" },
            { id: 7, label: "Day 7", date: "Dec 28", title: "Safari" },
            { id: 8, label: "Day 8", date: "Dec 29", title: "Hangzhou" },
            { id: 9, label: "Day 9", date: "Dec 30", title: "Imperial" },
            { id: 10, label: "Day 10", date: "Dec 31", title: "Departure" },
        ];

        const dayData = {
            0: [
                { type: 'van', time: '22:30', title: 'AIRPORT PICKUP', cn: '虹口名苑 (靠近西藏南路地铁站)', en: 'Hongkou Mingyuan', addr: '黄浦区, 西藏南路1501弄', wait: 'Arrivals Hall Exit (Level 1)', timeEst: '50m', price: 'Pre-booked', note: 'Look for driver with Name Sign at the barrier (Post-Baggage).' }
            ],
            1: [
                { type: 'didi', time: '09:30', title: 'START', cn: '田子坊 (泰康路入口)', en: 'Tianzifang (Gate 1)', addr: '黄浦区泰康路210弄', wait: 'Curbside Airbnb', timeEst: '15m', price: '~¥25', note: 'Enter via Gate 1.' },
                { type: 'didi', time: '11:00', title: 'LUNCH', cn: '喜粵8号 (汝南街店)', en: 'Canton 8', addr: '汝南街63号 (近局门路)', wait: 'Tianzifang Gate 1', timeEst: '15m', price: '~¥20' },
                { type: 'didi', time: '13:00', title: 'SHOPPING', cn: 'Sunflour (安福路店)', en: 'Sunflour Bakery', addr: '安福路322号', wait: 'Outside Canton 8', timeEst: '20m', price: '~¥35', note: 'Base camp for seniors.' },
                { type: 'didi', time: '18:00', title: 'DINNER', cn: '蟹三宝 (南汇路店)', en: 'Xie San Bao', addr: '静安区南汇路74号', wait: 'Curbside Sunflour', timeEst: '20m', price: '~¥25', note: 'Very congested street. Group ready at curb.' },
                { type: 'didi', time: '20:00', title: 'HOME', cn: '虹口名苑', en: 'Hongkou Mingyuan', addr: '西藏南路1501弄', wait: 'Outside Xie San Bao', timeEst: '25m', price: '~¥30' }
            ],
            2: [
                { type: 'metro', time: '07:30', title: 'TO DISNEY', line: 'Line 11', dir: 'Disney Resort', exit: 'Exit 1', timeEst: '50m', price: '~¥6', note: 'Transfer at Oriental Sports Center.' },
                { type: 'didi', time: '21:15', title: 'RETURN', cn: '虹口名苑', en: 'Hongkou Mingyuan', addr: '西藏南路1501弄', wait: 'West Public Transportation Hub', timeEst: '45m', price: '~¥150', note: 'Official Pickup Zone. Follow signs for Taxi/Ride Hailing to the WEST.' }
            ],
            3: [
                { type: 'metro', time: '09:15', title: 'TO NANJING RD', line: 'Line 8', dir: 'Shiguang Rd', exit: 'Exit 19', timeEst: '15m', price: '~¥3', note: 'Exit 19 is inside New World City (Basement).' },
                { type: 'didi', time: '13:45', title: 'TO YU GARDEN', cn: '豫园商城2号门', en: 'Yu Garden Gate 2', addr: '福佑路 (Fuyou Road)', wait: 'Huanghe Road', timeEst: '20m', price: '~¥30', note: 'Call from Huanghe Rd (less crowded).' },
                { type: 'didi', time: '19:30', title: 'HOME', cn: '虹口名苑', en: 'Hongkou Mingyuan', addr: '西藏南路1501弄', wait: 'Fuyou Road (Gate 2)', timeEst: '20m', price: '~¥25' }
            ],
            4: [
                { type: 'didi', time: '09:30', title: 'COFFEE', cn: '星巴克臻选烘焙工坊', en: 'Starbucks Reserve', addr: '南京西路789号', wait: 'Airbnb', timeEst: '20m', price: '~¥25' },
                { type: 'didi', time: '12:00', title: 'LUNCH', cn: '全聚德 (淮海中路店)', en: 'Quanjude Duck', addr: '淮海中路780号4楼', wait: 'Side Door: Shimen 1st Rd', timeEst: '15m', price: '~¥20', note: 'Exit Starbucks via SIDE door.' },
                { type: 'didi', time: '14:00', title: 'PHOTO', cn: '北外滩滨江绿地', en: 'North Bund', addr: '东大名路500号 (Opposite)', wait: 'Quanjude Lobby', timeEst: '20m', price: '~¥30', note: 'Silver Drop Sculpture.' },
                { type: 'didi', time: '15:30', title: 'MARKET', cn: '圆明园路 (近北京东路)', en: 'Rockbund', addr: 'Waitan Area', wait: 'North Bund Roadside', timeEst: '10m', price: '~¥15' },
                { type: 'walk', time: '17:30', title: 'DINNER', cn: '海底捞火锅 (外滩店)', en: 'Haidilao', addr: '南京东路123号', wait: 'Walk', timeEst: '8m', price: '0', note: 'Walk South on Yuanmingyuan Rd.' },
                { type: 'didi', time: '19:30', title: 'CRUISE', cn: '十六铺码头', en: 'Shiliupu Pier', addr: '中山东二路551号', wait: 'Bund Central Mall', timeEst: '10m', price: '~¥15' },
                { type: 'didi', time: '21:30', title: 'HOME', cn: '虹口名苑', en: 'Hongkou Mingyuan', addr: '西藏南路1501弄', wait: 'BFC Mall North Drop-off', timeEst: '20m', price: '~¥25', note: 'Walk 5 mins South to BFC. Do NOT wait at Pier curb.' }
            ],
            5: [
                { type: 'didi', time: '09:00', title: '1000 TREES', cn: '昌化路桥 (莫干山路口)', en: 'Changhua Bridge Drop', addr: 'Moganshan Rd', wait: 'Airbnb', timeEst: '25m', price: '~¥35', note: 'Best view from the bridge.' },
                { type: 'didi', time: '10:30', title: 'LUNCH', cn: '3号仓库 (新世界城店)', en: 'No. 3 Warehouse', addr: '南京西路2-68号4楼', wait: 'West Gate 1000 Trees', timeEst: '15m', price: '~¥25' },
                { type: 'metro', time: '13:00', title: 'SKYWALK', line: 'Line 2', dir: 'Pudong Airport', exit: 'Exit 2', timeEst: '15m', price: '~¥3', note: 'Exit 2 leads to Bridge Escalator.' },
                { type: 'didi', time: '14:30', title: 'GUNDAM', cn: '啦啦宝都 (新金桥路738号)', en: 'LaLaport', addr: 'Pudong New Area', wait: 'Grand Hyatt Lobby', timeEst: '25m', price: '~¥40' },
                { type: 'didi', time: '17:00', title: 'DINNER', cn: '费大厨辣椒炒肉 (悦荟广场店)', en: 'Fei Da Chu', addr: '南京东路353号', wait: 'LaLaport Entrance', timeEst: '30m', price: '~¥35' },
                { type: 'didi', time: '21:30', title: 'HOME', cn: '虹口名苑', en: 'Hongkou Mingyuan', addr: '西藏南路1501弄', wait: 'Nanjing Road Accessible Point', timeEst: '20m', price: '~¥30' }
            ],
            6: [
                { type: 'metro', time: '08:00', title: 'MEETING PT', line: 'Line 8', dir: 'Shiguang Rd', exit: 'See Ticket', timeEst: '15m', price: '~¥3', note: 'To People\'s Square.' },
                { type: 'bus', time: '08:30', title: 'TOUR BUS', cn: 'Bus Tour', en: 'Film Park & Water Town', addr: 'People\'s Square', wait: 'Designated Spot', timeEst: '50m', price: 'Included' },
                { type: 'walk', time: '18:30', title: 'DINNER', cn: '很久以前羊肉串', en: 'Henjiu Yiqian', addr: '云南南路180号', wait: 'Walk', timeEst: '10m', price: '0' },
                { type: 'didi', time: '20:30', title: 'HOME', cn: '虹口名苑', en: 'Hongkou Mingyuan', addr: '西藏南路1501弄', wait: 'Restaurant', timeEst: '15m', price: '~¥20' }
            ],
            7: [
                { type: 'didi', time: '08:00', title: 'SAFARI', cn: '上海野生动物园', en: 'Wild Animal Park', addr: 'Pudong', wait: 'Airbnb', timeEst: '1.5h', price: '~¥120', note: 'Long ride. Go to bathroom first.' },
                { type: 'didi', time: '16:30', title: 'HOME', cn: '虹口名苑', en: 'Hongkou Mingyuan', addr: '西藏南路1501弄', wait: 'Main Parking Lot (Taxi Zone)', timeEst: '1.5h', price: '~¥120', note: 'Book early.' }
            ],
            8: [
                { type: 'didi', time: '09:00', title: 'STATION', cn: '上海虹桥火车站 (出发层)', en: 'Hongqiao Station', addr: 'Departures Level', wait: 'Airbnb', timeEst: '45m', price: '~¥70', note: 'Traffic is heavy. Aim to arrive at station by 09:40 latest.' },
                { type: 'train', time: '10:00', title: 'TRAIN G239', cn: '杭州东站', en: 'Hangzhou East', addr: 'G Train', wait: 'Station Gates', timeEst: '45m', price: 'Booked', note: 'PASSPORT IS YOUR TICKET. Board directly with ID used for booking. Use Staff Lane if auto-gate fails.' },
                { type: 'didi', time: '11:00', title: 'LAKE', cn: '青山湖水上森林', en: 'Qingshan Lake', addr: 'Lin\'an', wait: 'Hangzhou East Taxi Stand', timeEst: '1h 15m', price: '~¥100', note: 'Long ride. Use restroom at station first.' },
                { type: 'didi', time: '15:15', title: 'RETURN RIDE', cn: '杭州东站 (出发层)', en: 'Hangzhou East Station', addr: 'Departures', wait: 'Lake Entrance', timeEst: '1.5h', price: '~¥100', note: 'Book at 15:15 sharp. Traffic can be bad. Aim to arrive station by 17:00.' },
                { type: 'train', time: '17:40', title: 'TRAIN G7590', cn: '上海虹桥站', en: 'Shanghai Hongqiao', addr: 'Shanghai', wait: 'Ticket Gate', timeEst: '59m', price: 'Booked', note: 'Board with Passport. No paper ticket needed.' },
                { type: 'didi', time: '19:00', title: 'HOME', cn: '虹口名苑', en: 'Hongkou Mingyuan', addr: '西藏南路1501弄', wait: 'P10 Parking Garage', timeEst: '45m', price: '~¥70', note: 'Follow signs for P10 or Ride Hailing (网约车) from Arrivals.' }
            ],
            9: [
                { type: 'metro', time: '08:30', title: 'LUNCH', line: 'Line 8 -> 9', dir: 'Songjiang', exit: 'Exit 2', timeEst: '1h', price: '~¥6', note: 'To Songjiang University Town.' },
                { type: 'didi', time: '14:30', title: 'PHOTO', cn: '宋庆龄故居 (淮海中路1843号)', en: 'Wukang Mansion', addr: 'Opposite Side', wait: 'Restaurant', timeEst: '1h', price: '~¥100', note: 'Nap time.' },
                { type: 'didi', time: '16:00', title: 'SHOPPING', cn: '新天地 (马当路兴业路路口)', en: 'Xintiandi', addr: 'Madang Rd', wait: 'Near Wukang Mansion', timeEst: '20m', price: '~¥25' },
                { type: 'didi', time: '20:30', title: 'HOME', cn: '虹口名苑', en: 'Hongkou Mingyuan', addr: '西藏南路1501弄', wait: 'Fuyou Road', timeEst: '20m', price: '~¥20' }
            ],
            10: [
                { type: 'didi', time: '11:30', title: 'LUGGAGE', cn: '龙阳路地铁站', en: 'Longyang Rd Station', addr: 'Maglev', wait: 'Airbnb', timeEst: '20m', price: '~¥30' },
                { type: 'didi', time: '14:30', title: 'LAST STOP', cn: 'EKA·天物', en: 'EKA Tianwu', addr: '浦东新区金桥路535号', wait: 'Longyang Rd', timeEst: '25m', price: '~¥30' },
                { type: 'didi', time: '19:00', title: 'RETRIEVE', cn: '龙阳路地铁站', en: 'Longyang Rd Station', addr: 'Maglev', wait: 'EKA Entrance', timeEst: '25m', price: '~¥30' },
                { type: 'train', time: '19:30', title: 'AIRPORT', cn: '浦东机场 T1', en: 'PVG Airport T1', addr: 'Maglev', wait: 'Platform', timeEst: '8m', price: '¥50', note: 'Maglev Train.' }
            ]
        };

        // --- 2. RENDERING ---
        
        function renderTabs(activeId) {
            const container = document.getElementById('tabs-container');
            container.innerHTML = days.map(day => {
                const isActive = day.id == activeId;
                const bg = isActive ? 'bg-[#D97706] text-white border-[#D97706] shadow-md scale-105' : 'bg-white text-[#8B4513] border-transparent hover:border-[#D97706]/30';
                
                return `
                    <button 
                        onclick="switchTab('${day.id}')"
                        class="tab-btn flex-shrink-0 px-4 py-2 rounded-xl text-xs font-bold border-2 ${bg}">
                        <div class="opacity-80 uppercase text-[10px] mb-0.5">${day.date}</div>
                        <div>${day.label}</div>
                    </button>
                `;
            }).join('');
        }

        function renderContent(activeId) {
            const container = document.getElementById('content-area');
            
            if (activeId === 'guide') {
                container.innerHTML = renderGuide();
            } else {
                const items = dayData[activeId] || [];
                container.innerHTML = items.map(item => {
                    if (item.type === 'metro') return getMetroCard(item);
                    return getTransportCard(item);
                }).join('');
            }
        }

        // --- 3. TEMPLATES ---

        function renderGuide() {
            return `
                <div class="bg-white rounded-2xl p-6 shadow-lg border-2 border-[#D97706]/20">
                    <h2 class="text-2xl font-black text-[#5C4033] mb-6 flex items-center gap-2">
                        📱 Essential Apps & How to Use
                    </h2>
                    <div class="space-y-4">
                        ${guideAppCard('bg-[#1677FF]', '支', 'Alipay (Zhifubao)', 'PAY • METRO • TAXI', [
                            '<strong>Setup:</strong> Link your foreign Visa/Mastercard. Works for 99% of vendors.',
                            '<strong>Metro:</strong> Click "Transport" ➔ "Metro". Scan QR at turnstile.',
                            '<strong>Ride Hailing:</strong> Click "Transport" ➔ "Taxi". Paste Chinese addresses from this deck.'
                        ])}
                        ${guideAppCard('bg-white border-2 border-blue-400 text-blue-500', '📍', 'Amap (Gaode Ditu)', 'MAPS', [
                            '<strong>Why:</strong> Google Maps is blocked. Amap is accurate.',
                            '<strong>Nav:</strong> Copy addresses from deck ➔ Paste ➔ Follow blue arrow.',
                            '<strong>3D:</strong> Shows 3D buildings to find entrances easily.'
                        ])}
                        ${guideAppCard('bg-[#07C160]', '💬', 'WeChat (Weixin)', 'CHAT • ORDERING', [
                            '<strong>Dining:</strong> Scan table QR codes to order food (digital menu).',
                            '<strong>Translate:</strong> Long-press Chinese messages to translate to English.'
                        ])}
                        ${guideAppCard('bg-[#FF6600]', '🍽️', 'Dianping', 'REVIEWS • QUEUE', [
                            '<strong>Queue:</strong> Use "排队" feature to queue remotely.',
                            '<strong>Menu:</strong> Show food photos to waiters to order visually.'
                        ])}
                    </div>
                </div>
            `;
        }

        function guideAppCard(bgColor, icon, title, badge, points) {
            return `
                <div class="flex gap-4 items-start p-4 rounded-xl border border-gray-100 bg-gray-50">
                    <div class="w-12 h-12 rounded-xl shadow-md shrink-0 flex items-center justify-center text-xl font-bold text-white ${bgColor}">
                        ${icon}
                    </div>
                    <div class="flex-1">
                        <div class="flex justify-between items-center mb-1">
                            <h3 class="font-bold text-slate-800">${title}</h3>
                            <span class="text-[10px] font-bold bg-slate-200 text-slate-600 px-2 py-0.5 rounded uppercase tracking-wide">${badge}</span>
                        </div>
                        <ul class="text-sm space-y-1 text-slate-600">
                            ${points.map(p => `<li class="flex gap-2"><span class="opacity-50">•</span> <span>${p}</span></li>`).join('')}
                        </ul>
                    </div>
                </div>
            `;
        }

        function getTransportCard(item) {
            const { type, time, title, cn, en, addr, wait, timeEst, price, note } = item;
            
            // Icon selection
            let iconLabel = type === 'didi' ? 'DiDi / Taxi' : type.toUpperCase();
            let iconColor = type === 'didi' || type === 'van' ? 'bg-[#D97706]' : (type === 'train' || type === 'bus' ? 'bg-blue-500' : 'bg-slate-500');

            return `
                <div class="bg-[#3E3E3E] text-white rounded-xl overflow-hidden shadow-lg border border-gray-700 card-hover transition-transform">
                    <!-- Clickable Header -->
                    <div onclick="copyText('${cn} ${addr}')" class="bg-[#2A2A2A] p-4 relative cursor-pointer hover:bg-[#333] transition-colors border-b border-gray-600 group">
                        
                        <div class="flex justify-between items-center mb-3">
                            <div class="${iconColor} text-white text-xs font-black px-3 py-1 rounded-full uppercase flex items-center gap-1">
                                ${iconLabel}
                            </div>
                            <div class="text-right">
                                <div class="text-xs font-mono text-gray-300 flex items-center justify-end gap-1">🕒 ${timeEst}</div>
                                <div class="text-[#D97706] font-bold text-sm bg-[#3E3E3E] px-2 py-0.5 rounded mt-1 inline-block border border-[#D97706]/30">${price}</div>
                            </div>
                        </div>

                        <div class="flex items-start justify-between gap-4">
                            <div>
                                <h3 class="text-2xl font-bold text-[#D97706] leading-tight mb-1 select-all-text">${cn}</h3>
                                <div class="text-white font-bold text-sm">${en}</div>
                                ${addr ? `<div class="text-gray-400 text-xs mt-1 font-mono select-all-text">📍 ${addr}</div>` : ''}
                            </div>
                            <div class="shrink-0 text-gray-500 group-hover:text-white transition-colors text-xs font-bold uppercase tracking-wider mt-2">
                                Click to Copy 📋
                            </div>
                        </div>
                    </div>

                    <!-- Bottom Info -->
                    <div class="p-4 bg-[#3E3E3E]">
                        <div class="mb-2">
                            <div class="text-[10px] text-gray-400 uppercase font-bold tracking-wider mb-1">Time & Wait Point</div>
                            <div class="font-bold text-lg">${time} <span class="text-gray-400 font-normal text-sm">@ ${wait}</span></div>
                        </div>
                        ${note ? `
                        <div class="bg-[#2A2A2A] p-2 rounded text-xs text-gray-300 flex items-start gap-2 border-l-2 border-[#D97706]">
                            <span>ℹ️</span> ${note}
                        </div>` : ''}
                    </div>
                </div>
            `;
        }

        function getMetroCard(item) {
            const { time, title, line, dir, exit, timeEst, price, note } = item;
            return `
                <div class="bg-white rounded-xl overflow-hidden shadow-sm border-l-8 border-[#D97706] p-4 flex flex-col gap-3">
                    <div class="flex justify-between items-center">
                        <div class="flex items-center gap-2 text-[#8B4513]">
                            <span class="text-xl">🚇</span>
                            <span class="font-black text-lg">METRO: ${title}</span>
                        </div>
                        <div class="text-right">
                            <div class="text-xs bg-gray-100 px-2 py-1 rounded text-gray-600 font-mono mb-1">${time} • ${timeEst}</div>
                            <div class="text-[#D97706] font-bold text-xs bg-orange-50 px-2 py-0.5 rounded border border-orange-100">${price}</div>
                        </div>
                    </div>

                    <div class="grid grid-cols-2 gap-4 text-sm">
                        <div>
                            <div class="text-xs text-gray-400 uppercase font-bold">Line & Direction</div>
                            <div class="font-bold text-gray-800">${line} <span class="font-normal text-gray-500">to ${dir}</span></div>
                        </div>
                        <div>
                            <div class="text-xs text-gray-400 uppercase font-bold">Target Exit</div>
                            <div class="font-bold text-[#D97706] text-lg flex items-center gap-1">📍 ${exit}</div>
                        </div>
                    </div>
                    ${note ? `<div class="text-xs text-gray-500 italic border-t border-gray-100 pt-2 mt-1">💡 ${note}</div>` : ''}
                </div>
            `;
        }

        // --- 4. ACTIONS ---

        window.switchTab = function(id) {
            // Update UI
            renderTabs(id);
            renderContent(id);
        };

        window.copyText = function(text) {
            if (!text) return;
            // Create hidden text area to copy from
            const textarea = document.createElement('textarea');
            textarea.value = text;
            document.body.appendChild(textarea);
            textarea.select();
            try {
                document.execCommand('copy');
                alert('Copied address: ' + text);
            } catch (err) {
                console.error('Failed to copy', err);
            }
            document.body.removeChild(textarea);
        };

        // Initialize
        switchTab('guide');

    </script>
</body>
</html>
