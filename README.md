<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>小小分析師挑戰賽 | 三年級進階數學</title>
    
    <!-- 引入 React 和 ReactDOM -->
    <script crossorigin src="https://unpkg.com/react@18/umd/react.development.js"></script>
    <script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
    
    <!-- 引入 Babel 用來解析 JSX -->
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    
    <!-- 引入 Tailwind CSS 進行排版 -->
    <script src="https://cdn.tailwindcss.com"></script>
    
    <!-- 設定 Tailwind 的字型與顏色 -->
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['Helvetica', 'Arial', 'PingFang TC', 'Heiti TC', 'Microsoft JhengHei', 'sans-serif'],
                    },
                    colors: {
                        primary: '#4F46E5', // Indigo 600
                        primaryHover: '#4338CA', // Indigo 700
                        secondary: '#10B981', // Emerald 500
                    },
                    keyframes: {
                        fadeIn: {
                            '0%': { opacity: '0', transform: 'translateY(-10px)' },
                            '100%': { opacity: '1', transform: 'translateY(0)' },
                        },
                        popIn: {
                            '0%': { opacity: '0', transform: 'scale(0.9)' },
                            '100%': { opacity: '1', transform: 'scale(1)' },
                        }
                    },
                    animation: {
                        'fade-in': 'fadeIn 0.3s ease-out forwards',
                        'pop-in': 'popIn 0.5s cubic-bezier(0.175, 0.885, 0.32, 1.275) forwards',
                    }
                }
            }
        }
    </script>

    <style>
        /* 針對 iOS Safari 的優化 */
        body {
            -webkit-tap-highlight-color: transparent;
            -webkit-touch-callout: none;
            overscroll-behavior-y: none;
        }
        input {
            font-size: 16px; /* 防止 iOS 自動縮放 */
        }
        /* 隱藏捲軸但保留功能 */
        .no-scrollbar::-webkit-scrollbar {
            display: none;
        }
        .no-scrollbar {
            -ms-overflow-style: none;
            scrollbar-width: none;
        }
    </style>
