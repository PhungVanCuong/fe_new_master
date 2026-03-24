<template>
    <div class="wrapper client-layout">
        <MenuClient></MenuClient>
        <div class="body-content">
            <router-view></router-view>
        </div>
        <BotClient></BotClient>
    </div>
</template>

<script>
// Giữ nguyên phần script của bạn
import MenuClient from "../components/Client/MenuClient.vue";
import BotClient from "../components/Client/BotClient.vue";
import "../../assets/js/bootstrap.bundle.min.js";
import "../../assets/js/jquery.min.js";
import "../../assets/plugins/simplebar/js/simplebar.min.js";
import "../../assets/plugins/metismenu/js/metisMenu.min.js";
import "../../assets/plugins/perfect-scrollbar/js/perfect-scrollbar.js";
import "../../assets/js/index.js";
import "../../assets/js/app.js";
import "../../assets/js/pace.min.js";

export default {
    name: "app",
    components: {
        MenuClient, BotClient
    }
}
</script>

<style>
/* 1. QUAN TRỌNG: Đưa tất cả @import lên đầu tiên */
@import "../../assets/plugins/simplebar/css/simplebar.css";
@import "../../assets/plugins/perfect-scrollbar/css/perfect-scrollbar.css";
@import "../../assets/plugins/metismenu/css/metisMenu.min.css";
@import "../../assets/css/pace.min.css";
@import "../../assets/css/bootstrap.min.css";
@import "../../assets/css/bootstrap-extended.css";
@import url("https://fonts.googleapis.com/css2?family=Roboto:wght@400;500&display=swap");
@import "../../assets/css/app.css";
@import "../../assets/css/icons.css";
@import "../../assets/css/dark-theme.css";
@import "../../assets/css/semi-dark.css";
@import "../../assets/css/header-colors.css";
@import url("https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.2/css/all.min.css");

/* 2. Viết CSS tùy chỉnh ở DƯỚI CÙNG để ghi đè các thư viện trên */
.client-layout {
    font-size: 14px;
    font-weight: 400;
    line-height: 22px;
    
    /* Ảnh nền */
    background: url("https://starlight.vn/Content/img/bgs6.jpg");
    
    /* Sửa thành 'cover' để ảnh phủ kín màn hình đẹp hơn 'contain' */
    background-size: cover; 
    background-attachment: fixed; /* Giữ ảnh đứng yên khi cuộn */
    background-position: center;
    
    box-shadow: 0 0 10px rgb(0 0 0 / 84%);
    letter-spacing: 1.5px;
    
    /* Đảm bảo chiều cao full màn hình */
    min-height: 100vh;
    width: 100%;

    /* --- THÊM 2 DÒNG NÀY ĐỂ KÍCH HOẠT FLEXBOX --- */
    display: flex;
    flex-direction: column;
}

.body-content {
    flex: 1; /* Lệnh này bắt phần nội dung (router-view) tự động giãn hết mức có thể, đẩy BotClient xuống dưới cùng */
}
</style>