<template>
    <div class="chat-widget-container">
        <transition name="fade">
            <div v-if="!isOpen && showWelcome" class="welcome-tooltip shadow-lg">
                Chào mừng đến với Tr0ondCinema! <br /> Bạn cần hỏi lịch chiếu hay đặt vé?
                <button class="close-welcome" @click.stop="showWelcome = false">×</button>
            </div>
        </transition>

        <div class="chat-toggle shadow-lg" @click="toggleChat" :class="{ 'is-open': isOpen }">
            <i v-if="!isOpen" class="fa-solid fa-robot"></i>
            <i v-else class="fa-solid fa-xmark"></i>
        </div>

        <transition name="fade-slide">
            <div v-show="isOpen" class="chat-box shadow-lg">
                <div class="chat-messages" ref="messagesContainer">
                    <div class="msg-wrapper is-bot">
                        <div class="bot-avatar me-2"><i class="fa-solid fa-robot"></i></div>
                        <div class="msg-bubble shadow-sm">
                            Xin chào! Tôi là trợ lý ảo của Tr0ondCinema. Tôi có thể giúp bạn tìm thông tin phim, đặt vé
                            hoặc giải đáp thắc mắc. Bạn cần tôi giúp gì?
                        </div>
                    </div>

                    <div v-for="(msg, index) in chatHistory" :key="index" class="msg-wrapper"
                        :class="msg.role === 'user' ? 'is-user' : 'is-bot'">
                        
                        <div v-if="msg.role === 'model'" class="bot-avatar me-2">
                            <i class="fa-solid fa-robot"></i>
                        </div>

                        <div class="msg-bubble shadow-sm" :class="msg.role === 'model' ? 'markdown-body' : ''"
                            v-html="msg.role === 'model' ? renderMarkdown(msg.text) : msg.text">
                        </div>
                    </div>

                    <div v-if="isTyping" class="msg-wrapper is-bot mt-2">
                        <div class="bot-avatar me-2"><i class="fa-solid fa-robot"></i></div>
                        <div class="typing-indicator msg-bubble shadow-sm">
                            <span></span><span></span><span></span>
                        </div>
                    </div>
                </div>

                <div class="chat-input">
                    <input type="text" v-model="inputText" @keypress.enter="sendMessage"
                        placeholder="Hỏi tôi bất cứ điều gì..." autocomplete="off" :disabled="isTyping" />
                    <button class="send-btn" @click="sendMessage" :disabled="!inputText.trim() || isTyping">
                        <i class="fa-solid fa-paper-plane"></i>
                    </button>
                </div>
            </div>
        </transition>
    </div>
</template>

<script setup>
import { ref, nextTick, onMounted } from "vue";
import { marked } from "marked";

const isOpen = ref(false);
const showWelcome = ref(true);
const chatHistory = ref([]); // Dùng để hiển thị lên UI
const inputText = ref("");
const isTyping = ref(false);
const messagesContainer = ref(null);

// Mảng lưu trữ lịch sử hội thoại chuẩn định dạng của Gemini API
const conversations = ref([]);

