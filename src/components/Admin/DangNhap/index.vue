<template>
    <div class="section-authentication-signin d-flex align-items-center justify-content-center my-5 my-lg-0" style="min-height: 100vh;">
        <div class="container">
            <div class="row">
                <div class="col-12 col-md-8 col-lg-6 col-xl-5 mx-auto">
                    
                    <div class="text-center">
                        <div class="mx-auto d-flex align-items-center justify-content-center">
                            <img src="../../../assets/images/2-removebg-preview.png" alt="Tr0ondCinema Logo" class="img-fluid" style="max-width: 300px; height: auto; margin-bottom: -80px;">
                        </div>
                        <h2 class="fw-bold text-white mb-2 text-uppercase">Cổng Quản Trị</h2>
                        <p class="text-light-50">Truy cập hệ thống quản lý <span class="fw-bold text-gradient-admin">Tr0ondCinema</span></p>
                    </div>

                    <div class="card glass-card border-0 shadow-lg">
                        <div class="card-body p-4 p-sm-5">
                            <div class="form-body">
                                <div class="row g-4">
                                    
                                    <div class="col-12">
                                        <label class="form-label text-white-50 small mb-1">Email Quản Trị</label>
                                        <div class="input-group">
                                            <span class="input-group-text custom-input-bg border-end-0 text-white-50">
                                                <i class="fa-solid fa-user-shield"></i>
                                            </span>
                                            <input v-model="user.email" type="email" 
                                                class="form-control custom-input-bg border-start-0 text-white" 
                                                placeholder="admin@tr0ondcinema.com">
                                        </div>
                                    </div>

                                    <div class="col-12">
                                        <label class="form-label text-white-50 small mb-1">Mật khẩu</label>
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

                                    <div class="col-12 mt-5">
                                        <button @click="dangNhap" class="btn btn-gradient-admin w-100 py-2 fw-bold text-white d-flex justify-content-center align-items-center gap-2">
                                            Đăng Nhập Hệ Thống <i class="fa-solid fa-right-to-bracket"></i>
                                        </button>
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
        dangNhap(){
            axios.post(apiUrl('admin/dang-nhap'), this.user)
                .then((res) => {
                    if (res.data.status) {
                        localStorage.setItem('key_admin', res.data.token);
                        this.$router.push('/admin/nhan-vien');
                        this.$toast.success(res.data.message);
                    } else {
                        this.$toast.error(res.data.message);
                    }
                })
                .catch((res) => {
                    const list = Object.values(res.response.data.errors);
                    list.forEach((v, i) => {
                        this.$toast.error(v[0]);
                    });
                });
        }
    }
}
</script>

<style scoped>
/* CSS CHO GIAO DIỆN ADMIN (Tông màu Xanh/Lục bảo) */

/* 1. Card trong suốt mờ */
.glass-card {
    background-color: rgba(10, 15, 30, 0.7) !important; /* Tối hơn bản client một chút */
    backdrop-filter: blur(15px); 
    -webkit-backdrop-filter: blur(15px);
    border-radius: 16px;
    border: 1px solid rgba(0, 201, 255, 0.3) !important; /* Viền xanh nhẹ */
}

/* 2. Nút Gradient Admin (Xanh dương sang Xanh ngọc) */
.btn-gradient-admin {
    background: linear-gradient(to right, #00c6ff, #0072ff);
    border: none;
    border-radius: 8px;
    transition: all 0.3s ease;
}
.btn-gradient-admin:hover {
    background: linear-gradient(to right, #00b4e6, #0066e6);
    transform: translateY(-2px);
    box-shadow: 0 5px 15px rgba(0, 198, 255, 0.4);
}

/* 3. Text Gradient Admin */
.text-gradient-admin {
    background: linear-gradient(to right, #00c6ff, #0072ff);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
}

/* 4. Custom Input */
.custom-input-bg {
    background-color: rgba(0, 0, 0, 0.4) !important;
    border-color: rgba(255, 255, 255, 0.1) !important;
    color: white !important;
}

/* 5. Xử lý khi focus vào ô input (Viền sáng màu xanh) */
.form-control.custom-input-bg:focus {
    background-color: rgba(0, 0, 0, 0.6) !important;
    border-color: #00c6ff !important; 
    box-shadow: 0 0 0 0.25rem rgba(0, 198, 255, 0.25) !important;
    color: white !important;
}

.form-control::placeholder {
    color: rgba(255, 255, 255, 0.3) !important;
}

.text-light-50 {
    color: rgba(255, 255, 255, 0.6);
}
</style>