<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>恩典之約 | 30日生命糧草</title>
    <style>
        :root {
            --primary-gold: #c5a059;
            --bg-gradient: linear-gradient(135deg, #fdfcfb 0%, #e2d1c3 100%);
            --bot-bubble: #ffffff;
            --user-bubble: #e8f5e9;
            --text-color: #3e3e3e;
        }

        body {
            font-family: 'PingFang TC', 'Microsoft JhengHei', sans-serif;
            background: var(--bg-gradient);
            height: 100vh;
            margin: 0;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        #chat-window {
            width: 500px;
            height: 90vh;
            background: rgba(255, 255, 255, 0.95);
            border-radius: 20px;
            box-shadow: 0 20px 60px rgba(0,0,0,0.15);
            display: flex;
            flex-direction: column;
            overflow: hidden;
        }

        header {
            background: var(--primary-gold);
            padding: 20px;
            text-align: center;
            font-size: 1.2rem;
            font-weight: bold;
            color: white;
            letter-spacing: 3px;
        }

        #chat-box {
            flex: 1;
            padding: 20px;
            overflow-y: auto;
            display: flex;
            flex-direction: column;
            gap: 15px;
            background-color: #fafafa;
        }

        .msg {
            max-width: 85%;
            padding: 15px 20px;
            border-radius: 20px;
            font-size: 1rem;
            line-height: 1.6;
            animation: slideIn 0.3s ease-out;
        }

        @keyframes slideIn {
            from { opacity: 0; transform: translateX(-10px); }
            to { opacity: 1; transform: translateX(0); }
        }

        .bot {
            align-self: flex-start;
            background: var(--bot-bubble);
            box-shadow: 3px 3px 10px rgba(0,0,0,0.05);
        }

        .user {
            align-self: flex-end;
            background: var(--user-bubble);
            color: #2e7d32;
        }

        .bible-ref {
            display: block;
            margin-top: 10px;
            font-size: 0.85rem;
            color: var(--primary-gold);
            font-weight: bold;
            border-top: 1px solid #f0f0f0;
            padding-top: 8px;
        }

        .interpretation {
            display: block;
            margin-top: 8px;
            font-size: 0.95rem;
            color: #666;
            background: #fff9eb;
            padding: 10px;
            border-left: 4px solid var(--primary-gold);
            border-radius: 4px;
        }

        .input-area {
            padding: 20px;
            background: white;
            display: flex;
            gap: 12px;
            border-top: 1px solid #eee;
        }

        input {
            flex: 1;
            border: 2px solid #f0f0f0;
            padding: 12px 20px;
            border-radius: 30px;
            outline: none;
        }

        button {
            background: var(--primary-gold);
            border: none;
            color: white;
            padding: 0 25px;
            border-radius: 30px;
            cursor: pointer;
            font-weight: bold;
        }

        small { color: #ccc; font-size: 0.7rem; }
    </style>
</head>
<body>

<div id="chat-window">
    <header>⛪ 恩典補給站 ⛪</header>
    <div id="chat-box">
        <div class="msg bot">
            平安！主正看著你的努力 🌿<br>
            不管現在心裡在想什麼，上帝都有話對你說。<br><br>
            請輸入 <b>/抽籤</b> 領取今日份的靈糧。
        </div>
    </div>
    <div class="input-area">
        <input type="text" id="userInput" placeholder="請輸入 /抽籤 ..." onkeypress="if(event.key==='Enter')handleSend()">
        <button onclick="handleSend()">阿們</button>
    </div>
</div>

<script>
    const fortunes = [
        {
            verse: "📖 **【安息籤】** 凡勞苦擔重擔的人可以到我這裡來，我就使你們得安息。🕊️",
            ref: "馬太福音 11:28",
            interp: "主知道你為了工作精疲力竭。現在請放下手中的焦慮，在祂的平安中好好喘口氣吧。"
        },
        {
            verse: "🕊️ **【引導籤】** 你要專心仰賴耶和華，不可倚靠自己的聰明。🧭",
            ref: "箴言 3:5",
            interp: "當你卡在決策中摸不著頭緒時，試著把主權交給主，祂會指引你最清晰、最正確的路。"
        },
        {
            verse: "🌈 **【喜樂籤】** 這是耶和華所定的日子，我們在其中要高興歡喜！☀️",
            ref: "詩篇 118:24",
            interp: "不管今天遇到什麼事，都是上帝賜予的禮物。試著用笑容迎接挑戰，喜樂就是你的力量。"
        },
        {
            verse: "🛡️ **【同在籤】** 我與你同在，你無論往哪裡去，我必保佑你。🧱",
            ref: "創世記 28:15",
            interp: "在壓力巨大的辦公室裡，你並不孤單。主就在你身旁桌邊，時刻守護著你的心靈。"
        },
        {
            verse: "🦅 **【更新籤】** 你的青春必如鷹返老還童。🚀",
            ref: "詩篇 103:5",
            interp: "覺得心累了嗎？主正為你注入屬天的活力，讓你重新得力，像鷹一樣在高空無憂展翅。"
        },
        {
            verse: "🕯️ **【話語籤】** 你的話是我腳前的燈，是我路上的光。✨",
            ref: "詩篇 119:105",
            interp: "即使前路模糊不清，主的話語會像微光照亮你當下這一步。別怕，專注走好眼前的路。"
        },
        {
            verse: "🍎 **【修剪籤】** 凡結果子的，祂就修理乾淨，使枝子結果子更多。🌱",
            ref: "約約翰福音 15:2",
            interp: "現在的磨練雖然疼痛，卻是上帝在修剪雜質，為了讓你未來的生命結出更豐盛、甜美的果實。"
        },
        {
            verse: "🧱 **【穩固籤】** 耶和華是我的巖石，我的山寨，我的救主。🏔️",
            ref: "詩篇 18:2",
            interp: "職場環境雖然瞬息萬變，但主的愛與應許是你最穩固的堡壘，沒有任何困難能動搖你。"
        },
        {
            verse: "🎐 **【平安籤】** 我將我的平安賜給你們。我所賜的不像世人所賜的。🌊",
            ref: "約翰福音 14:27",
            interp: "外界的雜訊雖吵雜，但主賜的平安由心而生。深呼吸，領受這份超脫環境的安穩力量。"
        },
        {
            verse: "🦁 **【勇敢籤】** 你當剛強壯膽！不要懼難，因為耶和華你的神必與你同在。⚔️",
            ref: "約書亞記 1:9",
            interp: "主已經為你勝過了世界！帶著祂賜予的勇氣，昂首阔步地面對困難，你必能得勝有餘。"
        },
        {
            verse: "💎 **【價值籤】** 你在耶和華眼中看為寶貝，為尊貴。💖",
            ref: "以賽亞書 43:4",
            interp: "你的價值不在於績效高低，而在於你是上帝重價買贖的寶貝。你是被深愛且無可取代的。"
        },
        {
            verse: "💧 **【滋潤籤】** 耶和華必時常引導你，在乾旱之地使你心滿意足。🌷",
            ref: "以賽亞書 58:11",
            interp: "即使身處枯燥忙碌的職場，主也會像甘泉般滋潤你的靈魂，讓你在壓力中依然靈裡飽足。"
        },
        {
            verse: "⚓ **【盼望籤】** 忍受試煉的人是有福的，因為他必得生命的冠冕。👑",
            ref: "雅各書 1:12",
            interp: "現在的忍耐並非徒然，主正看著你的堅持。等候祂的時間到來，你必會領受應許的榮耀。"
        },
        {
            verse: "⚖️ **【交託籤】** 當將你的事交託耶和華，並倚靠祂，祂就必成全。🤲",
            ref: "詩篇 37:5",
            interp: "別再獨自扛著重擔徹夜難眠了。試著完全放手交給主，祂會以最完美的智慧為你成就。"
        },
        {
            verse: "🍭 **【良藥籤】** 憂心忡忡使人心沉重，一句良言使人心歡愉。👏",
            ref: "箴言 12:25",
            interp: "主今天想親口對你說聲：「辛苦了，我的孩子。」讓這句溫暖的話醫治你一整天的疲憊。"
        },
        {
            verse: "🌿 **【謙卑籤】** 凡自高的，必降為卑；自卑的，必升為高。🌸",
            ref: "馬太福音 23:12",
            interp: "保持一顆柔軟謙卑的心。當你願意退後一步尊榮他人時，上帝必會在最適當的時刻提拔你。"
        },
        {
            verse: "🏗️ **【供應籤】** 神能照著運行在我們心裡的大力，充充足足地成就一切。🎁",
            ref: "以弗所書 3:20",
            interp: "上帝的恩典永遠超過你的想像！別用有限的視野限制了主，祂正預備超乎尋求的驚喜給你。"
        },
        {
            verse: "👂 **【垂聽籤】** 我曾尋求耶和華，祂就應允我，救我脫離了一切的恐懼。⛅",
            ref: "詩篇 34:4",
            interp: "你的每一聲嘆息和禱告，主都沒有漏掉。把恐懼告訴祂，祂會用大能的雙手抱緊你。"
        },
        {
            verse: "🤝 **【彼此相愛籤】** 你們若有彼此相愛的心，眾人因此就認出你們是我的門徒了。😊",
            ref: "約翰福音 13:35",
            interp: "今天試著成為辦公室裡的祝福。當你遞出一杯溫水或一個微笑，就是活出主愛最好的證明。"
        },
        {
            verse: "☀️ **【光亮籤】** 你們是世上的光。讓主的光透過你的良善照亮辦公室吧。💡",
            ref: "馬太福音 5:14",
            interp: "你的專業、誠實與溫柔，就是上帝照進職場的光。別小看自己，你正在影響身邊的人。"
        },
        {
            verse: "🦋 **【忍耐籤】** 患難生忍耐，忍耐生老練，老練生盼望。⏳",
            ref: "羅馬書 5:3-4",
            interp: "這場挑戰是靈魂的健身房。過程雖然吃力，卻正塑造你成為更成熟、更有盼望的精兵。"
        },
        {
            verse: "🍇 **【果實籤】** 聖靈所結的果子，就是仁愛、喜樂、和平、忍耐、恩慈。🧺",
            ref: "加拉太書 5:22",
            interp: "祈求聖靈在你心裡動工，讓你即便在急躁的環境中，也能流露出平靜安穩的屬靈氣質。"
        },
        {
            verse: "🍞 **【生命糧籤】** 人活著，不是單靠食物，乃是靠神口裡所出的一切話。🍱",
            ref: "馬太福音 4:4",
            interp: "薪水飽足了肚子，但神的話語飽足了心靈。讓今天的經文成為你最營養的精神午餐吧。"
        },
        {
            verse: "⛪ **【聖殿籤】** 豈不知你們的身子就是聖靈的殿嗎？🍎",
            ref: "哥林多前書 6:19",
            interp: "別為了趕進度而犧牲健康。主看重你的靈魂，也珍惜你的身體，今天請早點休息吧。"
        },
        {
            verse: "🛤️ **【信心籤】** 因為我們行事為人是憑著信心，不是憑著眼見。👣",
            ref: "哥林多後書 5:7",
            interp: "雖然眼前的結果還不明朗，但信心就是相信上帝正在背後動工。繼續憑信心邁出下一步。"
        },
        {
            verse: "🍃 **【輕省籤】** 我的軛是容易的，我的擔子是輕省的。☁️",
            ref: "馬太福音 11:30",
            interp: "主不要你獨自埋頭苦幹。與祂同負一軛，你會發現原本沉重的任務，竟變得如此奇妙輕省。"
        },
        {
            verse: "🏹 **【得勝籤】** 感謝神，使我們藉著我們的主耶穌基督得勝。🏆",
            ref: "哥林多前書 15:57",
            interp: "所有的挫折都只是得勝的前奏。依靠主的恩典，你已經站在得勝的那一方，勇敢前進吧！"
        },
        {
            verse: "💎 **【智慧籤】** 若有缺少智慧的，應當求那厚賜與眾人的神，祂必賜給他。🎓",
            ref: "雅各書 1:5",
            interp: "遇到棘手的難題了嗎？現在就向主祈求，祂會將超然的洞察力賜給你，讓你迎刃而解。"
        },
        {
            verse: "💝 **【不息籤】** 愛是永不止息。上帝的愛依然環繞著你，溫暖你的心。🕯️",
            ref: "哥林多前書 13:8",
            interp: "即使世界冷漠，上帝的愛也永不落空。讓這份愛成為你的盔甲，保護你度過低谷。"
        },
        {
            verse: "🌠 **【結局籤】** 我為你們所懷的意念是賜平安的意念，要叫你們末後有指望。🌟",
            ref: "耶利米書 29:11",
            interp: "上帝對你的計畫從不是傷害。請放心相信，祂正引領你走向一個充滿光明的祝福終點。"
        }
    ];

    function handleSend() {
        const input = document.getElementById('userInput');
        const chatBox = document.getElementById('chat-box');
        const text = input.value.trim();

        if (text === "") return;

        appendMsg('user', text);
        input.value = "";

        setTimeout(() => {
            if (text === "/抽籤") {
                const randomIndex = Math.floor(Math.random() * fortunes.length);
                const item = fortunes[randomIndex];
                const content = `
                    🙏 <b>為你領受的今日恩典：</b><br><br>
                    ${item.verse}<br>
                    <span class='interpretation'>💡 <b>【解籤】</b><br>${item.interp}</span>
                    <span class='bible-ref'>— ${item.ref}</span>
                `;
                appendMsg('bot', content + `<br><small>(第 ${randomIndex + 1} 號籤碼)</small>`);
            } else {
                appendMsg('bot', "主聽見你的聲音了！請輸入 <b>/抽籤</b>，讓祂的話語成為你今日的指引 🌿");
            }
        }, 700);
    }

    function appendMsg(type, content) {
        const chatBox = document.getElementById('chat-box');
        const msgDiv = document.createElement('div');
        msgDiv.className = `msg ${type}`;
        msgDiv.innerHTML = content;
        chatBox.appendChild(msgDiv);
        chatBox.scrollTop = chatBox.scrollHeight;
    }
</script>

</body>
</html>