// Cấu hình thông tin huấn luyện (System Instruction)
// Bạn có thể thay đổi text này sang bối cảnh Phòng Khám như file cũ nếu cần
const thongtinHuanluyen = `
    🎬 1. Thông tin chung về rạp chiếu phim

        Tên rạp: Tr0ond Cinema

        Địa chỉ: Tầng 5, Vincom Plaza, TP Đà Nẵng

        Số điện thoại hotline: 0909 888 999

        Email: cskh@tr0ondcinema.vn

        Giờ hoạt động: 
            Thứ 2 - Chủ nhật: 8h00 - 23h30
            (Mở cửa xuyên các ngày Lễ/Tết)

    🎫 2. Bảng giá vé & Dịch vụ bắp nước
    STT	| Tên dịch vụ | Giá tiền | Mô tả ngắn
    1 | Vé 2D (Tiêu chuẩn) | 80.000đ | Vé xem phim 2D ghế thường
    2 | Vé 3D | 120.000đ | Kính 3D được phát tại rạp
    3 | Vé Ghế Đôi (Sweetbox) | 180.000đ | Ghế đôi riêng tư dành cho 2 người
    4 | Vé Học sinh/Sinh viên | 65.000đ | Áp dụng khi xuất trình thẻ HSSV (chỉ thứ 2-thứ 5)
    5 | Combo Solo (1 bắp + 1 nước) | 75.000đ | Tiết kiệm hơn mua lẻ
    6 | Combo Couple (1 bắp lớn + 2 nước) | 115.000đ | Phù hợp cho 2 người (được chọn vị bắp phô mai/caramel)

    🎞️ 3. Danh sách phim đang chiếu & Lịch chiếu
    Tên phim | Thể loại | Thời lượng | Lịch chiếu nổi bật
    Dune: Hành Tinh Cát 2 | Hành động, Viễn tưởng | 166 phút | 09:00, 14:00, 19:30, 22:00
    Kung Fu Panda 4 | Hoạt hình, Hài hước | 94 phút | 08:30, 10:45, 13:00, 16:30
    Godzilla x Kong: Đế Chế Mới | Hành động, Kỹ xảo | 115 phút | 11:00, 15:30, 18:45, 21:15
    Exhuma: Quật Mộ Trùng Ma | Kinh dị, Giật gân | 134 phút | 18:00, 20:30, 23:45 (Phim T16)

    🔄 4. Chính sách đặt vé & Quy định rạp

        Cách đặt vé: Khách hàng có thể đặt vé qua Website, Fanpage hoặc mua trực tiếp tại quầy vé.

        Chính sách hoàn/hủy vé: Rạp KHÔNG hỗ trợ hoàn tiền hoặc đổi suất chiếu sau khi đã thanh toán thành công. Khách hàng vui lòng kiểm tra kỹ thông tin trước khi thanh toán.

        Quy định độ tuổi: Khán giả vui lòng mang theo CCCD/Thẻ HSSV để nhân viên kiểm tra đối với các phim có dán nhãn phân loại độ tuổi (T13, T16, T18). Rạp có quyền từ chối cho vào phòng chiếu nếu không đúng độ tuổi.

        Hình thức thanh toán: Chấp nhận Tiền mặt, Thẻ ATM/Visa, Momo, ZaloPay và VNPay.
`;

onMounted(() => {
    // Ẩn lời chào sau 10s
    setTimeout(() => { showWelcome.value = false; }, 10000);
});

const toggleChat = () => {
    isOpen.value = !isOpen.value;
    if (isOpen.value) showWelcome.value = false;
    scrollToBottom();
};

const scrollToBottom = async () => {
    await nextTick();
    if (messagesContainer.value) {
        messagesContainer.value.scrollTo({
            top: messagesContainer.value.scrollHeight,
            behavior: "smooth"
        });
    }
};

const renderMarkdown = (text) => {
    return marked.parse(text || "");
};

