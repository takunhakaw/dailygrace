<style>
    :root {
        --primary-gold: #c5a059;
        --bg-gradient: linear-gradient(135deg, #fdfcfb 0%, #e2d1c3 100%);
        --bot-bubble: #ffffff;
        --user-bubble: #e8f5e9;
        --text-color: #3e3e3e;
    }

    /* 關鍵 1：強制移除所有外距並防止水平溢出 */
    * {
        box-sizing: border-box;
        margin: 0;
        padding: 0;
        -webkit-tap-highlight-color: transparent;
    }

    html, body {
        width: 100%;
        height: 100%;
        /* 關鍵 2：固定住 body，防止手機瀏覽器橡皮筋特效導致內容跑位 */
        position: fixed;
        overflow: hidden;
        font-family: 'PingFang TC', 'Microsoft JhengHei', sans-serif;
    }

    #chat-window {
        /* 關鍵 3：寬度使用 100vw（螢幕全寬），高度使用 100dvh（動態全高） */
        width: 100vw;
        height: 100dvh; 
        background: var(--bg-gradient);
        display: flex;
        flex-direction: column;
    }

    header {
        background: var(--primary-gold);
        /* 關鍵 4：避開手機頂部瀏海、動態島區域 */
        padding-top: calc(env(safe-area-inset-top) + 15px);
        padding-bottom: 15px;
        color: white;
        text-align: center;
        font-size: 1.1rem;
        font-weight: bold;
        letter-spacing: 2px;
        box-shadow: 0 2px 8px rgba(0,0,0,0.1);
        flex-shrink: 0;
    }

    #chat-box {
        flex: 1;
        padding: 15px;
        overflow-y: auto;
        display: flex;
        flex-direction: column;
        gap: 12px;
        /* 關鍵 5：平滑滾動並確保背景色填滿 */
        background-color: transparent;
        -webkit-overflow-scrolling: touch;
    }

    .msg {
        /* 關鍵 6：訊息泡泡最大寬度設為 85%，確保不會貼邊 */
        max-width: 85%;
        padding: 12px 16px;
        border-radius: 18px;
        font-size: 1rem;
        line-height: 1.5;
        word-wrap: break-word; /* 避免長英文或網址撐開寬度 */
    }

    .bot {
        align-self: flex-start;
        background: var(--bot-bubble);
        border-bottom-left-radius: 4px;
        box-shadow: 2px 2px 5px rgba(0,0,0,0.05);
    }

    .user {
        align-self: flex-end;
        background: var(--user-bubble);
        color: #2e7d32;
        border-bottom-right-radius: 4px;
    }

    .interpretation {
        display: block;
        margin-top: 10px;
        font-size: 0.95rem;
        color: #555;
        background: #fffdf5;
        padding: 10px;
        border-left: 4px solid var(--primary-gold);
        border-radius: 4px;
    }

    .bible-ref {
        display: block;
        margin-top: 5px;
        font-size: 0.8rem;
        color: var(--primary-gold);
        text-align: right;
        font-weight: bold;
    }

    .input-area {
        background: white;
        padding: 12px 15px;
        /* 關鍵 7：避開手機底部的虛擬按鈕或手勢條區域 */
        padding-bottom: calc(env(safe-area-inset-bottom) + 12px);
        display: flex;
        gap: 10px;
        border-top: 1px solid #eee;
        flex-shrink: 0;
    }

    input {
        flex: 1;
        border: 1px solid #ddd;
        padding: 10px 15px;
        border-radius: 25px;
        outline: none;
        /* 關鍵 8：手機輸入框字體至少要 16px，否則 iOS 會強行放大畫面 */
        font-size: 16px;
    }

    button {
        background: var(--primary-gold);
        border: none;
        color: white;
        padding: 0 18px;
        border-radius: 20px;
        font-weight: bold;
        cursor: pointer;
    }
</style>