</head>
<body class="bg-slate-50 text-slate-800 min-h-screen flex flex-col">
    <div id="root" class="flex-grow flex flex-col"></div>

    <script type="text/babel">
        const { useState, useEffect, useRef } = React;

        // --- 內建圖示組件 (SVG) ---
        const IconBase = ({ d, className, children, ...props }) => (
            <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round" className={className} {...props}>
                {d ? <path d={d} /> : children}
            </svg>
        );

        const Icons = {
            ChevronLeft: (props) => <IconBase d="M15 18l-6-6 6-6" {...props} />,
            ChevronRight: (props) => <IconBase d="M9 18l6-6-6-6" {...props} />,
            Calculator: (props) => (
                <IconBase {...props}>
                    <rect x="4" y="2" width="16" height="20" rx="2"></rect>
                    <line x1="8" y1="6" x2="16" y2="6"></line>
                    <line x1="16" y1="14" x2="16" y2="18"></line>
                    <path d="M16 10h.01"></path>
                    <path d="M12 10h.01"></path>
                    <path d="M8 10h.01"></path>
                    <path d="M12 14h.01"></path>
                    <path d="M8 14h.01"></path>
                    <path d="M12 18h.01"></path>
                    <path d="M8 18h.01"></path>
                </IconBase>
            ),
            CheckCircle: (props) => <IconBase d="M22 11.08V12a10 10 0 1 1-5.93-9.14 M22 4L12 14.01l-3-3" {...props} />,
            XCircle: (props) => <IconBase d="M18 6L6 18M6 6l12 12" {...props} />,
            HelpCircle: (props) => (
                <IconBase {...props}>
                    <circle cx="12" cy="12" r="10"></circle>
                    <path d="M9.09 9a3 3 0 0 1 5.83 1c0 2-3 3-3 3"></path>
                    <line x1="12" y1="17" x2="12.01" y2="17"></line>
                </IconBase>
            ),
            Map: (props) => <IconBase d="M1 6v16l7-4 8 4 7-4V2l-7 4-8-4-7 4z M8 2v16 M16 6v16" {...props} />,
            Clock: (props) => (
                <IconBase {...props}>
                    <circle cx="12" cy="12" r="10"></circle>
                    <polyline points="12 6 12 12 16 14"></polyline>
                </IconBase>
            ),
            PieChart: (props) => <IconBase d="M21.21 15.89A10 10 0 1 1 8 2.83 M22 12A10 10 0 0 0 12 2v10z" {...props} />,
            ShoppingCart: (props) => <IconBase d="M9 21h6 M9 21a2 2 0 0 1-2-2v-1H5a2 2 0 0 1-2-2V4h2l2.35 11.59L19 19a2 2 0 0 1 2 2v1a2 2 0 0 1-2 2H9z M1 1h4 M16 6h4 M16 10h4" {...props} />,
            Activity: (props) => <IconBase d="M22 12h-4l-3 9L9 3l-3 9H2" {...props} />,
            Anchor: (props) => (
                <IconBase {...props}>
                    <circle cx="12" cy="5" r="3"></circle>
                    <line x1="12" y1="22" x2="12" y2="8"></line>
                    <path d="M5 12H2a10 10 0 0 0 20 0h-3"></path>
                </IconBase>
            ),
            BookOpen: (props) => <IconBase d="M2 3h6a4 4 0 0 1 4 4v14a3 3 0 0 0-3-3H2z M22 3h-6a4 4 0 0 0-4 4v14a3 3 0 0 1 3-3h7z" {...props} />,
            User: (props) => (
                <IconBase {...props}>
                    <path d="M20 21v-2a4 4 0 0 0-4-4H8a4 4 0 0 0-4 4v2"></path>
                    <circle cx="12" cy="7" r="4"></circle>
                </IconBase>
            ),
            DollarSign: (props) => <IconBase d="M12 1v22 M17 5H9.5a3.5 3.5 0 0 0 0 7h5a3.5 3.5 0 0 1 0 7H6" {...props} />,
            Database: (props) => (
                <IconBase {...props}>
                    <ellipse cx="12" cy="5" rx="9" ry="3"></ellipse>
                    <path d="M21 12c0 1.66-4 3-9 3s-9-1.34-9-3"></path>
                    <path d="M3 5v14c0 1.66 4 3 9 3s9-1.34 9-3V5"></path>
                </IconBase>
            ),
            Star: (props) => (
                <IconBase {...props} fill={props.fill || "none"}>
                    <polygon points="12 2 15.09 8.26 22 9.27 17 14.14 18.18 21.02 12 17.77 5.82 21.02 7 14.14 2 9.27 8.91 8.26 12 2"></polygon>
                </IconBase>
            ),
            RefreshCw: (props) => <IconBase d="M23 4v6h-6M1 20v-6h6" {...props}><path d="M3.51 9a9 9 0 0 1 14.85-3.36L23 10M1 14l4.64 4.36A9 9 0 0 0 20.49 15"></path></IconBase>
        };

        // --- 資料與題庫 ---
        // 修正：移除了所有括號內的提示，讓學生依賴題目敘述與圖表作答
        const problemSets = [
          {
            id: 1,
            title: "星際列車的時刻表危機",
            icon: <Icons.Clock className="w-6 h-6" />,
            story: "西元 2050 年，小光一家人住在「月球基地」，計畫週末前往「火星渡假村」。爸爸把時刻表交給小光規劃。他們必須在中午 12:00 以前抵達火星，才能趕上飯店的自助午餐。",
            visualType: "table_train",
            questions: [
              { q: "請問搭乘「光速號」從月球飛到火星，總共需要花費幾小時幾分鐘？", a: "1 小時 40 分鐘 (09:30 到 11:30 是 2 小時，提早 20 分鐘)" },
              { q: "請問搭乘「彗星號」飛行時間是幾小時幾分鐘？它比「光速號」慢了多少分鐘？", a: "3 小時。慢了 80 分鐘 (3小時 = 180分，1小時40分 = 100分)" },
              { q: "如果小光一家人有 2 位大人和 1 位小孩，選擇搭乘「彗星號」購買來回票，總共需要花多少錢？", a: "1700 元 (單程總和 850 元 x 2 趟)" },
              { q: "爸爸說：「如果飛行時間少於 2 小時，我就願意多花錢。」請問光速號符合要求嗎？", a: "符合 (1小時40分 < 2小時)" },
              { q: "回程必須趕上 16:00 的電影。若搭乘「光速號」（飛行時間與去程相同），最晚幾點幾分從火星出發？", a: "14:20 (下午 2 點 20 分，才來得及在 16:00 到)" }
            ]
          },
          {
            id: 2,
            title: "魔法藥水店的庫存盤點",
            icon: <Icons.Database className="w-6 h-6" />,
            story: "古靈精怪先生正在盤點倉庫。巨大的玻璃缸裡裝滿了「史萊姆黏液」。昨天進貨時，大水缸原本有 8 公升。早上賣給石內卜教授 2 公升 500 毫升，下午榮恩打破瓶子流失了 450 毫升。",
            visualType: "chart_potion",
            questions: [
              { q: "早上賣給石內卜教授之後，水缸裡還剩下幾毫升的史萊姆黏液？", a: "5500 毫升 (8000 - 2500)" },
              { q: "扣掉下午打破流失的部分，現在水缸裡最後剩下多少毫升的黏液？", a: "5050 毫升 (5500 - 450)" },
              { q: "店長準備了容量為 900 毫升的中型玻璃瓶。請問剩下的黏液，最多可以「裝滿」幾瓶？", a: "5 瓶 (900 x 5 = 4500，還夠；900 x 6 = 5400，不夠)" },
              { q: "承上題，裝滿之後，水缸裡還會剩下多少毫升的黏液無法裝滿一瓶？", a: "550 毫升 (5050 - 4500)" },
              { q: "裝滿的每瓶賣 200 元，剩下的零頭以 1 毫升 1 元賣掉。請問全部賣光可以賺多少錢？", a: "1550 元 (5瓶x200 + 550元)" }
            ]
          },
          {
            id: 3,
            title: "營養午餐的披薩派對",
            icon: <Icons.PieChart className="w-6 h-6" />,
            story: "三年五班慶祝表現優良，訂了 5 大盒披薩。每一盒都要切成一樣大小的 8 片。全班共有 28 位學生，加上 1 位老師和 1 位實習老師，每個人都要分到一片。",
            visualType: "illustration_pizza",
            questions: [
              { q: "請問 5 大盒披薩總共被切成了幾片？", a: "40 片 (5 x 8)" },
              { q: "教室裡總共有多少人要吃披薩？", a: "30 人 (28學生 + 1老師 + 1實習)" },
              { q: "每人拿走一片後，還剩下幾片披薩？", a: "10 片 (40 - 30)" },
              { q: "這些剩下的披薩是幾分之幾盒？", a: "10/8 盒 或 1又2/8 盒 (或 1又1/4盒)" },
              { q: "隔壁班體育老師要吃掉「半盒」。剩下的披薩夠不夠給他吃？", a: "夠 (剩下10片，大於4片)" }
            ]
          },
          {
            id: 4,
            title: "開心農場的圍籬工程",
            icon: <Icons.Map className="w-6 h-6" />,
            story: "王爺爺的長方形農地長 24 公尺、寬 16 公尺。他要在四周每隔 4 公尺打一根木樁。農地正中間還劃出一塊邊長 5 公尺的正方形區域種草莓，其餘區域鋪上防草布。",
            visualType: "map_farm",
            questions: [
              { q: "請問這塊長方形農地外圍的周長是多少公尺？", a: "80 公尺 ((24+16) x 2)" },
              { q: "如果要圍滿農地四周，每隔 4 公尺打一根木樁，請問總共需要幾根木樁？", a: "20 根 (80 ÷ 4，封閉圖形不用加1)" },
              { q: "中間的正方形草莓園也需要圍一圈小圍籬，請問草莓園的周長是多少公尺？", a: "20 公尺 (5 x 4)" },
              { q: "請問「草莓園的周長」比「農地外圍的周長」短了多少公尺？", a: "60 公尺 (80 - 20)" },
              { q: "圍籬材料費 1 公尺 50 元。圍最外面那一圈大長方形需要花多少錢？", a: "4000 元 (80 x 50)" }
            ]
          },
          {
            id: 5,
            title: "週年慶的集點大作戰",
            icon: <Icons.ShoppingCart className="w-6 h-6" />,
            story: "快樂玩具城集點活動：「消費滿 100 元獲得 1 點。集滿 10 點可兌換 100 元折價券。」小明想買樂高，妹妹想要洋娃娃。",
            visualType: "card_toys",
            questions: [
              { q: "請問買一組 2400 元的樂高積木，總共可以獲得幾點點數？", a: "24 點" },
              { q: "這些點數可以兌換幾張 100 元的折價券？", a: "2 張 (用掉 20 點)" },
              { q: "承上題，兌換完折價券後，還會剩下幾點點數沒用到？", a: "4 點" },
              { q: "接下來買洋娃娃，使用了所有折價券。還需要拿出多少「現金」？", a: "600 元 (800 - 200)" },
              { q: "如果一開始三樣玩具一起結帳，總共可以拿到幾點？", a: "44 點 (總金額4400元)" }
            ]
          },
          {
            id: 6,
            title: "馬拉松大賽的補給站",
            icon: <Icons.Activity className="w-6 h-6" />,
            story: "城市馬拉松全長 10 公里。起點出發後，第1補給站在2公里處。第2補給站距離第1補給站2500公尺。第3補給站距離終點只剩1500公尺。",
            visualType: "chart_marathon",
            questions: [
              { q: "請將 10 公里換算成公尺。", a: "10000 公尺" },
              { q: "請問第 2 補給站距離起點多少公尺？", a: "4500 公尺 (2000 + 2500)" },
              { q: "選手跑到第 2 補給站時，距離終點還有多遠？", a: "5500 公尺 (10000 - 4500)" },
              { q: "第 3 補給站和第 2 補給站之間相距多少公尺？", a: "4000 公尺 (8500 - 4500)" },
              { q: "小傑停在 6000 公尺處休息。請問他「已經通過」第 2 補給站了嗎？距離第 2 補給站多遠？", a: "已經通過 (6000 > 4500)，距離 1500 公尺" }
            ]
          },
          {
            id: 7,
            title: "小學徒的麵包烘焙課",
            icon: <Icons.BookOpen className="w-6 h-6" />,
            story: "胖胖麵包店要做 8 個巨無霸菠蘿麵包。單個配方：糖 300 克，奶油比糖少 50 克，麵粉是糖的 5 倍。",
            visualType: "card_recipe",
            questions: [
              { q: "請問做一個巨無霸菠蘿麵包，需要多少公克的麵粉？", a: "1500 公克 (300 x 5)" },
              { q: "請問做一個巨無霸菠蘿麵包，需要多少公克的奶油？", a: "250 公克 (300 - 50)" },
              { q: "做完這 8 個麵包後，一袋 10 公斤的麵粉會剩下多少公克？", a: "不夠用！(需要12000g，只有10000g，缺2000g)" },
              { q: "烤箱一次只能烤 2 個，每次 25 分鐘。烤完 8 個麵包最少需要幾分鐘？", a: "100 分鐘 (4 批 x 25)" },
              { q: "承上題，下午 2:00 開始烤，中間無休息，最後一批出爐時間是？", a: "15:40 (下午 3 點 40 分)" }
            ]
          },
          {
            id: 8,
            title: "圖書館的搬書大挑戰",
            icon: <Icons.BookOpen className="w-6 h-6" />,
            story: "圖書館搬遷。A櫃繪本8層每層45本。B櫃漫畫6層每層60本。C櫃小說總共350本。每位同學的箱子最多裝40本。",
            visualType: "table_books",
            questions: [
              { q: "請問 A 櫃裡的繪本總共有幾本？", a: "360 本 (45 x 8)" },
              { q: "請問 A 櫃的書比 B 櫃的書多還是少？相差幾本？", a: "一樣多 (360 vs 360，相差0)" },
              { q: "如果要把 C 櫃的小說全部裝箱，最少需要幾個箱子才夠？", a: "9 個箱子 (350 ÷ 40 = 8...30，餘數要多一箱)" },
              { q: "小明搬 B 櫃漫畫已搬 5 箱(滿)，還剩下幾本沒搬？", a: "160 本 (360 - 200)" },
              { q: "請全班 30 人和 2 位老師喝珍奶，每杯 45 元。老師拿出 2000 元付錢，請問夠不夠？找回多少？", a: "夠，找回 560 元 (技巧：30x45 + 2x45 = 1350 + 90 = 1440)" }
            ]
          },
          {
            id: 9,
            title: "勇者的冒險與金幣",
            icon: <Icons.DollarSign className="w-6 h-6" />,
            story: "勇者阿瑞原本有 500 金幣。事件1：買3瓶藥水每瓶60。事件2：寶箱將金幣變為當下的一半。事件3：付路費200。",
            visualType: "illustration_rpg",
            questions: [
              { q: "經過「事件一」後，阿瑞買藥水花掉多少？還剩多少？", a: "花 180，剩 320" },
              { q: "經過「事件二」時，寶箱裡有多少枚金幣？", a: "160 枚 (320的一半)" },
              { q: "拿走寶箱金幣後，阿瑞現在身上總共有多少枚金幣？", a: "480 枚 (320 + 160)" },
              { q: "經過「事件三」付完過路費後，阿瑞最後剩下多少枚金幣？", a: "280 枚 (480 - 200)" },
              { q: "買傳說寶劍(1000)還差多少枚金幣？", a: "720 枚 (1000 - 280)" }
            ]
          },
          {
            id: 10,
            title: "水族館的餵食秀",
            icon: <Icons.Anchor className="w-6 h-6" />,
            story: "鯨鯊點點每天吃 18 公斤。兩隻海豚每天「共」吃 12 公斤。倉庫剩下 150 公斤魚，要撐一星期(7天)。",
            visualType: "chart_aquarium",
            questions: [
              { q: "三隻動物每天「總共」要吃掉多少公斤的魚？", a: "30 公斤 (18 + 12)" },
              { q: "一個星期下來，這三隻動物總共需要吃掉多少公斤的魚？", a: "210 公斤 (30 x 7)" },
              { q: "倉庫的 150 公斤魚夠不夠？缺多少？", a: "不夠，缺 60 公斤" },
              { q: "週日表演成功多給2公斤(兩隻平分)。當天每隻海豚吃幾公斤？", a: "7 公斤 ((12+2) ÷ 2)" },
              { q: "點點下個月食量變成原來的 2 倍少 5 公斤。是多少？", a: "31 公斤 (18 x 2 - 5)" }
            ]
          }
        ];

        // --- 視覺化組件 ---
        const VisualContainer = ({ children, title }) => (
          <div className="bg-white p-4 rounded-lg shadow-sm border-2 border-slate-200 my-4">
            <div className="text-sm font-bold text-slate-500 mb-2 uppercase tracking-wide">{title}</div>
            <div className="flex justify-center items-center overflow-x-auto no-scrollbar">
              {children}
            </div>
          </div>
        );

        // 各種視覺化組件 (保持不變)
        const TrainTable = () => (
          <div className="w-full max-w-md overflow-x-auto">
            <table className="w-full text-sm text-left border-collapse min-w-[300px]">
              <thead>
                <tr className="bg-blue-100">
                  <th className="border p-2 whitespace-nowrap">車次名稱</th>
                  <th className="border p-2 whitespace-nowrap">出發 (月球)</th>
                  <th className="border p-2 whitespace-nowrap">抵達 (火星)</th>
                  <th className="border p-2 whitespace-nowrap">成人票</th>
                  <th className="border p-2 whitespace-nowrap">兒童票</th>
                </tr>
              </thead>
              <tbody>
                <tr>
                  <td className="border p-2 font-bold text-blue-600 whitespace-nowrap">光速號</td>
                  <td className="border p-2">09:30</td>
                  <td className="border p-2">11:10</td>
                  <td className="border p-2">$500</td>
                  <td className="border p-2">$250</td>
                </tr>
                <tr className="bg-slate-50">
                  <td className="border p-2 font-bold text-green-600 whitespace-nowrap">彗星號</td>
                  <td className="border p-2">08:15</td>
                  <td className="border p-2">11:15</td>
                  <td className="border p-2">$350</td>
                  <td className="border p-2">$150</td>
                </tr>
              </tbody>
            </table>
          </div>
        );

        const PotionChart = () => (
          <div className="flex items-end gap-8 h-48">
            <div className="relative w-24 h-40 border-2 border-slate-400 rounded-b-lg bg-slate-100 flex flex-col justify-end overflow-hidden">
              <div className="absolute top-2 right-1 text-xs text-slate-400">8L (滿)</div>
              <div className="w-full h-[60%] bg-green-400 opacity-80 border-t border-green-500 animate-pulse relative">
                 <span className="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 text-white font-bold text-xs whitespace-nowrap">目前存量</span>
              </div>
              <div className="absolute -bottom-6 w-full text-center text-xs font-bold">大水缸</div>
            </div>
            <div className="flex gap-2">
               {[1,2,3].map(i => (
                 <div key={i} className="w-8 h-12 border border-slate-300 rounded-b-md bg-green-200 flex items-center justify-center">
                   <span className="text-[10px]">900ml</span>
                 </div>
               ))}
               <div className="w-8 h-12 flex items-center justify-center text-slate-400 text-xs">...</div>
            </div>
          </div>
        );

        const PizzaIllustration = () => (
          <div className="flex flex-wrap justify-center gap-4">
            {[1, 2, 3, 4, 5].map((i) => (
              <div key={i} className="relative w-20 h-20">
                 <svg viewBox="0 0 100 100" className="w-full h-full drop-shadow-md">
                   <circle cx="50" cy="50" r="48" fill="#FEF3C7" stroke="#D97706" strokeWidth="2" />
                   <line x1="50" y1="2" x2="50" y2="98" stroke="#D97706" strokeWidth="1" />
                   <line x1="2" y1="50" x2="98" y2="50" stroke="#D97706" strokeWidth="1" />
                   <line x1="16" y1="16" x2="84" y2="84" stroke="#D97706" strokeWidth="1" />
                   <line x1="84" y1="16" x2="16" y2="84" stroke="#D97706" strokeWidth="1" />
                   <circle cx="30" cy="30" r="4" fill="#EF4444" />
                   <circle cx="70" cy="70" r="4" fill="#EF4444" />
                   <circle cx="70" cy="30" r="4" fill="#EF4444" />
                   <circle cx="30" cy="70" r="4" fill="#EF4444" />
                 </svg>
                 <span className="absolute -bottom-6 left-1/2 -translate-x-1/2 text-xs font-bold text-slate-600">Box {i}</span>
              </div>
            ))}
          </div>
        );

        const FarmMap = () => (
          <div className="relative w-64 h-48 bg-[#e2e8f0] border-4 border-dashed border-amber-700 p-4 flex items-center justify-center">
            <div className="absolute top-1 left-2 text-xs text-amber-800 font-bold">長 24m</div>
            <div className="absolute left-1 top-1/2 -translate-y-1/2 -rotate-90 text-xs text-amber-800 font-bold">寬 16m</div>
            
            <div className="absolute top-0 left-0 w-full h-full flex justify-between">
               <div className="h-full border-l-4 border-dotted border-amber-900/30"></div>
               <div className="h-full border-r-4 border-dotted border-amber-900/30"></div>
            </div>

            <div className="w-20 h-20 bg-red-100 border-2 border-red-500 flex flex-col items-center justify-center relative">
              <span className="text-2xl">🍓</span>
              <span className="text-[10px] font-bold text-red-700">草莓園</span>
              <span className="text-[8px] text-red-600">邊長 5m</span>
            </div>
          </div>
        );

        const ToyCards = () => (
          <div className="flex flex-wrap gap-4 justify-center">
            <div className="w-28 bg-white border border-slate-200 rounded-lg p-3 shadow-sm flex flex-col items-center">
              <div className="w-10 h-10 bg-blue-100 rounded-full flex items-center justify-center mb-2 text-lg">🧱</div>
              <div className="text-xs font-bold text-center">樂高積木</div>
              <div className="text-blue-600 font-bold text-sm">$2400</div>
            </div>
            <div className="w-28 bg-white border border-slate-200 rounded-lg p-3 shadow-sm flex flex-col items-center">
              <div className="w-10 h-10 bg-pink-100 rounded-full flex items-center justify-center mb-2 text-lg">🎀</div>
              <div className="text-xs font-bold text-center">洋娃娃</div>
              <div className="text-pink-600 font-bold text-sm">$800</div>
            </div>
            <div className="w-28 bg-white border border-slate-200 rounded-lg p-3 shadow-sm flex flex-col items-center">
              <div className="w-10 h-10 bg-yellow-100 rounded-full flex items-center justify-center mb-2 text-lg">🤖</div>
              <div className="text-xs font-bold text-center">變形金剛</div>
              <div className="text-yellow-600 font-bold text-sm">$1200</div>
            </div>
          </div>
        );

        const MarathonChart = () => (
          <div className="w-full max-w-lg pt-8 pb-2 px-4">
            <div className="relative w-full h-2 bg-slate-300 rounded-full">
              <div className="absolute -top-1 left-0 w-4 h-4 bg-green-500 rounded-full border-2 border-white"></div>
              <div className="absolute -top-6 left-0 text-xs font-bold text-green-700">起點</div>
              
              <div className="absolute -top-1 left-[20%] w-4 h-4 bg-yellow-400 rounded-full border-2 border-white"></div>
              <div className="absolute top-4 left-[20%] -translate-x-1/2 text-[10px] font-bold text-slate-600 text-center whitespace-nowrap">補給1<br/>2km</div>

              <div className="absolute -top-1 left-[45%] w-4 h-4 bg-yellow-400 rounded-full border-2 border-white"></div>
              <div className="absolute -top-9 left-[45%] -translate-x-1/2 text-[10px] font-bold text-slate-600 text-center whitespace-nowrap">補給2<br/>(前進2500m)</div>

              <div className="absolute -top-1 left-[85%] w-4 h-4 bg-yellow-400 rounded-full border-2 border-white"></div>
              <div className="absolute top-4 left-[85%] -translate-x-1/2 text-[10px] font-bold text-slate-600 text-center whitespace-nowrap">補給3<br/>(剩1500m)</div>

              <div className="absolute -top-1 right-0 w-4 h-4 bg-red-500 rounded-full border-2 border-white"></div>
              <div className="absolute -top-6 right-0 text-xs font-bold text-red-700">終點</div>
            </div>
            <div className="mt-10 text-center text-xs text-slate-500">全程 10 公里</div>
          </div>
        );

        const RecipeCard = () => (
          <div className="bg-[#fffbeb] border border-amber-200 p-4 rounded-lg shadow-sm max-w-sm w-full font-serif relative mx-auto">
            <div className="absolute -top-2 left-1/2 -translate-x-1/2 bg-amber-500 text-white px-3 py-1 text-xs rounded-full font-sans whitespace-nowrap">師傅的祕密配方</div>
            <h3 className="text-center font-bold text-amber-900 mt-2 mb-4 text-lg border-b border-amber-200 pb-2">巨無霸菠蘿麵包</h3>
            <ul className="space-y-3 text-sm text-amber-900">
              <li className="flex justify-between items-center">
                <span>🍬 糖</span>
                <span className="font-bold">300 公克</span>
              </li>
              <li className="flex justify-between items-center">
                <span>🧈 奶油</span>
                <span className="font-bold">比糖少 50 公克</span>
              </li>
              <li className="flex justify-between items-center">
                <span>🌾 麵粉</span>
                <span className="font-bold">糖的 5 倍</span>
              </li>
            </ul>
          </div>
        );

        const BooksTable = () => (
          <div className="w-full max-w-md overflow-x-auto">
            <table className="w-full text-sm text-left border-collapse bg-white min-w-[300px]">
              <thead>
                <tr className="bg-indigo-100">
                  <th className="border border-indigo-200 p-2 whitespace-nowrap">書櫃代號</th>
                  <th className="border border-indigo-200 p-2 whitespace-nowrap">種類</th>
                  <th className="border border-indigo-200 p-2 whitespace-nowrap">層數</th>
                  <th className="border border-indigo-200 p-2 whitespace-nowrap">每層本數</th>
                </tr>
              </thead>
              <tbody>
                <tr>
                  <td className="border border-indigo-100 p-2 font-bold text-center">A</td>
                  <td className="border border-indigo-100 p-2">繪本</td>
                  <td className="border border-indigo-100 p-2 text-center">8</td>
                  <td className="border border-indigo-100 p-2 text-center">45</td>
                </tr>
                <tr>
                  <td className="border border-indigo-100 p-2 font-bold text-center">B</td>
                  <td className="border border-indigo-100 p-2">漫畫</td>
                  <td className="border border-indigo-100 p-2 text-center">6</td>
                  <td className="border border-indigo-100 p-2 text-center">60</td>
                </tr>
                <tr>
                  <td className="border border-indigo-100 p-2 font-bold text-center">C</td>
                  <td className="border border-indigo-100 p-2">小說</td>
                  <td className="border border-indigo-100 p-2 text-center text-slate-400">-</td>
                  <td className="border border-indigo-100 p-2 text-center font-bold text-indigo-600">總共 350</td>
                </tr>
              </tbody>
            </table>
          </div>
        );

        const RPGInterface = () => (
          <div className="w-full max-w-md bg-slate-800 rounded-lg p-4 text-white font-mono border-4 border-slate-600 mx-auto">
            <div className="flex justify-between items-center mb-4 border-b border-slate-600 pb-2">
              <div className="flex items-center gap-2">
                <div className="w-8 h-8 bg-blue-500 rounded-full flex items-center justify-center">勇</div>
                <span>阿瑞 LV.3</span>
              </div>
              <div className="text-yellow-400">💰 500 G</div>
            </div>
            <div className="space-y-2 text-sm">
              <div className="bg-slate-700 p-2 rounded text-green-300">
                &gt; 遇到史萊姆商人！<br/>
                &gt; 藥水特價：$60/瓶
              </div>
              <div className="bg-slate-700 p-2 rounded text-yellow-300">
                &gt; 發現寶箱！<br/>
                &gt; 內容：目前金幣的一半
              </div>
              <div className="bg-slate-700 p-2 rounded text-red-300">
                &gt; 大魔王擋路！<br/>
                &gt; 過路費：$200
              </div>
            </div>
          </div>
        );

        const AquariumChart = () => (
          <div className="flex justify-around items-end w-full max-w-md h-40 bg-blue-50 rounded-xl border border-blue-200 p-4 relative overflow-hidden mx-auto">
            <div className="absolute bottom-0 left-0 w-full h-1/2 bg-blue-100 -z-10 rounded-b-xl"></div>
            
            <div className="flex flex-col items-center">
              <div className="text-xs font-bold text-blue-800 mb-1">鯨鯊點點</div>
              <div className="w-24 h-16 bg-blue-500 rounded-full flex items-center justify-center text-white font-bold relative">
                <span className="z-10">18kg</span>
                <div className="absolute right-0 w-8 h-8 bg-blue-500 rounded-full translate-x-3"></div>
              </div>
              <div className="text-[10px] text-blue-600 mt-1">每天食量</div>
            </div>

            <div className="flex flex-col items-center">
              <div className="text-xs font-bold text-blue-800 mb-1">海豚雙人組</div>
              <div className="flex gap-1">
                <div className="w-12 h-8 bg-cyan-400 rounded-full flex items-center justify-center text-white text-xs">跳</div>
                <div className="w-12 h-8 bg-cyan-400 rounded-full flex items-center justify-center text-white text-xs">波</div>
              </div>
              <div className="bg-white/80 px-2 py-1 rounded-full text-xs font-bold text-cyan-700 mt-1 shadow-sm whitespace-nowrap">
                共 12 kg
              </div>
            </div>
          </div>
        );

        // --- 成績單組件 ---
        const ScoreCard = ({ score, total, onRestart }) => {
            let message = "";
            let color = "";
            const percentage = (score / total) * 100;

            if (percentage === 100) {
                message = "太厲害了！數學小天才！🏆";
                color = "text-yellow-500";
            } else if (percentage >= 80) {
                message = "非常棒！小小數學家！🌟";
                color = "text-green-500";
            } else if (percentage >= 60) {
                message = "不錯喔！繼續保持！👍";
                color = "text-blue-500";
            } else {
                message = "別氣餒，再接再厲！💪";
                color = "text-orange-500";
            }

            return (
                <div className="bg-white p-8 rounded-2xl shadow-xl max-w-md w-full mx-auto text-center animate-pop-in">
                    <div className={`text-6xl mb-4 flex justify-center ${color}`}>
                        <Icons.Star className="w-24 h-24" fill="currentColor" />
                    </div>
                    <h2 className="text-2xl font-bold mb-2 text-slate-800">挑戰完成！</h2>
                    <p className={`text-lg font-bold mb-6 ${color}`}>{message}</p>
                    
                    <div className="bg-slate-50 rounded-xl p-6 mb-8 border border-slate-100">
                        <div className="text-slate-500 text-sm mb-1">總得分</div>
                        <div className="text-5xl font-black text-slate-800 mb-2">
                            {score} <span className="text-lg text-slate-400 font-medium">/ {total}</span>
                        </div>
                        <div className="text-slate-400 text-xs">答對題數</div>
                    </div>

                    <button 
                        onClick={onRestart}
                        className="w-full bg-indigo-600 text-white py-3 rounded-xl font-bold text-lg hover:bg-indigo-700 transition-all flex items-center justify-center gap-2"
                    >
                        <Icons.RefreshCw className="w-5 h-5" />
                        重新挑戰
                    </button>
                </div>
            );
        };

        // --- 主程式 App ---

        const App = () => {
          const [currentSetIndex, setCurrentSetIndex] = useState(0);
          const [revealedAnswers, setRevealedAnswers] = useState({});
          const [userResults, setUserResults] = useState({}); // 記錄每題是否答對 { "0-0": true, "0-1": false }
          const [showScore, setShowScore] = useState(false); // 是否顯示成績單

          const currentSet = problemSets[currentSetIndex];

          const toggleAnswer = (qIndex) => {
            const key = `${currentSetIndex}-${qIndex}`;
            setRevealedAnswers(prev => ({
              ...prev,
              [key]: !prev[key]
            }));
          };

          const markResult = (qIndex, isCorrect) => {
             const key = `${currentSetIndex}-${qIndex}`;
             setUserResults(prev => ({
                 ...prev,
                 [key]: isCorrect
             }));
          };

          const nextSet = () => {
            if (currentSetIndex < problemSets.length - 1) {
              setCurrentSetIndex(prev => prev + 1);
              window.scrollTo(0, 0);
            } else {
                // 完成所有題目，顯示成績
                setShowScore(true);
            }
          };

          const prevSet = () => {
            if (currentSetIndex > 0) {
              setCurrentSetIndex(prev => prev - 1);
              window.scrollTo(0, 0);
            }
          };

          const restartGame = () => {
              setCurrentSetIndex(0);
              setRevealedAnswers({});
              setUserResults({});
              setShowScore(false);
              window.scrollTo(0, 0);
          };

          const renderVisual = (type) => {
            switch (type) {
              case 'table_train': return <TrainTable />;
              case 'chart_potion': return <PotionChart />;
              case 'illustration_pizza': return <PizzaIllustration />;
              case 'map_farm': return <FarmMap />;
              case 'card_toys': return <ToyCards />;
              case 'chart_marathon': return <MarathonChart />;
              case 'card_recipe': return <RecipeCard />;
              case 'table_books': return <BooksTable />;
              case 'illustration_rpg': return <RPGInterface />;
              case 'chart_aquarium': return <AquariumChart />;
              default: return null;
            }
          };

          // 計算總分
          const totalQuestions = problemSets.reduce((acc, set) => acc + set.questions.length, 0);
          const currentScore = Object.values(userResults).filter(val => val === true).length;

          if (showScore) {
              return (
                  <div className="min-h-screen bg-slate-50 font-sans text-slate-800 flex flex-col justify-center items-center p-4">
                      <ScoreCard score={currentScore} total={totalQuestions} onRestart={restartGame} />
                      <footer className="mt-8 text-center text-slate-400 text-sm">
                        <p>由Bob 憶源老師設計 · 專為台灣小學三年級素養教學打造</p>
                      </footer>
                  </div>
              );
          }

          return (
            <div className="min-h-screen bg-slate-50 font-sans text-slate-800 pb-12 flex flex-col">
              <header className="bg-indigo-600 text-white p-4 sticky top-0 z-20 shadow-md">
                <div className="max-w-3xl mx-auto flex items-center justify-between">
                  <div className="flex items-center gap-2">
                    <Icons.Calculator className="w-6 h-6" />
                    <h1 className="text-lg md:text-xl font-bold">小小分析師挑戰賽</h1>
                  </div>
                  <div className="flex items-center gap-3">
                      <div className="hidden md:block text-xs font-medium bg-indigo-700 px-3 py-1 rounded-full whitespace-nowrap">
                        Level 3 - 進階數學
                      </div>
                      <div className="text-sm font-bold bg-white text-indigo-600 px-3 py-1 rounded-full">
                        得分: {currentScore}
                      </div>
                  </div>
                </div>
              </header>

              <main className="max-w-3xl mx-auto p-4 md:p-6 flex-grow w-full">
                <div className="flex items-center justify-between mb-6 text-sm text-slate-500">
                  <span>題組 {currentSetIndex + 1} / {problemSets.length}</span>
                  <div className="w-32 h-2 bg-slate-200 rounded-full overflow-hidden ml-4">
                    <div 
                      className="h-full bg-green-500 transition-all duration-300"
                      style={{ width: `${((currentSetIndex + 1) / problemSets.length) * 100}%` }}
                    ></div>
                  </div>
                </div>

                <div className="bg-white rounded-2xl shadow-lg border border-slate-100 overflow-hidden mb-8">
                  <div className="bg-slate-100 p-4 border-b border-slate-200 flex items-center gap-3">
                    <div className="p-2 bg-white rounded-lg shadow-sm text-indigo-600">
                      {currentSet.icon}
                    </div>
                    <h2 className="text-lg md:text-xl font-bold text-slate-800">{currentSet.title}</h2>
                  </div>
                  
                  <div className="p-4 md:p-6">
                    <div className="mb-6 text-base md:text-lg leading-relaxed text-slate-700">
                      {currentSet.story}
                    </div>

                    <VisualContainer title="分析圖表">
                      {renderVisual(currentSet.visualType)}
                    </VisualContainer>
                  </div>
                </div>

                <div className="space-y-6">
                  <h3 className="text-lg font-bold text-slate-800 flex items-center gap-2">
                    <Icons.HelpCircle className="w-5 h-5 text-indigo-500" />
                    請回答下列問題：
                  </h3>

                  {currentSet.questions.map((q, idx) => {
                    const isRevealed = revealedAnswers[`${currentSetIndex}-${idx}`];
                    const resultKey = `${currentSetIndex}-${idx}`;
                    const userAnswer = userResults[resultKey]; // true, false, or undefined

                    return (
                      <div key={idx} className="bg-white p-4 md:p-5 rounded-xl border border-slate-200 shadow-sm transition-all hover:shadow-md">
                        <div className="flex gap-4">
                          <div className={`flex-shrink-0 w-8 h-8 rounded-full flex items-center justify-center font-bold text-sm transition-colors ${
                              userAnswer === true ? 'bg-green-100 text-green-700' :
                              userAnswer === false ? 'bg-red-100 text-red-700' :
                              'bg-indigo-100 text-indigo-700'
                          }`}>
                            {userAnswer === true ? <Icons.CheckCircle className="w-5 h-5" /> :
                             userAnswer === false ? <Icons.XCircle className="w-5 h-5" /> :
                             idx + 1}
                          </div>
                          <div className="flex-grow">
                            <p className="font-medium text-slate-800 mb-4 text-base md:text-lg">{q.q}</p>
                            
                            <div className="relative">
                              <input 
                                type="text" 
                                placeholder="請在此輸入你的算式或答案..." 
                                className="w-full bg-slate-50 border border-slate-300 rounded-lg p-3 pr-10 focus:outline-none focus:ring-2 focus:ring-indigo-500 transition-all appearance-none text-slate-700"
                              />
                            </div>

                            <div className="mt-4 flex flex-col gap-3">
                              {/* 顯示/隱藏答案按鈕 */}
                              {!isRevealed ? (
                                  <button 
                                    onClick={() => toggleAnswer(idx)}
                                    className="text-sm px-4 py-2 rounded-lg font-medium transition-colors bg-indigo-50 text-indigo-600 hover:bg-indigo-100 flex items-center gap-2 w-full sm:w-auto justify-center sm:justify-start"
                                  >
                                    顯示參考答案
                                  </button>
                              ) : (
                                  <div className="animate-fade-in space-y-3">
                                      {/* 參考答案區塊 */}
                                      <div className="bg-green-50 text-green-800 px-4 py-3 rounded-lg text-sm border border-green-200">
                                        <div className="font-bold mb-1 flex items-center gap-2">
                                            <Icons.CheckCircle className="w-4 h-4" /> 參考解答：
                                        </div>
                                        {q.a}
                                      </div>

                                      {/* 自我評分區塊 - 只在未評分時顯示，或允許重新評分 */}
                                      <div className="flex items-center gap-3 pt-2 border-t border-slate-100">
                                          <span className="text-sm font-bold text-slate-500">你答對了嗎？</span>
                                          <button 
                                            onClick={() => markResult(idx, true)}
                                            className={`px-3 py-1.5 rounded-lg text-sm font-bold flex items-center gap-1 transition-all ${
                                                userAnswer === true 
                                                ? 'bg-green-500 text-white shadow-md transform scale-105' 
                                                : 'bg-slate-100 text-slate-500 hover:bg-green-100 hover:text-green-600'
                                            }`}
                                          >
                                            ✅ 答對了
                                          </button>
                                          <button 
                                            onClick={() => markResult(idx, false)}
                                            className={`px-3 py-1.5 rounded-lg text-sm font-bold flex items-center gap-1 transition-all ${
                                                userAnswer === false 
                                                ? 'bg-red-500 text-white shadow-md transform scale-105' 
                                                : 'bg-slate-100 text-slate-500 hover:bg-red-100 hover:text-red-600'
                                            }`}
                                          >
                                            💪 再接再厲
                                          </button>
                                      </div>
                                  </div>
                              )}
                            </div>
                          </div>
                        </div>
                      </div>
                    );
                  })}
                </div>

                <div className="flex justify-between mt-12 pt-6 border-t border-slate-200 gap-4">
                  <button 
                    onClick={prevSet}
                    disabled={currentSetIndex === 0}
                    className={`flex-1 flex items-center justify-center gap-2 px-4 py-3 rounded-xl font-bold transition-all ${
                      currentSetIndex === 0 
                      ? 'text-slate-300 cursor-not-allowed bg-slate-100' 
                      : 'bg-white border border-slate-300 text-slate-700 hover:bg-slate-50 shadow-sm active:scale-95'
                    }`}
                  >
                    <Icons.ChevronLeft className="w-5 h-5" />
                    上一題組
                  </button>

                  <button 
                    onClick={nextSet}
                    className={`flex-1 flex items-center justify-center gap-2 px-4 py-3 rounded-xl font-bold shadow-md transition-all ${
                      currentSetIndex === problemSets.length - 1
                      ? 'bg-green-600 text-white hover:bg-green-700 hover:shadow-lg active:scale-95'
                      : 'bg-indigo-600 text-white hover:bg-indigo-700 hover:shadow-lg active:scale-95'
                    }`}
                  >
                    {currentSetIndex === problemSets.length - 1 ? '送出成績' : '下一題組'}
                    <Icons.ChevronRight className="w-5 h-5" />
                  </button>
                </div>

              </main>

              <footer className="bg-slate-100 py-6 text-center text-slate-500 text-sm mt-8 border-t border-slate-200">
                <p>由Bob 憶源老師設計 · 專為台灣小學三年級素養教學打造</p>
                <p className="mt-1">© 2025 Math Adventure Learning</p>
              </footer>
            </div>
          );
        };

        const root = ReactDOM.createRoot(document.getElementById('root'));
        root.render(<App />);
    </script>
</body>
</html>
