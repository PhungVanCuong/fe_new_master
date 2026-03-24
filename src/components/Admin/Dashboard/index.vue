<template>
    <div class="row row-cols-1 row-cols-md-2 row-cols-xl-4">
        <div class="col">
            <div class="card radius-10 border-start border-0 border-3 " style="background: linear-gradient(to right, rgb(142, 45, 226), rgb(74, 0, 224)) !important; color: white;">
                <div class="card-body">
                    <div class="d-flex align-items-center">
                        <div class="text-wrap">
                            <p class="mb-0">Tổng doanh thu</p>
                            <h4 class="my-1 text-white">{{ formatVND(thong_ke.tong_doanh_thu) }}</h4>
                        </div>
                        <div class="widgets-icons-2 rounded-circle bg-gradient-scooter text-white ms-auto">
                            <i class='bx bxs-wallet'></i>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="col">
            <div class="card radius-10 border-start border-0 border-3" style="background: linear-gradient(45deg, #ee0979, #ff6a00)!important; color: white;">
                <div class="card-body">
                    <div class="d-flex align-items-center">
                        <div class="text-wrap">
                            <p class="mb-0 text-white">Tổng Phim</p>
                            <h4 class="my-1 text-white">{{ thong_ke.tong_phim }}</h4>
                        </div>
                        <div class="widgets-icons-2 rounded-circle bg-gradient-bloody text-white ms-auto">
                            <i class='bx bx-film'></i>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <div class="col">
            <div class="card radius-10 border-start border-0 border-3" style="background:  linear-gradient(to right, rgb(247, 151, 30), rgb(255, 210, 0)) !important;">         
                      <div class="card-body">
                    <div class="d-flex align-items-center">
                        <div>
                            <p class="mb-0 text-white" style="margin-right: 10px;">Tổng Vé Đã Bán</p>
                            <h4 class="my-1 text-white" style="margin-right: 10px;">{{ thong_ke.tong_ve }}</h4>
                        </div>
                        <div class="widgets-icons-2 rounded-circle bg-gradient-blooker text-white ms-auto">
                            <i class='bx bxs-group'></i>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <div class="col">
            <div class="card radius-10 border-start border-0 border-3 " style="background: linear-gradient(43deg, #00b09b, #96C93C)!important; color: white;">
                <div class="card-body">
                    <div class="d-flex align-items-center">
                        <div>
                            <p class="mb-0 text-white" style="margin-right: 10px;">Tổng Phòng Chiếu</p>
                            <h4 class="my-1 text-white" style="margin-right: 10px;">{{ thong_ke.tong_phong }}</h4>
                        </div>
                        <div class="widgets-icons-2 rounded-circle bg-gradient-ohhappiness text-white ms-auto">
                            <i class='bx bx-building'></i>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <div class="row">
        <div class="col-lg-8 d-flex">
            <div class="card radius-10 border-top border-0 border-3 border-info flex-fill">
                <div class="card-body">
                    <div class="row">
                        <div class="col-lg-12">
                            <h4 class="mt-2"><b>ĐẶT VÉ GẦN ĐÂY</b></h4>
                            <p class="text-secondary">Các giao dịch mới nhất.</p>

                            <div v-for="(don, index) in don_hang_gan_day" :key="index" class="booking-item border rounded p-3 mb-2">
                                <div class="d-flex justify-content-between align-items-center">
                                    <div>
                                        <h5 class="mb-0">{{ don.ho_va_ten }}</h5>
                                        <p class="mb-0">{{ don.ten_phim || 'Đơn hàng' }} - {{ don.tong_ve }} vé</p>
                                    </div>
                                    <div class="text-end">
                                        <h5 class="mb-0">{{ formatVND(don.thanh_tien) }}</h5>
                                        <p class="mb-0">{{ formatDate(don.ngay_tao) }}</p>
                                    </div>
                                </div>
                            </div>
                            
                            <div v-if="don_hang_gan_day.length === 0" class="text-center text-muted mt-3">
                                Chưa có đơn hàng nào.
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <div class="col-lg-4 d-flex">
            <div class="card radius-10 border-top border-0 border-3 border-success flex-fill">
                <div class="card-body">
                    <div class="row">
                        <div class="col-lg-12">
                            <h4 class="mt-2"><b>PHIM PHỔ BIẾN</b></h4>
                            <p class="text-secondary">Top phim bán chạy nhất.</p>

                            <div v-for="(phim, index) in phim_pho_bien" :key="index" class="d-flex justify-content-between align-items-center mb-3">
                                <div class="d-flex align-items-center">
                                    <span class="me-3 fw-bold">{{ index + 1 }}.</span>
                                    <div>
                                        <h5 class="mb-0 text-truncate" style="max-width: 150px;">{{ phim.ten_phim }}</h5>
                                        <p class="mb-0 text-secondary">{{ phim.the_loai || 'Phim hay' }}</p>
                                    </div>
                                </div>
                                <div>
                                    <span class="badge bg-primary">{{ phim.tong_ve }} vé</span>
                                </div>
                            </div>

                            <div v-if="phim_pho_bien.length === 0" class="text-center text-muted mt-3">
                                Chưa có dữ liệu thống kê.
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
import apiUrl from '../../../utils/api'; // Đường dẫn tới file config API của bạn

export default {
    data() {
        return {
            thong_ke: {
                tong_doanh_thu: 0,
                tong_phim: 0,
                tong_ve: 0,
                tong_phong: 0
            },
            don_hang_gan_day: [],
            phim_pho_bien: []
        }
    },
    mounted() {
        this.loadDashboard();
    },
    methods: {
        loadDashboard() {
            // Gọi API lấy số liệu
            axios.get(apiUrl('admin/dashboard/index'), {
                headers: {
                    Authorization: "Bearer " + localStorage.getItem("key_admin")
                }
            })
            .then((res) => {
                if(res.data.status) {
                    this.thong_ke = res.data.thong_ke;
                    this.don_hang_gan_day = res.data.don_hang_gan_day;
                    this.phim_pho_bien = res.data.phim_pho_bien;
                }
            })
            .catch((err) => {
                console.error("Lỗi tải dashboard:", err);
            });
        },
        formatVND(number) {
            return new Intl.NumberFormat('vi-VN', { style: 'currency', currency: 'VND' }).format(number);
        },
        formatDate(dateString) {
            if (!dateString) return '';
            const date = new Date(dateString);
            return date.toLocaleString('vi-VN', { 
                hour: '2-digit', minute: '2-digit', 
                day: '2-digit', month: '2-digit' 
            });
        }
    }
}
</script>

<style scoped>
/* Giữ nguyên style của bạn */
</style>