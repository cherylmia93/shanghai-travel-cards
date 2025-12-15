# shanghai-travel-cards
Visual cards to get to places easily. Copy address to paste on didi/amap
import React, { useState } from 'react';
import { 
  Car, 
  Train, 
  MapPin, 
  Navigation, 
  Bus, 
  Info, 
  Clock, 
  Copy, 
  Check, 
  Smartphone, 
  MessageCircle, 
  Utensils,
  CreditCard
} from 'lucide-react';

const TravelDeck = () => {
  const [activeTab, setActiveTab] = useState('guide');

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

  return (
    <div className="min-h-screen bg-[#F2F0E6] p-4 md:p-6 font-sans text-slate-800">
      <div className="max-w-3xl mx-auto">
        
        {/* Header */}
        <div className="mb-6 text-center">
          <h1 className="text-3xl md:text-5xl font-black text-[#8B4513] uppercase tracking-tight mb-2">
            Shanghai <span className="text-[#D97706]">Logistics</span>
          </h1>
          <p className="text-xs font-bold tracking-widest text-[#5C4033] uppercase">Master Transport Deck</p>
        </div>

        {/* Scrollable Tabs */}
        <div className="flex overflow-x-auto gap-2 pb-4 mb-4 no-scrollbar">
          {days.map((day) => (
            <button
              key={day.id}
              onClick={() => setActiveTab(day.id)}
              className={`flex-shrink-0 px-4 py-2 rounded-xl text-xs font-bold transition-all border-2 ${
                activeTab === day.id
                  ? 'bg-[#D97706] text-white border-[#D97706] shadow-md'
                  : 'bg-white text-[#8B4513] border-transparent hover:border-[#D97706]/30'
              }`}
            >
              <div className="opacity-80 uppercase text-[10px] mb-0.5">{day.date}</div>
              <div>{day.label}</div>
            </button>
          ))}
        </div>

        {/* Content Area */}
        <div className="space-y-6 pb-12">
          {activeTab === 'guide' ? <TravelGuide /> : <DayContent dayId={activeTab} />}
        </div>

      </div>
    </div>
  );
};

/* --- COMPONENTS --- */