const sendMessage = async () => {
    const text = inputText.value.trim();
    if (!text) return;

    // 1. Cập nhật tin nhắn của User lên UI
    chatHistory.value.push({ role: "user", text: text });
    
    // 2. Thêm vào mảng lịch sử gửi lên Gemini
    conversations.value.push({ role: "user", parts: [{ text: text }] });

    inputText.value = "";
    isTyping.value = true;
    scrollToBottom();

    // 3. Tạo một object tin nhắn rỗng của Model trên UI để đón luồng chữ (streaming)
    const currentModelIndex = chatHistory.value.length;
    chatHistory.value.push({ role: "model", text: "" });

    try {
        // Lưu ý: Đảm bảo biến môi trường chứa API Key đúng tên
        const apiKey = import.meta.env.VITE_GEMINI_API_KEY; 

        // GỌI API STREAMING GIỐNG FILE chatbotai.js
        const response = await fetch(
            `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:streamGenerateContent?alt=sse&key=${apiKey}`,
            {
                method: "POST",
                headers: {
                    "Content-Type": "application/json",
                },
                body: JSON.stringify({
                    system_instruction: {
                        parts: [{ text: thongtinHuanluyen }],
                    },
                    contents: conversations.value,
                }),
            }
        );

        // Đã có kết nối trả về, tắt hiệu ứng typing
        isTyping.value = false;

        const reader = response.body
            .pipeThrough(new TextDecoderStream("utf-8"))
            .getReader();

        let fullResponseText = "";

        // Xử lý stream (từng chunk)
        while (true) {
            const { value, done } = await reader.read();
            if (done) break;

            // Tách theo chữ "data: " ra mảng
            const arr = value.split("data: ");
            arr.forEach((item, idx) => {
                if (idx > 0) {
                    try {
                        // Parse JSON từ chunk
                        const parsedData = JSON.parse(item);
                        if (parsedData.candidates && parsedData.candidates[0].content) {
                            const chunkText = parsedData.candidates[0].content.parts[0].text;
                            
                            // Nối văn bản vào biến tổng
                            fullResponseText += chunkText;
                            
                            // Cập nhật UI ngay lập tức (Vue sẽ tự render lại)
                            chatHistory.value[currentModelIndex].text = fullResponseText;
                            scrollToBottom();
                        }
                    } catch (err) {
                        // Bỏ qua lỗi parse JSON nếu bị cắt dở chunk
                    }
                }
            });
        }

        // 4. Khi stream hoàn tất, lưu toàn bộ câu trả lời của Bot vào lịch sử Gemini
        // 4. Khi stream hoàn tất, kiểm tra xem Bot có trả lời không
        if (fullResponseText.trim() !== "") {
            // Có chữ -> Lưu vào lịch sử bình thường
            conversations.value.push({
                role: "model",
                parts: [{ text: fullResponseText }],
            });
        } else {
            // Rỗng -> Do mạng lag hoặc API lỗi không trả về dữ liệu
            
            // Ghi câu báo lỗi ra màn hình chat
            chatHistory.value[currentModelIndex].text = "Xin lỗi, tôi bị mất kết nối hoặc không thể trả lời câu này. Bạn thử lại nhé!";
            
            // XÓA câu hỏi vừa rồi của user khỏi lịch sử Gemini để tránh bị lệch pha (bắt buộc phải theo thứ tự User -> Model -> User -> Model)
            conversations.value.pop();
        }

    } catch (error) {
        console.error("Lỗi khi gọi Gemini API:", error);
        chatHistory.value[currentModelIndex].text = "Xin lỗi, hệ thống đang bận. Vui lòng thử lại sau.";
        
        // Bắt buộc xóa câu hỏi của user nếu API sập để không phá vỡ logic lịch sử
        if (conversations.value.length > 0 && conversations.value[conversations.value.length - 1].role === "user") {
            conversations.value.pop();
        }
    } finally {
        scrollToBottom();
    }
};
</script>

