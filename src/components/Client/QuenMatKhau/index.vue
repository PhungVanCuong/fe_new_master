<template>
    <div class="section-authentication-signin d-flex align-items-center justify-content-center my-5 my-lg-0" style="min-height: 100vh;">
        <div class="container">
            <div class="row row-cols-1 row-cols-lg-2 row-cols-xl-2">
                <div class="col mx-auto">
                    <div class="card glass-card border-0 shadow-lg">
                        <div class="card-body p-4 p-sm-5">
                            <div class="form-body">
                                
                                <div class="text-center mb-4">
                                    <div class="mb-3 d-inline-block p-3 rounded-circle" style="background-color: rgba(227, 204, 129, 0.1);">
                                        <i class="fa-solid fa-key fs-1 text-warning" style="color: #E3CC81 !important;"></i>
                                    </div>
                                    <h4 class="mt-2 fw-bold text-white">Quên Mật Khẩu?</h4>
                                    <p class="text-light-50 fs-6">Đừng lo! Nhập email của bạn và chúng tôi sẽ gửi liên kết để đặt lại mật khẩu.</p>
                                </div>
                                
                                <div class="my-4">
                                    <label class="form-label text-white-50 small mb-1">Địa chỉ Email</label>
                                    <div class="input-group">
                                        <span class="input-group-text custom-input-bg border-end-0 text-white-50">
                                            <i class="fa-regular fa-envelope"></i>
                                        </span>
                                        <input v-model="email" 
                                               @keyup.enter="guiMail"
                                               type="email" 
                                               class="form-control custom-input-bg border-start-0 text-white form-control-lg fs-6" 
                                               placeholder="Nhập email tài khoản của bạn">
                                    </div>
                                </div>
                                
                                <div class="d-grid gap-3 mt-4">
                                    <button v-on:click="guiMail()" class="btn btn-gradient btn-lg fw-bold text-white d-flex justify-content-center align-items-center gap-2">
                                        Gửi Liên Kết <i class="fa-regular fa-paper-plane"></i>
                                    </button>
                                    
                                    <router-link to="/client/dang-nhap" class="text-decoration-none">
                                        <button class="btn btn-outline-light w-100 btn-lg fs-6 d-flex justify-content-center align-items-center gap-2" style="background-color: rgba(255,255,255,0.05); border-color: rgba(255,255,255,0.2);">
                                            <i class="fa-solid fa-arrow-left"></i> Quay lại đăng nhập
                                        </button>
                                    </router-link>
                                </div>

                            </div>
                        </div>
                    </div>
                    
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
import apiUrl from '../../../utils/api';

export default {
    data() {
        return {
            email: null
        }
    },
    methods: {
        guiMail() {
            // Optional: Thêm một loading state ở đây nếu muốn hiển thị vòng xoay chờ gửi mail
            
            var payload = {
                email: this.email
            };
            axios.post(apiUrl('client/quen-mat-khau'), payload)
                .then((response) => {
                    if (response.data.status) {
                        this.$router.push('/client/lay-lai-mat-khau');
                        this.$toast.success(response.data.message);
                    } else {
                        this.$toast.error(response.data.message);
                    }
                })
                .catch((err) => {
                     this.$toast.error("Hệ thống đang bận hoặc có lỗi xảy ra.");
                })
        }
    }
}
</script>

<style scoped>
/* CSS ĐỒNG BỘ VỚI TRANG ĐĂNG NHẬP / ĐĂNG KÝ */

/* 1. Card trong suốt mờ (Glassmorphism) */
.glass-card {
    background-color: rgba(26, 13, 91, 0.6) !important; 
    backdrop-filter: blur(15px); 
    -webkit-backdrop-filter: blur(15px);
    border-radius: 16px;
    border: 1px solid #E3CC81 !important; 
}

/* 2. Nút Gradient */
.btn-gradient {
    background: linear-gradient(to right, #deaa3b, #E3CC81);
    border: none;
    border-radius: 8px;
    transition: all 0.3s ease;
}
.btn-gradient:hover {
    background: linear-gradient(to right,#deaa3b, #E3CC81);
    transform: translateY(-2px);
    box-shadow: 0 5px 15px rgba(222, 170, 59, 0.4);
}

/* 3. Nút Outline (Quay lại) */
.btn-outline-light:hover {
    background-color: rgba(255, 255, 255, 0.15) !important;
    color: white;
}

/* 4. Custom Input (Nền tối, chữ trắng) */
.custom-input-bg {
    background-color: rgba(15, 10, 30, 0.5) !important;
    border-color: rgba(255, 255, 255, 0.1) !important;
    color: white !important;
}

/* 5. Xử lý khi focus vào ô input */
.form-control.custom-input-bg:focus {
    background-color: rgba(15, 10, 30, 0.8) !important;
    border-color: #E3CC81 !important; 
    box-shadow: 0 0 0 0.25rem rgba(227, 204, 129, 0.25) !important;
    color: white !important;
}

/* Placeholder màu xám nhạt */
.form-control::placeholder {
    color: rgba(255, 255, 255, 0.3) !important;
}

/* 6. Màu chữ phụ trợ */
.text-light-50 {
    color: rgba(255, 255, 255, 0.6);
}
</style>