const TravelGuide = () => (
  <div className="bg-white rounded-2xl p-6 shadow-lg border-2 border-[#D97706]/20">
    <h2 className="text-2xl font-black text-[#5C4033] mb-6 flex items-center gap-2">
      <Smartphone className="text-[#D97706]" /> 
      Essential Apps & How to Use
    </h2>
    
    <div className="grid grid-cols-1 gap-6">
      
      {/* 1. ALIPAY (Super App) */}
      <AppCard 
        color="bg-[#1677FF]" 
        icon={<span className="text-2xl font-bold text-white">支</span>}
        title="Alipay (Zhifubao)"
        badge="PAY • METRO • TAXI"
      >
        <li className="flex gap-2 items-start text-sm"><span className="text-[#1677FF] mt-1">●</span> <span><strong>Setup:</strong> Link your foreign Visa/Mastercard. It works for 99% of vendors, from luxury malls to street food.</span></li>
        <li className="flex gap-2 items-start text-sm"><span className="text-[#1677FF] mt-1">●</span> <span><strong>"Scan" vs "Pay":</strong> Use "Scan" (top left) to pay small merchants (QR on wall). Use "Pay/Receive" (top right) to show <i>your</i> barcode to supermarkets or Metro turnstiles.</span></li>
        <li className="flex gap-2 items-start text-sm"><span className="text-[#1677FF] mt-1">●</span> <span><strong>Metro/Bus:</strong> Click "Transport" ➔ Select "Shanghai" ➔ Scan the QR at the turnstile. No tokens needed!</span></li>
        <li className="flex gap-2 items-start text-sm bg-blue-50 p-2 rounded border border-blue-100">
          <Car size={14} className="mt-1 text-[#1677FF] shrink-0" /> 
          <span><strong>Ride Hailing:</strong> Click "Transport" ➔ "Taxi". You can type English addresses or paste Chinese from this deck. It calls DiDi cars automatically without a separate app.</span>
        </li>
      </AppCard>

      {/* 2. AMAP (Navigation) */}
      <AppCard 
        color="bg-white border-2 border-blue-400" 
        icon={<Navigation className="text-blue-500 fill-blue-500" size={28} />}
        title="Amap (Gaode Ditu)"
        badge="WALKING • FINDING SPOTS"
      >
        <li className="flex gap-2 items-start text-sm"><span className="text-blue-500 mt-1">●</span> <span><strong>Why:</strong> Google Maps is often blocked or outdated. Amap is accurate to the meter.</span></li>
        <li className="flex gap-2 items-start text-sm"><span className="text-blue-500 mt-1">●</span> <span><strong>Navigation:</strong> Copy addresses from this deck ➔ Paste into search ➔ Click the "Blue Arrow" for walking directions.</span></li>
        <li className="flex gap-2 items-start text-sm"><span className="text-blue-500 mt-1">●</span> <span><strong>3D View:</strong> The map shows 3D building shapes, which helps you recognize landmarks and entrances easier than 2D maps.</span></li>
      </AppCard>

      {/* 3. WECHAT (Comms) */}
      <AppCard 
        color="bg-[#07C160]" 
        icon={<MessageCircle className="text-white fill-white" size={28} />}
        title="WeChat (Weixin)"
        badge="CHAT • ORDERING FOOD"
      >
        <li className="flex gap-2 items-start text-sm"><span className="text-green-600 mt-1">●</span> <span><strong>Dining:</strong> Most Shanghai restaurants have a QR code on the table. Scan with WeChat to see the menu (with photos!) and order/pay directly.</span></li>
        <li className="flex gap-2 items-start text-sm"><span className="text-green-600 mt-1">●</span> <span><strong>Translation:</strong> Long-press any Chinese text message (from a driver or friend) ➔ Click "Translate". Essential for logistics.</span></li>
      </AppCard>

      {/* 4. DIANPING (Food) */}
      <AppCard 
        color="bg-[#FF6600]" 
        icon={<Utensils className="text-white" size={24} />}
        title="Dianping"
        badge="REVIEWS • QUEUING"
      >
        <li className="flex gap-2 items-start text-sm"><span className="text-orange-600 mt-1">●</span> <strong>Virtual Queue:</strong> Popular spots (like Fei Da Chu) allow remote queuing. Search the restaurant ➔ Look for "排队" (Queue) icon ➔ Get a number before you arrive to skip the wait.</li>
        <li className="flex gap-2 items-start text-sm"><span className="text-orange-600 mt-1">●</span> <strong>Visual Menu:</strong> Can't read the menu? Show the waiter the "Recommended Dishes" photos on Dianping.</li>
      </AppCard>

    </div>
  </div>
);

const AppCard = ({ color, icon, title, badge, children }) => (
  <div className="flex gap-4 items-start">
    {/* App Icon */}
    <div className={`w-14 h-14 rounded-xl shadow-md shrink-0 flex items-center justify-center ${color}`}>
      {icon}
    </div>
    
    {/* Content */}
    <div className="flex-1">
      <div className="flex justify-between items-center mb-1">
        <h3 className="font-bold text-lg text-slate-800">{title}</h3>
        <span className="text-[10px] font-bold bg-slate-200 text-slate-600 px-2 py-0.5 rounded uppercase tracking-wide">{badge}</span>
      </div>
      <ul className="space-y-1.5 text-slate-600 leading-relaxed">
        {children}
      </ul>
    </div>
  </div>
);

