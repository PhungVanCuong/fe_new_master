<template>
    <div class="authentication-reset-password d-flex align-items-center justify-content-center my-5 my-lg-0" style="min-height: 100vh;">
        <div class="container">
            <div class="row">
                <div class="col-12 col-lg-10 mx-auto">
                    <div class="card glass-card border-0 shadow-lg overflow-hidden">
                        <div class="row g-0">
                            
                            <div class="col-lg-5 border-end border-secondary border-opacity-25 d-flex align-items-center">
                                <div class="card-body p-4 p-sm-5">
                                    <div class="form-body">
                                        <h4 class="fw-bold text-white mb-2">Thay Đổi Mật Khẩu</h4>
                                        <p class="text-light-50 mb-4">Nhập mã khôi phục và mật khẩu mới để tiếp tục</p>
                                        
                                        <div class="row g-4">
                                            
                                            <div class="col-12">
                                                <label class="form-label text-white-50 small mb-1">Mã Khôi Phục</label>
                                                <div class="input-group">
                                                    <span class="input-group-text custom-input-bg border-end-0 text-white-50">
                                                        <i class="fa-solid fa-shield-halved"></i>
                                                    </span>
                                                    <input v-model="reset.ma_khoi_phuc" type="text" 
                                                           class="form-control custom-input-bg border-start-0 text-white" 
                                                           placeholder="Nhập mã khôi phục">
                                                </div>
                                            </div>
                                            
                                            <div class="col-12">
                                                <label class="form-label text-white-50 small mb-1">Mật khẩu mới</label>
                                                <div class="input-group">
                                                    <span class="input-group-text custom-input-bg border-end-0 text-white-50">
                                                        <i class="fa-solid fa-lock"></i>
                                                    </span>
                                                    <input v-model="reset.password" type="password" 
                                                           class="form-control custom-input-bg border-start-0 text-white" 
                                                           placeholder="••••••••">
                                                </div>
                                            </div>
                                            
                                            <div class="col-12">
                                                <label class="form-label text-white-50 small mb-1">Xác nhận mật khẩu</label>
                                                <div class="input-group">
                                                    <span class="input-group-text custom-input-bg border-end-0 text-white-50">
                                                        <i class="fa-solid fa-lock-open"></i>
                                                    </span>
                                                    <input v-model="reset.new_password" type="password" 
                                                           @keyup.enter="doiMatKhau"
                                                           class="form-control custom-input-bg border-start-0 text-white" 
                                                           placeholder="••••••••">
                                                </div>
                                            </div>
                                            
                                            <div class="col-12 mt-4">
                                                <div class="d-grid gap-3">
                                                    <button @click="doiMatKhau()" type="button" class="btn btn-gradient py-2 fw-bold text-white d-flex justify-content-center align-items-center gap-2">
                                                        Xác Nhận Thay Đổi <i class="fa-solid fa-check"></i>
                                                    </button>
                                                    
                                                    <router-link to="/client/dang-nhap" class="text-decoration-none">
                                                        <button class="btn btn-outline-light w-100 py-2 d-flex justify-content-center align-items-center gap-2" style="background-color: rgba(255,255,255,0.05); border-color: rgba(255,255,255,0.2);">
                                                            <i class="fa-solid fa-arrow-left"></i> Quay lại đăng nhập
                                                        </button>
                                                    </router-link>
                                                </div>
                                            </div>

                                        </div>
                                    </div>
                                </div>
                            </div>

                            <div class="col-lg-7 d-none d-lg-block">
                                <img src="../../../assets/images/login-images/forgot-password-frent-img.jpg" 
                                     class="img-fluid h-100 w-100" 
                                     style="object-fit: cover; opacity: 0.9;" 
                                     alt="Thay đổi mật khẩu">
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
            reset: {
                ma_khoi_phuc: null,
                password: null,
                new_password: null,
            },
        }
    },
    methods: {
        doiMatKhau() {
            axios.post(apiUrl('client/lay-lai-mat-khau'), this.reset)
                .then((response) => {
                    if (response.data.status) {
                        this.$router.push('/client/dang-nhap');
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
/* CSS ĐỒNG BỘ VỚI CÁC TRANG TRƯỚC */

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
    background: linear-gradient(to right, #deaa3b, #E3CC81);
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