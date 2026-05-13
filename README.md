<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Get My User ID</title>
    <!-- ใช้ CSS เล็กน้อยเพื่อให้ปุ่มดูสวยงาม -->
    <style>
        body { font-family: sans-serif; text-align: center; padding: 20px; background-color: #f4f4f4; }
        .card { background: white; padding: 20px; border-radius: 15px; box-shadow: 0 4px 6px rgba(0,0,0,0.1); }
        .user-id { background: #eee; padding: 10px; border-radius: 5px; word-break: break-all; font-family: monospace; margin: 15px 0; display: block; }
        button { background-color: #ff6600; color: white; border: none; padding: 10px 20px; border-radius: 5px; cursor: pointer; font-size: 16px; }
        button:active { background-color: #e65c00; }
    </style>
</head>
<body>

    <div class="card">
        <h2>รหัส User ID ของคุณ</h2>
        <span id="userIdText" class="user-id">กำลังโหลด...</span>
        <button onclick="copyUserId()">คัดลอกรหัส</button>
        <p id="msg" style="color: green; display: none;">คัดลอกลง Clipboard แล้ว!</p>
    </div>

    <!-- 1. โหลด LIFF SDK -->
    <script src="https://static.line-scdn.net/liff/edge/2/sdk.js"></script>
    <script>
        async function initializeLiff() {
            try {
                // 2. ใส่ LIFF ID ของคุณที่นี่
                await liff.init({ liffId: "YOUR_LIFF_ID" });

                if (liff.isLoggedIn()) {
                    const context = liff.getContext();
                    if (context && context.userId) {
                        document.getElementById("userIdText").innerText = context.userId;
                    }
                } else {
                    liff.login();
                }
            } catch (error) {
                console.error("LIFF Initialization failed", error);
                document.getElementById("userIdText").innerText = "เกิดข้อผิดพลาดในการโหลด";
            }
        }

        // 3. ฟังก์ชันสำหรับคัดลอกข้อความ
        function copyUserId() {
            const idText = document.getElementById("userIdText").innerText;
            navigator.clipboard.writeText(idText).then(() => {
                const msg = document.getElementById("msg");
                msg.style.display = "block";
                setTimeout(() => { msg.style.display = "none"; }, 2000);
            });
        }

        initializeLiff();
    </script>
</body>
</html>