const DayContent = ({ dayId }) => {
  const getDayData = () => {
    switch (dayId) {
      case 0: return [
        { type: 'van', time: '22:30', title: 'AIRPORT PICKUP', cn: '虹口名苑', en: 'Hongkou Mingyuan', addr: '黄浦区, 西藏南路1501弄', wait: 'Arrivals Hall Exit (Level 1)', timeEst: '50m', price: 'Pre-booked', note: 'Look for driver with Name Sign at the barrier.' }
      ];
      case 1: return [
        { type: 'didi', time: '09:30', title: 'START', cn: '田子坊 (泰康路入口)', en: 'Tianzifang (Gate 1)', addr: '黄浦区泰康路210弄', wait: 'Curbside Airbnb', timeEst: '15m', price: '~¥25', note: 'Enter via Gate 1.' },
        { type: 'didi', time: '11:00', title: 'LUNCH', cn: '喜粵8号 (汝南街店)', en: 'Canton 8', addr: '汝南街63号 (近局门路)', wait: 'Tianzifang Gate 1', timeEst: '15m', price: '~¥20' },
        { type: 'didi', time: '13:00', title: 'SHOPPING', cn: 'Sunflour (安福路店)', en: 'Sunflour Bakery', addr: '安福路322号', wait: 'Outside Canton 8', timeEst: '20m', price: '~¥35', note: 'Base camp for seniors.' },
        { type: 'didi', time: '18:00', title: 'DINNER', cn: '蟹三宝 (南汇路店)', en: 'Xie San Bao', addr: '静安区南汇路74号', wait: 'Curbside Sunflour', timeEst: '20m', price: '~¥25', note: 'Very congested street. Group ready at curb.' },
        { type: 'didi', time: '20:00', title: 'HOME', cn: '虹口名苑', en: 'Hongkou Mingyuan', addr: '西藏南路1501弄', wait: 'Outside Xie San Bao', timeEst: '25m', price: '~¥30' }
      ];
      case 2: return [
        { type: 'metro', time: '07:30', title: 'TO DISNEY', line: 'Line 11', dir: 'Disney Resort', exit: 'Exit 1', timeEst: '50m', price: '~¥6', note: 'Transfer at Oriental Sports Center.' },
        { type: 'didi', time: '21:15', title: 'RETURN', cn: '虹口名苑', en: 'Hongkou Mingyuan', addr: '西藏南路1501弄', wait: 'West Public Transportation Hub', timeEst: '45m', price: '~¥150', note: 'Follow signs for Taxi/Ride Hailing to the WEST.' }
      ];
      case 3: return [
        { type: 'metro', time: '09:15', title: 'TO NANJING RD', line: 'Line 8', dir: 'Shiguang Rd', exit: 'Exit 19', timeEst: '15m', price: '~¥3', note: 'Exit 19 is inside New World City (Basement).' },
        { type: 'didi', time: '13:45', title: 'TO YU GARDEN', cn: '豫园商城2号门', en: 'Yu Garden Gate 2', addr: '福佑路 (Fuyou Road)', wait: 'Huanghe Road', timeEst: '20m', price: '~¥30', note: 'Call from Huanghe Rd (less crowded).' },
        { type: 'didi', time: '19:30', title: 'HOME', cn: '虹口名苑', en: 'Hongkou Mingyuan', addr: '西藏南路1501弄', wait: 'Fuyou Road (Gate 2)', timeEst: '20m', price: '~¥25' }
      ];
      case 4: return [
        { type: 'didi', time: '09:30', title: 'COFFEE', cn: '星巴克臻选烘焙工坊', en: 'Starbucks Reserve', addr: '南京西路789号', wait: 'Airbnb', timeEst: '20m', price: '~¥25' },
        { type: 'didi', time: '12:00', title: 'LUNCH', cn: '全聚德 (淮海中路店)', en: 'Quanjude Duck', addr: '淮海中路780号4楼', wait: 'Side Door: Shimen 1st Rd', timeEst: '15m', price: '~¥20', note: 'Exit Starbucks via SIDE door.' },
        { type: 'didi', time: '14:00', title: 'PHOTO', cn: '北外滩滨江绿地', en: 'North Bund', addr: '东大名路500号 (Opposite)', wait: 'Quanjude Lobby', timeEst: '20m', price: '~¥30', note: 'Silver Drop Sculpture.' },
        { type: 'didi', time: '15:30', title: 'MARKET', cn: '圆明园路 (近北京东路)', en: 'Rockbund', addr: 'Waitan Area', wait: 'North Bund Roadside', timeEst: '10m', price: '~¥15' },
        { type: 'walk', time: '17:30', title: 'DINNER', cn: '海底捞火锅 (外滩店)', en: 'Haidilao', addr: '南京东路123号', wait: 'Walk', timeEst: '8m', price: '0', note: 'Walk South on Yuanmingyuan Rd.' },
        { type: 'didi', time: '19:30', title: 'CRUISE', cn: '十六铺码头', en: 'Shiliupu Pier', addr: '中山东二路551号', wait: 'Bund Central Mall', timeEst: '10m', price: '~¥15' },
        { type: 'didi', time: '21:30', title: 'HOME', cn: '虹口名苑', en: 'Hongkou Mingyuan', addr: '西藏南路1501弄', wait: 'BFC Mall North Drop-off', timeEst: '20m', price: '~¥25', note: 'Walk 5 mins South to BFC. Do NOT wait at Pier curb.' }
      ];
      case 5: return [
        { type: 'didi', time: '09:00', title: '1000 TREES', cn: '昌化路桥 (莫干山路口)', en: 'Changhua Bridge Drop', addr: 'Moganshan Rd', wait: 'Airbnb', timeEst: '25m', price: '~¥35', note: 'Best view from the bridge.' },
        { type: 'didi', time: '10:30', title: 'LUNCH', cn: '3号仓库 (新世界城店)', en: 'No. 3 Warehouse', addr: '南京西路2-68号4楼', wait: 'West Gate 1000 Trees', timeEst: '15m', price: '~¥25' },
        { type: 'metro', time: '13:00', title: 'SKYWALK', line: 'Line 2', dir: 'Pudong Airport', exit: 'Exit 2', timeEst: '15m', price: '~¥3', note: 'Exit 2 leads to Bridge Escalator.' },
        { type: 'didi', time: '14:30', title: 'GUNDAM', cn: '啦啦宝都 (新金桥路738号)', en: 'LaLaport', addr: 'Pudong New Area', wait: 'Grand Hyatt Lobby', timeEst: '25m', price: '~¥40' },
        { type: 'didi', time: '17:00', title: 'DINNER', cn: '费大厨辣椒炒肉 (悦荟广场店)', en: 'Fei Da Chu', addr: '南京东路353号', wait: 'LaLaport Entrance', timeEst: '30m', price: '~¥35' },
        { type: 'didi', time: '21:30', title: 'HOME', cn: '虹口名苑', en: 'Hongkou Mingyuan', addr: '西藏南路1501弄', wait: 'Nanjing Road Accessible Point', timeEst: '20m', price: '~¥30' }
      ];
      case 6: return [
        { type: 'metro', time: '08:00', title: 'MEETING PT', line: 'Line 8', dir: 'Shiguang Rd', exit: 'See Ticket', timeEst: '15m', price: '~¥3', note: 'To People\'s Square.' },
        { type: 'bus', time: '08:30', title: 'TOUR BUS', cn: 'Bus Tour', en: 'Film Park & Water Town', addr: 'People\'s Square', wait: 'Designated Spot', timeEst: '50m', price: 'Included' },
        { type: 'walk', time: '18:30', title: 'DINNER', cn: '很久以前羊肉串', en: 'Henjiu Yiqian', addr: '云南南路180号', wait: 'Walk', timeEst: '10m', price: '0' },
        { type: 'didi', time: '20:30', title: 'HOME', cn: '虹口名苑', en: 'Hongkou Mingyuan', addr: '西藏南路1501弄', wait: 'Restaurant', timeEst: '15m', price: '~¥20' }
      ];
      case 7: return [
        { type: 'didi', time: '08:00', title: 'SAFARI', cn: '上海野生动物园', en: 'Wild Animal Park', addr: 'Pudong', wait: 'Airbnb', timeEst: '1.5h', price: '~¥120', note: 'Long ride. Go to bathroom first.' },
        { type: 'didi', time: '16:30', title: 'HOME', cn: '虹口名苑', en: 'Hongkou Mingyuan', addr: '西藏南路1501弄', wait: 'Main Parking Lot (Taxi Zone)', timeEst: '1.5h', price: '~¥120', note: 'Book early.' }
      ];
      case 8: return [
        { type: 'didi', time: '09:00', title: 'STATION', cn: '上海虹桥火车站 (出发层)', en: 'Hongqiao Station', addr: 'Departures Level', wait: 'Airbnb', timeEst: '45m', price: '~¥70' },
        { type: 'train', time: '10:00', title: 'TO HANGZHOU', cn: '杭州东站', en: 'Hangzhou East', addr: 'G Train', wait: 'Gates', timeEst: '50m', price: '~¥73', note: 'Passport Required.' },
        { type: 'didi', time: '11:00', title: 'LAKE', cn: '青山湖水上森林', en: 'Qingshan Lake', addr: 'Lin\'an', wait: 'Hangzhou East Taxi Stand', timeEst: '1.5h', price: '~¥100' },
        { type: 'didi', time: '15:30', title: 'STATION', cn: '杭州东站', en: 'Hangzhou East', addr: 'Hangzhou', wait: 'Lake Entrance', timeEst: '1.5h', price: '~¥100' },
        { type: 'didi', time: '19:30', title: 'HOME', cn: '虹口名苑', en: 'Hongkou Mingyuan', addr: '西藏南路1501弄', wait: 'Hongqiao P10 Parking', timeEst: '45m', price: '~¥70', note: 'Pickup at P10 Garage.' }
      ];
      case 9: return [
        { type: 'metro', time: '08:30', title: 'LUNCH', line: 'Line 8 -> 9', dir: 'Songjiang', exit: 'Exit 2', timeEst: '1h', price: '~¥6', note: 'To Songjiang University Town.' },
        { type: 'didi', time: '14:30', title: 'PHOTO', cn: '宋庆龄故居 (淮海中路1843号)', en: 'Wukang Mansion', addr: 'Opposite Side', wait: 'Restaurant', timeEst: '1h', price: '~¥100', note: 'Nap time.' },
        { type: 'didi', time: '16:00', title: 'SHOPPING', cn: '新天地 (马当路兴业路路口)', en: 'Xintiandi', addr: 'Madang Rd', wait: 'Wukang Mansion', timeEst: '20m', price: '~¥25' },
        { type: 'didi', time: '20:30', title: 'HOME', cn: '虹口名苑', en: 'Hongkou Mingyuan', addr: '西藏南路1501弄', wait: 'Fuyou Road', timeEst: '20m', price: '~¥20' }
      ];
      case 10: return [
        { type: 'didi', time: '11:30', title: 'LUGGAGE', cn: '龙阳路地铁站', en: 'Longyang Rd Station', addr: 'Maglev', wait: 'Airbnb', timeEst: '20m', price: '~¥30' },
        { type: 'didi', time: '14:30', title: 'LAST STOP', cn: 'EKA·天物', en: 'EKA Tianwu', addr: '浦东新区金桥路535号', wait: 'Longyang Rd', timeEst: '25m', price: '~¥30' },
        { type: 'didi', time: '19:00', title: 'RETRIEVE', cn: '龙阳路地铁站', en: 'Longyang Rd Station', addr: 'Maglev', wait: 'EKA Entrance', timeEst: '25m', price: '~¥30' },
        { type: 'train', time: '19:30', title: 'AIRPORT', cn: '浦东机场 T1', en: 'PVG Airport T1', addr: 'Maglev', wait: 'Platform', timeEst: '8m', price: '¥50', note: 'Maglev Train.' }
      ];
      default: return [];
    }
  };

  const data = getDayData();

  return (
    <div className="grid gap-4">
      {data.map((item, idx) => (
        item.type === 'metro' ? (
          <MetroCard key={idx} {...item} />
        ) : (
          <TransportCard key={idx} {...item} />
        )
      ))}
    </div>
  );
};

