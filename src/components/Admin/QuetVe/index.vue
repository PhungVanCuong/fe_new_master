<template>
    <div class="container-fluid  bg-light">
        <div class="container-xl py-4 py-md-5">
            <div class="row g-4">
                <div class="col-12 col-lg-4">
                    <div class="card border-0 shadow-sm h-100">
                        <div class="card-body p-4 p-md-5">
                            <div class="mb-4">
                                <h2 class="fw-bold mb-3">
                                    Quét Mã Vé
                                </h2>
                                <p class="text-muted mb-0">Nhập mã vé hoặc quét mã vạch để tra cứu thông tin</p>
                            </div>
                            <div class="mb-3">
                                <label class="form-label fw-semibold mb-2">Mã vé</label>
                                <input ref="scannerInput" type="text" 
                                    class="form-control form-control-lg" 
                                    placeholder="Nhập mã vé..." 
                                    v-model="ma_ve_input" @keyup.enter="scanTicket" autofocus>
                            </div>
                            <button class="btn btn-dark btn-lg w-100 mb-3" @click="scanTicket">
                                <i class="bi bi-search me-2"></i> Tìm kiếm
                            </button>
                            
                            <div v-if="ticket_info">
                                <button class="btn btn-outline-secondary w-100" @click="resetScan">
                                    <i class="bi bi-arrow-clockwise me-2"></i>
                                    Quét vé khác
                                </button>
                            </div>
                        </div>
                    </div>
                </div>

                <div class="col-12 col-lg-8">
                    <div v-if="ticket_info">
                        <div class="mb-4">
                            <h3 class="fw-bold mb-0">
                                <i class="bi bi-ticket-perforated me-2"></i>
                                Thông tin vé
                            </h3>
                        </div>
                        <div class="row g-4">
                            <div class="col-12 col-md-5">
                                <div class="card border-0 shadow-sm h-100">
                                    <div class="card-body p-0 d-flex flex-column">
                                        <div class="p-3 bg-white">
                                            <h6 class="mb-0 fw-bold">{{ ticket_info.ten_phim }}</h6>
                                        </div>
                                        <div>
                                            <img :src="ticket_info.hinh_anh" class="object-fit-cover h-100 w-100 img-fluid">
                                        </div>
                                    </div>
                                </div>
                            </div>
                            <div class="col-12 col-md-7">
                                <div class="card border-0 shadow-sm h-100">
                                    <div class="card-body p-4 p-md-5">
                                        <div class="mb-4 pb-3 border-bottom">
                                            <div class="d-flex align-items-center gap-3">
                                                <i v-if="ticket_info.is_thanh_toan === 1" class="bi bi-check-circle-fill text-success fs-1"></i>
                                                <i v-else class="bi bi-x-circle-fill text-danger fs-1"></i>
                                                <div>
                                                    <small class="text-muted d-block mb-1">Trạng thái thanh toán</small>
                                                    <h4 v-if="ticket_info.is_thanh_toan === 1" class="mb-0 fw-bold text-success">
                                                        Đã Thanh Toán
                                                    </h4>
                                                    <h4 v-else class="mb-0 fw-bold text-danger">
                                                        Chưa Thanh Toán
                                                    </h4>
                                                </div>
                                            </div>
                                        </div>
                                        <div class="mb-4">
                                            <div class="row g-3">
                                                <div class="col-6">
                                                    <div class="p-3 bg-light rounded">
                                                        <small class="text-muted d-block mb-2">Mã vé</small>
                                                        <h6 class="mb-0 fw-bold">{{ ticket_info.ma_ve }}</h6>
                                                    </div>
                                                </div>
                                                
                                                <div class="col-6">
                                                    <div class="p-3 bg-light rounded">
                                                        <small class="text-muted d-block mb-2">Giá vé</small>
                                                        <h5 class="mb-0 fw-bold">{{ formatVND(ticket_info.gia_ve) }}</h5>
                                                    </div>
                                                </div>
                                                
                                                <div class="col-6">
                                                    <div class="p-3 bg-light rounded">
                                                        <small class="text-muted d-block mb-2">Suất chiếu</small>
                                                        <h6 class="mb-0 fw-bold">{{ ticket_info.gio_chieu }}</h6>
                                                    </div>
                                                </div>
                                                
                                                <div class="col-6">
                                                    <div class="p-3 bg-light rounded">
                                                        <small class="text-muted d-block mb-2">Phòng chiếu</small>
                                                        <h6 class="mb-0 fw-bold">{{ ticket_info.ten_phong }}</h6>
                                                    </div>
                                                </div>
                                                
                                                <div class="col-6">
                                                    <div class="p-3 bg-light rounded">
                                                        <small class="text-muted d-block mb-2">Ghế</small>
                                                        <h6 class="mb-0 fw-bold">{{ ticket_info.ten_ghe }}</h6>
                                                    </div>
                                                </div>
                                                
                                                <div class="col-6">
                                                    <div class="p-3 bg-light rounded">
                                                        <small class="text-muted d-block mb-2">Ngày chiếu</small>
                                                        <h6 class="mb-0 fw-bold">{{ ticket_info.ngay_chieu }}</h6>
                                                    </div>
                                                </div>
                                            </div>
                                        </div>
                                        <div class="mb-4 pb-3 border-top border-bottom pt-3">
                                            <small class="text-muted d-block mb-2">Khách hàng</small>
                                            <h5 class="mb-0 fw-bold">{{ ticket_info.ten_khach_hang }}</h5>
                                        </div>
                                        <div class="mt-auto">
                                            <div v-if="ticket_info.is_check_in === 1" class="alert alert-success mb-3 border-0">
                                                <div class="d-flex align-items-center">
                                                    <i class="bi bi-check-circle-fill me-3 fs-4"></i>
                                                    <div>
                                                        <strong class="d-block mb-1">Đã check-in</strong>
                                                        <p class="mb-0 small">Vé đã được xác nhận vào cửa</p>
                                                    </div>
                                                </div>
                                            </div>
                                            <div v-else class="alert alert-warning mb-3 border-0">
                                                <div class="d-flex align-items-center">
                                                    <i class="bi bi-clock-history me-3 fs-4"></i>
                                                    <div>
                                                        <strong class="d-block mb-1">Chưa check-in</strong>
                                                        <p class="mb-0 small">Vui lòng check-in để vào phòng</p>
                                                    </div>
                                                </div>
                                            </div>
                                            
                                            <button v-if="ticket_info.is_check_in === 0" 
                                                class="btn btn-dark btn-lg w-100"
                                                @click="checkInTicket">
                                                <i class="bi bi-box-arrow-in-right me-2"></i>
                                                CHECK-IN NGAY
                                            </button>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>

                    <div v-else>
                        <div class="card border-0 shadow-sm">
                            <div class="card-body p-5 text-center">
                                <i class="bi bi-ticket-detailed text-muted mb-3 d-block display-1"></i>
                                <h4 class="text-muted mb-2">Chưa có thông tin vé</h4>
                                <p class="text-muted mb-0">Vui lòng nhập mã vé ở bên trái để tra cứu</p>
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
            ma_ve_input: '',
            ticket_info: null,
        }
    },
    methods: {
        scanTicket() {
            axios.post(apiUrl("admin/ve/in-ve"), {noi_dung: this.ma_ve_input}, {
                    headers: { 
                        Authorization: "Bearer " + localStorage.getItem("key_admin") 
                    }
                })
                .then((res) => {
                    if(res.data.status === 1) {
                        this.ticket_info = res.data.data;
                        setTimeout(() => {
                            if(this.ticket_info.is_check_in === 0 && this.ticket_info.is_thanh_toan === 1) {
                                this.checkInTicket(true);
                            }
                        }, 500);
                    } else {
                        this.$toast.error(res.data.message);
                        this.ticket_info = null;
                    }
                })
                .catch((res) => {
                    if (res.response && res.response.data.errors) {
                        const list = Object.values(res.response.data.errors);
                        list.forEach((v, i) => {
                            this.$toast.error(v[0]);
                        });
                    } else if (res.response && res.response.data.message) {
                        this.$toast.error(res.response.data.message);
                    }
                    this.ticket_info = null;
                })
                .finally(() => {
                    setTimeout(() => {
                        this.ma_ve_input = '';
                        this.$refs.scannerInput.focus();
                    }, 200);
                });
        },
        
        checkInTicket(auto = false) {
            axios.post(apiUrl("admin/ve/check-in"), {
                    ma_ve: this.ticket_info.ma_ve
                }, {
                    headers: { 
                        Authorization: "Bearer " + localStorage.getItem("key_admin") 
                    }
                })
                .then((res) => {
                    if(res.data.status === true) {
                        this.$toast.success(res.data.message);
                        // Gọi lại API để lấy thông tin vé mới nhất
                        this.ma_ve_input = this.ticket_info.ma_ve;
                        this.scanTicket();
                    } else {
                        this.$toast.error(res.data.message);
                    }
                })
                .catch((res) => {
                    if (res.response && res.response.data.errors) {
                        const list = Object.values(res.response.data.errors);
                        list.forEach((v, i) => {
                            this.$toast.error(v[0]);
                        });
                    } else if (res.response && res.response.data.message) {
                        this.$toast.error(res.response.data.message);
                    }
                })
                .finally(() => {
                    this.$refs.scannerInput.focus();
                });
        },

        resetScan() {
            this.ticket_info = null;
            this.ma_ve_input = '';
            this.$refs.scannerInput.focus();
        },

        formatVND(number) {
            return new Intl.NumberFormat("vi-VI", { style: "currency", currency: "VND" }).format(number);
        },
    },
    mounted() {
        this.$refs.scannerInput.focus();
    },
}
</script>

<style></style>