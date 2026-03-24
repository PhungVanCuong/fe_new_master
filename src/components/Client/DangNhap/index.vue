<template>
    <div class="section-authentication-signin d-flex align-items-center justify-content-center my-5 my-lg-0" style="min-height: 100vh;">
        <div class="container">
            <div class="row">
                <div class="col-12 col-md-8 col-lg-6 col-xl-5 mx-auto">
                    
                    <div class="text-center ">
                        <div class="mx-auto d-flex align-items-center justify-content-center">
                            <img src="../../../assets/images/2-removebg-preview.png" alt="Tr0ondCinema Logo" class="img-fluid" style="max-width: 300px; height: auto; margin-bottom: -80px;">
                        </div>
                        <h2 class="fw-bold text-white mb-2">Chào Mừng Trở Lại</h2>
                        <p class="text-light-50">Đăng nhập để tiếp tục trải nghiệm điện ảnh cùng <span class="fw-bold text-gradient">Tr0ondCinema</span></p>
                    </div>

                    <div class="card glass-card border-0 shadow-lg">
                        <div class="card-body p-4 p-sm-5">
                            <div class="form-body">
                                <div class="row g-4">
                                    
                                    <div class="col-12">
                                        <label class="form-label text-white-50 small mb-1">Địa chỉ Email</label>
                                        <div class="input-group">
                                            <span class="input-group-text custom-input-bg border-end-0 text-white-50">
                                                <i class="fa-regular fa-envelope"></i>
                                            </span>
                                            <input v-model="user.email" type="email" 
                                                class="form-control custom-input-bg border-start-0 text-white" 
                                                placeholder="you@example.com">
                                        </div>
                                    </div>

                                    <div class="col-12">
                                        <div class="d-flex justify-content-between align-items-center mb-1">
                                            <label class="form-label text-white-50 small mb-0">Mật khẩu</label>
                                            <router-link to="/client/quen-mat-khau" class="text-gradient small text-decoration-none">
                                                Quên mật khẩu?
                                            </router-link>
                                        </div>
                                        <div class="input-group">
                                            <span class="input-group-text custom-input-bg border-end-0 text-white-50">
                                                <i class="fa-solid fa-lock"></i>
                                            </span>
                                            <input v-model="user.password" type="password" 
                                                @keyup.enter="dangNhap"
                                                class="form-control custom-input-bg border-start-0 text-white" 
                                                placeholder="••••••••">
                                        </div>
                                    </div>

                                    <div class="col-12 mt-4">
                                        <button @click="dangNhap" class="btn btn-gradient w-100 py-2 fw-bold text-white d-flex justify-content-center align-items-center gap-2">
                                            Đăng Nhập <i class="fa-solid fa-arrow-right"></i>
                                        </button>
                                    </div>

                                    <div class="col-12 text-center mt-4 pt-2">
                                        <p class="text-light-50 mb-0">Bạn chưa có tài khoản? 
                                            <router-link to="/client/dang-ky" class="text-gradient fw-bold text-decoration-none ms-1">
                                                Đăng ký tại đây
                                            </router-link>
                                        </p>
                                    </div>
                                    
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
import apiUrl from '../../../utils/api'

export default {
    data() {
        return {
            user: {
                email: '',
                password: '',
            },
        }
    },
    methods: {
        dangNhap() {
            axios.post(apiUrl('client/dang-nhap'), this.user)
                .then((res) => {
                    if (res.data.status) {
                        localStorage.setItem('key_client', res.data.token);
                        this.$router.push('/');
                        this.$toast.success(res.data.message);
                    } else {
                        this.$toast.error(res.data.message);
                    }
                })
                .catch((err) => {
                    this.$toast.error("Đã có lỗi xảy ra. Vui lòng thử lại!");
                })
        }
    },
}
</script>

<style scoped>
/* CSS TÙY CHỈNH CHO GIAO DIỆN ĐĂNG NHẬP */

/* 1. Card trong suốt mờ (Glassmorphism) */
.glass-card {
    background-color: rgba(26, 13, 91, 0.6) !important; 
    backdrop-filter: blur(15px); 
    -webkit-backdrop-filter: blur(15px);
    border-radius: 16px;
    border: 1px solid #E3CC81 !important; 
}

/* ĐÃ XOÁ CLASS .login-logo BỊ SAI KÍCH THƯỚC Ở ĐÂY */

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

/* 3. Text Gradient (Dùng cho chữ nổi bật) */
.text-gradient {
    background: linear-gradient(to right, #deaa3b, #E3CC81);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
}

/* 4. Custom Input (Nền tối, chữ trắng) */
.custom-input-bg {
    background-color: rgba(15, 10, 30, 0.5) !important; /* Đen tím mờ */
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