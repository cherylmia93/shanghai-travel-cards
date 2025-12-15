<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Shanghai Travel Deck</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        .no-scrollbar::-webkit-scrollbar { display: none; }
        .no-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }
        .tab-btn { transition: all 0.2s ease; }
        .card-hover:hover { transform: translateY(-2px); }
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

        /* DATA STRUCTURE UPDATE:
           searchStr: The exact string copied to clipboard for DiDi. 
           wait: Specific waiting instructions.
        */
        const dayData = {
            0: [
                { type: 'van', time: '22:30', title: 'AIRPORT PICKUP', cn: '虹口名苑', en: 'Hongkou Mingyuan', searchStr: '虹口名苑 西藏南路1501弄', addr: '西藏南路1501弄', wait: 'Arrivals Hall Barrier (Inside)', timeEst: '50m', price: 'Pre-booked', note: 'Driver will hold a name sign. Do not go outside.' }
            ],
            1: [
                { type: 'didi', time: '09:30', title: 'START', cn: '田子坊(1号门)', en: 'Tianzifang (Gate 1)', searchStr: '田子坊-1号门', addr: '泰康路210弄', wait: 'Lane 1501 Curbside', timeEst: '15m', price: '~¥25', note: 'Gate 1 is the main entrance on Taikang Rd.' },
                { type: 'didi', time: '11:00', title: 'LUNCH', cn: '喜粤8号(汝南街店)', en: 'Canton 8', searchStr: '喜粤8号(汝南街店) 汝南街63号', addr: '汝南街63号', wait: 'Tianzifang Gate 1', timeEst: '15m', price: '~¥20' },
                { type: 'didi', time: '13:00', title: 'SHOPPING', cn: 'Sunflour(安福路店)', en: 'Sunflour Bakery', searchStr: 'Sunflour(安福路店)', addr: '安福路322号', wait: 'Restaurant Curbside', timeEst: '20m', price: '~¥35', note: 'Seniors stay here. Young adults explore.' },
                { type: 'didi', time: '18:00', title: 'DINNER', cn: '蟹三宝(南京西路店)', en: 'Xie San Bao', searchStr: '蟹三宝(南京西路店)', addr: '南汇路74号', wait: 'Sunflour Curbside', timeEst: '20m', price: '~¥25', note: 'Address is Nanhui Rd, but listed as Nanjing West Rd Branch.' },
                { type: 'didi', time: '20:00', title: 'HOME', cn: '虹口名苑', en: 'Hongkou Mingyuan', searchStr: '虹口名苑 西藏南路1501弄', addr: '西藏南路1501弄', wait: 'Outside Restaurant', timeEst: '25m', price: '~¥30' }
            ],
            2: [
                { type: 'metro', time: '07:30', title: 'TO DISNEY', line: 'Line 11', dir: 'Disney Resort', exit: 'Exit 1', timeEst: '50m', price: '~¥6', note: 'Transfer at Oriental Sports Center.' },
                { type: 'didi', time: '21:15', title: 'RETURN', cn: '虹口名苑', en: 'Hongkou Mingyuan', searchStr: '虹口名苑 西藏南路1501弄', addr: '西藏南路1501弄', wait: 'West Public Transportation Hub', timeEst: '45m', price: '~¥150', note: 'Follow signs to "West PTH" / Taxi. Do not wait at main gate.' }
            ],
            3: [
                { type: 'metro', time: '09:15', title: 'TO NANJING RD', line: 'Line 8', dir: 'Shiguang Rd', exit: 'Exit 19', timeEst: '15m', price: '~¥3', note: 'Exit 19 leads into New World City Basement.' },
                { type: 'didi', time: '13:45', title: 'TO YU GARDEN', cn: '豫园商城-2号门', en: 'Yu Garden Gate 2', searchStr: '豫园商城-2号门', addr: '福佑路', wait: 'Huanghe Road', timeEst: '20m', price: '~¥30', note: 'Call from Huanghe Rd (quieter street).' },
                { type: 'didi', time: '19:30', title: 'HOME', cn: '虹口名苑', en: 'Hongkou Mingyuan', searchStr: '虹口名苑 西藏南路1501弄', addr: '西藏南路1501弄', wait: 'Fuyou Road (Gate 2)', timeEst: '20m', price: '~¥25' }
            ],
            4: [
                { type: 'didi', time: '09:30', title: 'COFFEE', cn: '星巴克臻选上海烘焙工坊', en: 'Starbucks Reserve', searchStr: '星巴克臻选上海烘焙工坊', addr: '南京西路789号', wait: 'Lane 1501 Curbside', timeEst: '20m', price: '~¥25' },
                { type: 'didi', time: '12:00', title: 'LUNCH', cn: '全聚德(淮海中路店)', en: 'Quanjude Duck', searchStr: '全聚德(淮海中路店)', addr: '淮海中路780号', wait: 'Shimen 1st Road (Side Door)', timeEst: '15m', price: '~¥20', note: 'Exit Starbucks via SIDE door to avoid traffic.' },
                { type: 'didi', time: '14:00', title: 'PHOTO', cn: '北外滩滨江绿地', en: 'North Bund', searchStr: '北外滩滨江绿地', addr: '东大名路500号对面', wait: 'Quanjude Lobby', timeEst: '20m', price: '~¥30' },
                { type: 'didi', time: '15:30', title: 'MARKET', cn: '圆明园路步行街', en: 'Rockbund', searchStr: '圆明园路步行街', addr: '北京东路', wait: 'North Bund Roadside', timeEst: '10m', price: '~¥15' },
                { type: 'walk', time: '17:30', title: 'DINNER', cn: '海底捞(外滩店)', en: 'Haidilao', addr: '南京东路123号', wait: 'Walk', timeEst: '8m', price: '0', note: 'Walk South on Yuanmingyuan Rd.' },
                { type: 'didi', time: '19:30', title: 'CRUISE', cn: '十六铺码头', en: 'Shiliupu Pier', searchStr: '十六铺码头', addr: '中山东二路551号', wait: 'Bund Central Mall', timeEst: '10m', price: '~¥15' },
                { type: 'didi', time: '21:30', title: 'HOME', cn: '虹口名苑', en: 'Hongkou Mingyuan', searchStr: '虹口名苑 西藏南路1501弄', addr: '西藏南路1501弄', wait: 'BFC North District Drop-off', timeEst: '20m', price: '~¥25', note: 'Walk to BFC Mall driveway. Do NOT wait at Pier curbside.' }
            ],
            5: [
                { type: 'didi', time: '09:00', title: '1000 TREES', cn: '昌化路桥', en: 'Changhua Bridge', searchStr: '昌化路桥', addr: '莫干山路口', wait: 'Lane 1501 Curbside', timeEst: '25m', price: '~¥35', note: 'Drop on the bridge for the best photo view.' },
                { type: 'didi', time: '10:30', title: 'LUNCH', cn: '3号仓库(新世界城店)', en: 'No. 3 Warehouse', searchStr: '3号仓库(新世界城店)', addr: '南京西路2-68号', wait: '1000 Trees West Gate', timeEst: '15m', price: '~¥25' },
                { type: 'metro', time: '13:00', title: 'SKYWALK', line: 'Line 2', dir: 'Pudong Airport', exit: 'Exit 2', timeEst: '15m', price: '~¥3', note: 'Exit 2 leads directly to Bridge Escalator.' },
                { type: 'didi', time: '14:30', title: 'GUNDAM', cn: '啦啦宝都(上海金桥店)', en: 'LaLaport', searchStr: '啦啦宝都(上海金桥店)', addr: '新金桥路738号', wait: 'Grand Hyatt Lobby', timeEst: '25m', price: '~¥40' },
                { type: 'didi', time: '17:00', title: 'TO METRO', cn: '台儿庄路(地铁站)', en: 'Tai\'erzhuang Rd', searchStr: '台儿庄路(地铁站)', addr: '9号线', wait: 'LaLaport Main Entrance', timeEst: '5m', price: '~¥15', note: 'Short ride to avoid rush hour traffic.' },
                { type: 'metro', time: '17:15', title: 'DIRECT TRAIN', line: 'Line 9', dir: 'Songjiang', exit: 'Jiashan Rd Exit 2', timeEst: '40m', price: '~¥5', note: 'Relaxing 40 min ride. No transfers.' },
                { type: 'walk', time: '18:00', title: 'DINNER', cn: '人和馆(肇嘉浜路店)', en: 'Ren He Guan', addr: '肇嘉浜路407号', wait: 'Exit 2', timeEst: '5m', price: '0', note: 'Walk 300m straight.' },
                { type: 'didi', time: '20:00', title: 'HOME', cn: '虹口名苑', en: 'Hongkou Mingyuan', searchStr: '虹口名苑 西藏南路1501弄', addr: '西藏南路1501弄', wait: 'Restaurant Curbside', timeEst: '15m', price: '~¥20' }
            ],
            6: [
                { type: 'metro', time: '08:00', title: 'MEETING PT', line: 'Line 8', dir: 'Shiguang Rd', exit: 'See Ticket', timeEst: '15m', price: '~¥3', note: 'Check bus confirmation for specific Exit.' },
                { type: 'bus', time: '08:30', title: 'TOUR BUS', cn: 'Bus Tour', en: 'Film Park', addr: 'People\'s Square', wait: 'Designated Spot', timeEst: '50m', price: 'Included' },
                { type: 'walk', time: '18:30', title: 'DINNER', cn: '很久以前羊肉串(云南南路店)', en: 'Henjiu Yiqian', addr: '云南南路180号', wait: 'Walk', timeEst: '10m', price: '0' },
                { type: 'didi', time: '20:30', title: 'HOME', cn: '虹口名苑', en: 'Hongkou Mingyuan', searchStr: '虹口名苑 西藏南路1501弄', addr: '西藏南路1501弄', wait: 'Restaurant Curbside', timeEst: '15m', price: '~¥20' }
            ],
            7: [
                { type: 'didi', time: '08:00', title: 'SAFARI', cn: '上海野生动物园', en: 'Wild Animal Park', searchStr: '上海野生动物园', addr: '浦东新区', wait: 'Lane 1501 Curbside', timeEst: '1.5h', price: '~¥120', note: 'Long ride.' },
                { type: 'didi', time: '16:30', title: 'HOME', cn: '虹口名苑', en: 'Hongkou Mingyuan', searchStr: '虹口名苑 西藏南路1501弄', addr: '西藏南路1501弄', wait: 'Main Parking Lot (Taxi Zone)', timeEst: '1.5h', price: '~¥120', note: 'Go to designated Taxi Zone.' }
            ],
            8: [
                { type: 'didi', time: '09:00', title: 'STATION', cn: '上海虹桥站-出发层', en: 'Hongqiao Station', searchStr: '上海虹桥站-出发层', addr: 'South/North Drop-off', wait: 'Lane 1501 Curbside', timeEst: '45m', price: '~¥70', note: 'Go to "Departures" level.' },
                { type: 'train', time: '10:00', title: 'TRAIN G239', cn: '杭州东站', en: 'Hangzhou East', addr: 'G Train', wait: 'Station Gates', timeEst: '45m', price: 'Booked', note: 'Scan Passport at Manual Lane.' },
                { type: 'didi', time: '11:00', title: 'LAKE', cn: '青山湖水上森林', en: 'Qingshan Lake', searchStr: '青山湖水上森林', addr: '临安区', wait: 'Hangzhou East P1 Parking', timeEst: '1.5h', price: '~¥100', note: 'Follow signs to "Online Car Hailing" (网约车).' },
                { type: 'didi', time: '15:15', title: 'RETURN RIDE', cn: '杭州东站-出发层', en: 'Hangzhou East Station', searchStr: '杭州东站-出发层', addr: 'Departures', wait: 'Lake Entrance', timeEst: '1.5h', price: '~¥100', note: 'Aim to arrive by 17:00.' },
                { type: 'train', time: '17:40', title: 'TRAIN G7590', cn: '上海虹桥站', en: 'Shanghai Hongqiao', addr: 'Shanghai', wait: 'Ticket Gate', timeEst: '59m', price: 'Booked' },
                { type: 'didi', time: '19:00', title: 'HOME', cn: '虹口名苑', en: 'Hongkou Mingyuan', searchStr: '虹口名苑 西藏南路1501弄', addr: '西藏南路1501弄', wait: 'P10 Parking Garage', timeEst: '45m', price: '~¥70', note: 'Follow signs to P10 / Ride Hailing.' }
            ],
            9: [
                { type: 'metro', time: '08:30', title: 'LUNCH', line: 'Line 8 -> 9', dir: 'Songjiang', exit: 'Exit 2', timeEst: '1h', price: '~¥6', note: 'To Songjiang University Town.' },
                { type: 'didi', time: '14:30', title: 'PHOTO', cn: '宋庆龄故居', en: 'Wukang Mansion', searchStr: '宋庆龄故居', addr: '淮海中路1843号', wait: 'Restaurant Curbside', timeEst: '1h', price: '~¥100', note: 'Drop here for best view of Mansion.' },
                { type: 'didi', time: '16:00', title: 'SHOPPING', cn: '新天地', en: 'Xintiandi', searchStr: '新天地', addr: '马当路', wait: 'Wukang Mansion', timeEst: '20m', price: '~¥25' },
                { type: 'didi', time: '20:30', title: 'HOME', cn: '虹口名苑', en: 'Hongkou Mingyuan', searchStr: '虹口名苑 西藏南路1501弄', addr: '西藏南路1501弄', wait: 'Fuyou Road', timeEst: '20m', price: '~¥20' }
            ],
            10: [
                { type: 'didi', time: '11:00', title: 'LUGGAGE', cn: '龙阳路(地铁站)', en: 'Longyang Rd Station', searchStr: '龙阳路(地铁站)', addr: '磁悬浮', wait: 'Lane 1501 Curbside', timeEst: '20m', price: '~¥30' },
                { type: 'didi', time: '11:30', title: 'LUNCH', cn: '费大厨辣椒炒肉(南京东路悦荟广场店)', en: 'Fei Da Chu (Mosaic Mall)', searchStr: '费大厨辣椒炒肉(南京东路悦荟广场店)', addr: '南京东路353号', wait: 'Longyang Rd', timeEst: '25m', price: '~¥30', note: 'Queue on Dianping!' },
                { type: 'walk', time: '13:30', title: 'STROLL', cn: '南京东路步行街', en: 'Nanjing Rd Stroll', addr: 'Walking Street', wait: 'Mosaic Mall', timeEst: '1h', price: '0', note: 'Shop & Explore.' },
                { type: 'didi', time: '15:00', title: 'LAST STOP', cn: 'EKA·天物', en: 'EKA Tianwu', searchStr: 'EKA·天物', addr: '金桥路535号', wait: 'Nanjing East Rd', timeEst: '30m', price: '~¥40' },
                { type: 'didi', time: '19:00', title: 'RETRIEVE', cn: '龙阳路(地铁站)', en: 'Longyang Rd Station', searchStr: '龙阳路(地铁站)', addr: '磁悬浮', wait: 'EKA Entrance', timeEst: '30m', price: '~¥30' },
                { type: 'train', time: '19:30', title: 'AIRPORT', cn: '浦东国际机场T1航站楼', en: 'PVG Airport T1', addr: 'Terminal 1', wait: 'Maglev Platform', timeEst: '8m', price: '¥50', note: 'Maglev Train.' }
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
            const { type, time, title, cn, en, searchStr, addr, wait, timeEst, price, note } = item;
            
            // Icon selection
            let iconLabel = type === 'didi' ? 'DiDi / Taxi' : type.toUpperCase();
            let iconColor = type === 'didi' || type === 'van' ? 'bg-[#D97706]' : (type === 'train' || type === 'bus' ? 'bg-blue-500' : 'bg-slate-500');
            
            // Use searchStr for copying if it exists, otherwise fallback to cn
            const textToCopy = searchStr || cn;

            return `
                <div class="bg-white rounded-xl overflow-hidden shadow-lg border border-gray-100 card-hover transition-transform">
                    
                    <!-- 1. Top Logistics Bar -->
                    <div class="bg-slate-50 p-3 flex justify-between items-center border-b border-gray-100">
                        <div class="flex items-center gap-2">
                            <span class="font-black text-xl text-[#8B4513]">${time}</span>
                            <div class="${iconColor} text-white text-[10px] font-bold px-2 py-0.5 rounded-full uppercase">
                                ${iconLabel}
                            </div>
                        </div>
                        <div class="text-right flex flex-col items-end">
                            <div class="text-xs font-bold text-slate-500 flex items-center gap-1">
                                ⏱ ${timeEst}
                            </div>
                            <div class="text-xs font-bold text-[#D97706] bg-orange-50 px-1.5 rounded mt-0.5">
                                ${price}
                            </div>
                        </div>
                    </div>

                    <!-- 2. Middle Context (English + Instructions) -->
                    <div class="p-4 pb-2">
                        <h3 class="font-bold text-lg text-slate-800 leading-tight mb-1">${title}: ${en}</h3>
                        <div class="text-sm text-slate-500 mb-2 flex items-start gap-1">
                            <span>📍</span> 
                            <span><strong>Wait:</strong> ${wait}</span>
                        </div>
                        ${note ? `<div class="text-xs bg-slate-50 text-slate-600 p-2 rounded border border-slate-100 italic">ℹ️ ${note}</div>` : ''}
                    </div>

                    <!-- 3. Bottom Action (Driver Card) -->
                    <div onclick="copyText('${textToCopy}')" class="bg-[#2A2A2A] p-4 cursor-pointer hover:bg-[#333] transition-colors relative group mt-2">
                        <div class="absolute top-3 right-3 text-[#D97706] text-[10px] font-bold uppercase tracking-wider opacity-70 group-hover:opacity-100">
                            Tap to Copy for DiDi
                        </div>
                        <div class="text-[10px] text-gray-400 uppercase font-bold tracking-widest mb-1">
                            Destination
                        </div>
                        <h2 class="text-2xl font-bold text-[#D97706] leading-tight mb-1 select-all-text">
                            ${cn}
                        </h2>
                        <div class="text-gray-400 text-xs font-mono select-all-text">
                            ${addr ? `📍 ${addr}` : ''}
                        </div>
                    </div>

                </div>
            `;
        }

        function getMetroCard(item) {
            const { time, title, line, dir, exit, timeEst, price, note } = item;
            return `
                <div class="bg-white rounded-xl overflow-hidden shadow-sm border-l-8 border-[#D97706] flex flex-col">
                    
                    <!-- Top Logistics -->
                    <div class="p-3 bg-slate-50 border-b border-gray-100 flex justify-between items-center">
                        <div class="flex items-center gap-2">
                            <span class="font-black text-xl text-[#8B4513]">${time}</span>
                            <div class="bg-green-600 text-white text-[10px] font-bold px-2 py-0.5 rounded-full uppercase">
                                METRO
                            </div>
                        </div>
                        <div class="text-right">
                            <div class="text-xs font-bold text-slate-500">⏱ ${timeEst}</div>
                            <div class="text-xs font-bold text-[#D97706] bg-orange-50 px-1.5 rounded mt-0.5">${price}</div>
                        </div>
                    </div>

                    <!-- Main Info -->
                    <div class="p-4">
                        <h3 class="font-bold text-lg text-slate-800 mb-3">${title}</h3>
                        
                        <div class="grid grid-cols-2 gap-4 text-sm mb-3">
                            <div>
                                <div class="text-[10px] text-slate-400 uppercase font-bold">Line & Direction</div>
                                <div class="font-bold text-slate-800">${line} <span class="font-normal text-slate-500 block text-xs">to ${dir}</span></div>
                            </div>
                            <div>
                                <div class="text-[10px] text-slate-400 uppercase font-bold">Target Exit</div>
                                <div class="font-bold text-[#D97706] text-lg flex items-center gap-1">📍 ${exit}</div>
                            </div>
                        </div>

                        ${note ? `<div class="text-xs text-slate-500 italic border-t border-slate-100 pt-2">💡 ${note}</div>` : ''}
                    </div>
                </div>
            `;
        }

        // --- 4. ACTIONS ---

        window.switchTab = function(id) {
            renderTabs(id);
            renderContent(id);
        };

        window.copyText = function(text) {
            if (!text) return;
            const textarea = document.createElement('textarea');
            textarea.value = text;
            document.body.appendChild(textarea);
            textarea.select();
            try {
                document.execCommand('copy');
                alert('Copied for DiDi:\n' + text);
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
