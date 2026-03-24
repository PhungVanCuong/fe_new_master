<template>
    <div class="section-authentication-signin d-flex align-items-center justify-content-center my-5 my-lg-0" style="min-height: 100vh;">
        <div class="container">
            <div class="row">
                <div class="col-12 col-md-10 col-lg-8 col-xl-7 mx-auto">
                    
                    <div class="text-center ">
                        <div class="mx-auto d-flex align-items-center justify-content-center">
                            <img src="../../../assets/images/2-removebg-preview.png" alt="Tr0ondCinema Logo" class="img-fluid" style="max-width: 300px; height: auto; margin-bottom: -80px;">
                        </div>
                        <h2 class="fw-bold text-white mb-2">Đăng Ký Tài Khoản</h2>
                        <p class="text-light-50">Tạo tài khoản và bắt đầu hành trình khám phá phim của bạn!</p>
                    </div>

                    <div class="card glass-card border-0 shadow-lg">
                        <div class="card-body p-4 p-sm-5">
                            <div class="form-body">
                                <div class="row g-4">
                                    
                                    <div class="col-md-6">
                                        <label class="form-label text-white-50 small mb-1">Họ và tên</label>
                                        <div class="input-group">
                                            <span class="input-group-text custom-input-bg border-end-0 text-white-50">
                                                <i class="fa-regular fa-user"></i>
                                            </span>
                                            <input v-model="user.ho_va_ten" type="text" class="form-control custom-input-bg border-start-0 text-white" placeholder="Nguyễn Văn A">
                                        </div>
                                    </div>

                                    <div class="col-md-6">
                                        <label class="form-label text-white-50 small mb-1">Địa chỉ Email</label>
                                        <div class="input-group">
                                            <span class="input-group-text custom-input-bg border-end-0 text-white-50">
                                                <i class="fa-regular fa-envelope"></i>
                                            </span>
                                            <input v-model="user.email" type="email" class="form-control custom-input-bg border-start-0 text-white" placeholder="you@example.com">
                                        </div>
                                    </div>

                                    <div class="col-md-4">
                                        <label class="form-label text-white-50 small mb-1">Số điện thoại</label>
                                        <div class="input-group">
                                            <span class="input-group-text custom-input-bg border-end-0 text-white-50">
                                                <i class="fa-solid fa-phone"></i>
                                            </span>
                                            <input v-model="user.so_dien_thoai" type="text" class="form-control custom-input-bg border-start-0 text-white" placeholder="0987654321">
                                        </div>
                                    </div>

                                    <div class="col-md-4">
                                        <label class="form-label text-white-50 small mb-1">Số CCCD</label>
                                        <div class="input-group">
                                            <span class="input-group-text custom-input-bg border-end-0 text-white-50">
                                                <i class="fa-regular fa-id-card"></i>
                                            </span>
                                            <input v-model="user.cccd" type="text" class="form-control custom-input-bg border-start-0 text-white" placeholder="012345678912">
                                        </div>
                                    </div>

                                    <div class="col-md-4">
                                        <label class="form-label text-white-50 small mb-1">Ngày sinh</label>
                                        <div class="input-group">
                                            <span class="input-group-text custom-input-bg border-end-0 text-white-50">
                                                <i class="fa-regular fa-calendar"></i>
                                            </span>
                                            <input v-model="user.ngay_sinh" type="date"  class="form-control custom-input-bg border-start-0 text-white" style="color-scheme: dark;">
                                        </div>
                                    </div>

                                    <div class="col-md-6">
                                        <label class="form-label text-white-50 small mb-1">Mật khẩu</label>
                                        <div class="input-group">
                                            <span class="input-group-text custom-input-bg border-end-0 text-white-50">
                                                <i class="fa-solid fa-lock"></i>
                                            </span>
                                            <input v-model="user.password" :type="showPassword ? 'text' : 'password'" class="form-control custom-input-bg border-start-0 border-end-0 text-white" placeholder="••••••••">
                                            <a href="javascript:;" class="input-group-text custom-input-bg border-start-0 text-white-50" @click="showPassword = !showPassword">
                                                <i :class="showPassword ? 'fa-regular fa-eye' : 'fa-regular fa-eye-slash'"></i>
                                            </a>
                                        </div>
                                    </div>

                                    <div class="col-md-6">
                                        <label class="form-label text-white-50 small mb-1">Nhập lại mật khẩu</label>
                                        <div class="input-group">
                                            <span class="input-group-text custom-input-bg border-end-0 text-white-50">
                                                <i class="fa-solid fa-lock"></i>
                                            </span>
                                            <input v-model="user.re_password" :type="showRePassword ? 'text' : 'password'" class="form-control custom-input-bg border-start-0 border-end-0 text-white" placeholder="••••••••">
                                            <a href="javascript:;" class="input-group-text custom-input-bg border-start-0 text-white-50" @click="showRePassword = !showRePassword">
                                                <i :class="showRePassword ? 'fa-regular fa-eye' : 'fa-regular fa-eye-slash'"></i>
                                            </a>
                                        </div>
                                    </div>

                                    <div class="col-12 mt-4">
                                        <div class="form-check form-switch">
                                            <input class="form-check-input" type="checkbox" id="flexSwitchCheckChecked">
                                            <label class="form-check-label text-light-50 " for="flexSwitchCheckChecked" style="padding-top: 2px;">
                                                Tôi đồng ý với Điều khoản dịch vụ & Chính sách bảo mật của <b class="text-gradient">Tr0ondCinema</b>.
                                            </label>
                                        </div>
                                    </div>

                                    <div class="col-12 mt-4">
                                        <button @click="dangKyTaiKhoan" @keyup.enter="dangKyTaiKhoan" class="btn btn-gradient w-100 py-2 fw-bold text-white d-flex justify-content-center align-items-center gap-2">
                                            Đăng Ký Ngay <i class="fa-solid fa-user-plus"></i>
                                        </button>
                                    </div>

                                    <div class="col-12 text-center mt-4 pt-2 border-top border-secondary border-opacity-25">
                                        <p class="text-light-50 mb-0 mt-3">Bạn đã có tài khoản? 
                                            <router-link to="/client/dang-nhap" class="text-gradient fw-bold text-decoration-none ms-1">
                                                Đăng nhập tại đây
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
                ho_va_ten: '',
                so_dien_thoai: '',
                cccd: '',
                ngay_sinh: '',
                email: '',
                password: '',
                re_password: '',
            },
            showPassword: false,
            showRePassword: false,
        }
    },
    methods: {
        dangKyTaiKhoan() {
            axios.post(apiUrl('client/dang-ky'), this.user)
                .then((res) => {
                    if (res.data.status) {
                        this.$router.push('/client/dang-nhap');
                        this.$toast.success(res.data.message); // Chuyển alert thành toast cho đẹp
                    } else {
                        this.$toast.error(res.data.message);
                    }
                })
                .catch((err) => {
                    this.$toast.error("Đã có lỗi xảy ra, vui lòng thử lại!");
                })
        }
    },
}
</script>

<style scoped>
.glass-card {
    background-color: rgba(26, 13, 91, 0.6) !important; 
    backdrop-filter: blur(15px); 
    -webkit-backdrop-filter: blur(15px);
    border-radius: 16px;
    border: 1px solid #E3CC81 !important; 
}
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

.text-gradient {
    background: linear-gradient(to right, #deaa3b, #E3CC81);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
}
.custom-input-bg {
    background-color: rgba(15, 10, 30, 0.5) !important; /* Đen tím mờ */
    border-color: rgba(255, 255, 255, 0.1) !important;
    color: white !important;
}
.form-control.custom-input-bg:focus {
    background-color: rgba(15, 10, 30, 0.8) !important;
    border-color: #E3CC81 !important; 
    box-shadow: 0 0 0 0.25rem rgba(227, 204, 129, 0.25) !important;
    color: white !important;
}

.form-control::placeholder {
    color: rgba(255, 255, 255, 0.3) !important;
}

.text-light-50 {
    color: rgba(255, 255, 255, 0.6);
}
.form-check-input:checked {
    background-color: #E3CC81;
    border-color: #E3CC81;
}
</style>