/* --- CARD COMPONENTS --- */

const TransportCard = ({ type, time, title, cn, en, addr, wait, timeEst, price, note }) => {
  const [copied, setCopied] = useState(false);

  const handleCopy = () => {
    if (!cn) return;
    const textToCopy = `${cn} ${addr || ''}`;
    const textarea = document.createElement('textarea');
    textarea.value = textToCopy;
    document.body.appendChild(textarea);
    textarea.select();
    
    try {
      document.execCommand('copy');
      setCopied(true);
      setTimeout(() => setCopied(false), 2000);
    } catch (err) {
      console.error('Copy failed', err);
    }
    document.body.removeChild(textarea);
  };

  return (
    <div className="bg-[#3E3E3E] text-white rounded-xl overflow-hidden shadow-lg border border-gray-700 hover:scale-[1.01] transition-transform">
      {/* Top Banner (Clickable) */}
      <div 
        onClick={handleCopy}
        className="bg-[#2A2A2A] p-4 relative cursor-pointer hover:bg-[#333] transition-colors border-b border-gray-600 group"
      >
        <div className="flex justify-between items-center mb-3">
          {/* Badge */}
          <div className="bg-[#D97706] text-[#3E3E3E] text-xs font-black px-3 py-1 rounded-full uppercase flex items-center gap-1">
            {type === 'didi' || type === 'van' ? <Car size={12} /> : 
             type === 'walk' ? <Navigation size={12} /> : 
             type === 'bus' ? <Bus size={12} /> : 
             type === 'train' ? <Train size={12} /> : <MapPin size={12} />}
            {type === 'didi' ? 'DiDi / Taxi' : type.toUpperCase()}
          </div>

          {/* Time & Price */}
          <div className="text-right">
            <div className="text-xs font-mono text-gray-300 flex items-center justify-end gap-1">
              <Clock size={12} className="text-[#D97706]" /> {timeEst}
            </div>
            <div className="text-[#D97706] font-bold text-sm bg-[#3E3E3E] px-2 py-0.5 rounded mt-1 inline-block border border-[#D97706]/30">
              {price}
            </div>
          </div>
        </div>

        {/* Copy Indicator */}
        <div className="flex items-start justify-between gap-4">
          <div>
            <h3 className="text-2xl font-bold text-[#D97706] leading-tight mb-1">{cn}</h3>
            <div className="text-white font-bold text-sm">{en}</div>
            {addr && <div className="text-gray-400 text-xs mt-1 font-mono">📍 {addr}</div>}
          </div>
          
          <div className={`shrink-0 transition-opacity duration-200 ${copied ? 'text-green-400' : 'text-gray-500 group-hover:text-white'}`}>
            {copied ? <Check size={20} /> : <Copy size={20} />}
          </div>
        </div>
      </div>

      {/* Bottom Info */}
      <div className="p-4 bg-[#3E3E3E]">
        <div className="mb-2">
          <div className="text-[10px] text-gray-400 uppercase font-bold tracking-wider mb-1">Time & Wait Point</div>
          <div className="font-bold text-lg">{time} <span className="text-gray-400 font-normal text-sm">@ {wait}</span></div>
        </div>
        
        {note && (
          <div className="bg-[#2A2A2A] p-2 rounded text-xs text-gray-300 flex items-start gap-2 border-l-2 border-[#D97706]">
            <Info size={14} className="text-[#D97706] shrink-0 mt-0.5" />
            {note}
          </div>
        )}
      </div>
    </div>
  );
};