<style scoped>
.chat-widget-container { position: fixed; bottom: 30px; right: 30px; z-index: 999999 !important; font-family: 'Roboto', sans-serif; display: flex; flex-direction: column; align-items: flex-end; }
.welcome-tooltip { position: absolute; bottom: 85px; right: 10px; background: #fff; color: #0F122B; padding: 12px 35px 12px 15px; border-radius: 12px; font-size: 13px; font-weight: 500; line-height: 1.5; box-shadow: 0 10px 25px rgba(0, 0, 0, 0.3); border: 2px solid #E3CC81; width: max-content; animation: bounce 2s infinite; }
.welcome-tooltip::after { content: ''; position: absolute; bottom: -8px; right: 20px; border-width: 8px 8px 0; border-style: solid; border-color: #E3CC81 transparent transparent transparent; }
.close-welcome { position: absolute; top: 5px; right: 8px; background: none; border: none; color: #888; font-size: 18px; cursor: pointer; line-height: 1; }
@keyframes bounce { 0%, 100% { transform: translateY(0); } 50% { transform: translateY(-8px); } }
.chat-toggle { width: 65px; height: 65px; border-radius: 50%; background: linear-gradient(135deg, #E3CC81 0%, #d4b455 100%); color: #0F122B; display: flex; justify-content: center; align-items: center; cursor: pointer; transition: all 0.3s ease; font-size: 28px; border: 2px solid #fff; }
.chat-toggle:hover { transform: scale(1.1) translateY(-5px); }
.chat-toggle.is-open { background: #191970; color: #E3CC81; border-color: #E3CC81; transform: scale(0.9); }
.chat-box { width: 380px; height: 550px; background: #0F122B; border-radius: 16px; display: flex; flex-direction: column; overflow: hidden; border: 1px solid rgba(227, 204, 129, 0.5); position: absolute; bottom: 85px; right: 0; }
.fade-slide-enter-active, .fade-slide-leave-active { transition: all 0.3s cubic-bezier(0.25, 0.8, 0.25, 1); transform-origin: bottom right; }
.fade-slide-enter-from, .fade-slide-leave-to { opacity: 0; transform: scale(0.8) translateY(30px); }
.fade-enter-active, .fade-leave-active { transition: opacity 0.5s; }
.fade-enter-from, .fade-leave-to { opacity: 0; }
.chat-messages { flex: 1; padding: 20px; overflow-y: auto; background: url('https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSLyg8w7yRqHIAFBkTzMPy_TumLgDvMG66dXw&s') center/cover; box-shadow: inset 0 0 0 2000px rgba(15, 18, 43, 0.92); display: flex; flex-direction: column; gap: 16px; }
.chat-messages::-webkit-scrollbar { width: 5px; }
.chat-messages::-webkit-scrollbar-thumb { background-color: rgba(227, 204, 129, 0.5); border-radius: 10px; }
.msg-wrapper { display: flex; width: 100%; align-items: flex-end; }
.msg-wrapper.is-user { justify-content: flex-end; }
.msg-wrapper.is-bot { justify-content: flex-start; }
.bot-avatar { width: 32px; height: 32px; border-radius: 50%; background: #E3CC81; color: #0F122B; display: flex; justify-content: center; align-items: center; font-size: 14px; border: 2px solid #0F122B; margin-bottom: 5px; }
.msg-bubble { border-radius: 18px; padding: 12px 16px; max-width: 80%; line-height: 1.5; font-size: 14.5px; word-wrap: break-word; }
.is-user .msg-bubble { background: linear-gradient(135deg, #E3CC81 0%, #d4b455 100%); color: #0F122B; font-weight: 500; border-bottom-right-radius: 4px; }
.is-bot .msg-bubble { background: rgba(25, 25, 112, 0.85); border: 1px solid rgba(227, 204, 129, 0.3); color: #f0f0f0; border-bottom-left-radius: 4px; }
:deep(.markdown-body p) { margin-bottom: 8px; }
:deep(.markdown-body p:last-child) { margin-bottom: 0; }
:deep(.markdown-body ul) { margin-bottom: 0; padding-left: 20px; }
:deep(.markdown-body strong) { color: #E3CC81; }
:deep(.markdown-body a) { color: #87CEFA; text-decoration: underline; }
.typing-indicator span { display: inline-block; width: 6px; height: 6px; background-color: #E3CC81; border-radius: 50%; margin: 0 2px; animation: typing 1.4s infinite ease-in-out both; }
.typing-indicator span:nth-child(1) { animation-delay: -0.32s; }
.typing-indicator span:nth-child(2) { animation-delay: -0.16s; }
@keyframes typing { 0%, 80%, 100% { transform: scale(0); } 40% { transform: scale(1); } }

/* KHUNG INPUT */
.chat-input { display: flex; gap: 12px; padding: 15px; border-top: 1px solid rgba(227, 204, 129, 0.3); background: #0F122B; }
.chat-input input { flex: 1; background: rgba(255, 255, 255, 0.05); border: 1px solid rgba(227, 204, 129, 0.3); border-radius: 999px; color: #fff; padding: 12px 20px; outline: none; font-size: 14px; transition: 0.3s; }
.chat-input input:focus { border-color: #E3CC81; box-shadow: 0 0 0 3px rgba(227, 204, 129, 0.15); background: rgba(255, 255, 255, 0.1); }
.chat-input input::placeholder { color: rgba(255, 255, 255, 0.8); opacity: 1; }
.chat-input input::-webkit-input-placeholder { color: rgba(255, 255, 255, 0.8); }
.chat-input input::-moz-placeholder { color: rgba(255, 255, 255, 0.8); opacity: 1; }
.send-btn { width: 46px; height: 46px; border: none; border-radius: 50%; background: linear-gradient(135deg, #E3CC81 0%, #d4b455 100%); color: #0F122B; cursor: pointer; transition: 0.2s; display: flex; justify-content: center; align-items: center; font-size: 16px; }
.send-btn:hover:not(:disabled) { transform: scale(1.1); box-shadow: 0 0 15px rgba(227, 204, 129, 0.5); }
.send-btn:disabled { background: rgba(255, 255, 255, 0.1); color: rgba(255, 255, 255, 0.3); cursor: not-allowed; }
</style>