const MetroCard = ({ time, title, line, dir, exit, timeEst, price, note }) => (
  <div className="bg-white rounded-xl overflow-hidden shadow-sm border-l-8 border-[#D97706] p-4 flex flex-col gap-3">
    <div className="flex justify-between items-center">
      <div className="flex items-center gap-2 text-[#8B4513]">
        <Train size={20} />
        <span className="font-black text-lg">METRO: {title}</span>
      </div>
      <div className="text-right">
        <div className="text-xs bg-gray-100 px-2 py-1 rounded text-gray-600 font-mono mb-1">
          {time} • {timeEst}
        </div>
        <div className="text-[#D97706] font-bold text-xs bg-orange-50 px-2 py-0.5 rounded border border-orange-100">
          {price}
        </div>
      </div>
    </div>

    <div className="grid grid-cols-2 gap-4 text-sm">
      <div>
        <div className="text-xs text-gray-400 uppercase font-bold">Line & Direction</div>
        <div className="font-bold text-gray-800">{line} <span className="font-normal text-gray-500">to {dir}</span></div>
      </div>
      <div>
        <div className="text-xs text-gray-400 uppercase font-bold">Target Exit</div>
        <div className="font-bold text-[#D97706] text-lg flex items-center gap-1">
          <Navigation size={16} /> {exit}
        </div>
      </div>
    </div>

    {note && (
      <div className="text-xs text-gray-500 italic border-t border-gray-100 pt-2 mt-1">
        💡 {note}
      </div>
    )}
  </div>
);

export default TravelDeck;
