<template>
    <div class="container py-5">
        <div class="row justify-content-center">
            <div class="col-md-6">
                <div class="card shadow-lg border-0 rounded-4">
                    <div class="card-body text-center p-5">

                        <div v-if="!isThanhCong">
                            <div class="spinner-border text-primary mb-4" style="width: 4rem; height: 4rem;"
                                role="status">
                                <span class="visually-hidden">Loading...</span>
                            </div>
                            <h3 class="fw-bold text-primary">Đang chờ xác nhận thanh toán...</h3>
                            <p class="text-muted mt-3">
                                Vui lòng không tắt trình duyệt. Hệ thống đang kiểm tra giao dịch cho đơn hàng <b>{{
                                    ma_don_hang }}</b>.
                            </p>
                            <div class="alert alert-warning mt-4">
                                <small>(Hệ thống sẽ tự động cập nhật sau khi Admin xác nhận)</small>
                            </div>
                        </div>

                        <div v-else>
                            <div class="mb-4">
                                <i class="fa-solid fa-circle-check text-success" style="font-size: 80px;"></i>
                            </div>
                            <h2 class="fw-bold text-success">Thanh Toán Thành Công!</h2>
                            <p class="text-muted mb-4">Vé của bạn đã được xuất thành công.</p>

                            <div class="d-grid gap-2">
                                <a href="/" class="btn btn-outline-primary btn-lg">
                                    <i class="fa-solid fa-house me-2"></i> Quay Về Trang Chủ
                                </a>
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
import apiUrl from '../../../utils/api'; // Đảm bảo đường dẫn đúng

export default {
    data() {
        return {
            ma_don_hang: this.$route.params.ma_don_hang, // Lấy mã đơn hàng từ URL
            isThanhCong: false,
            polling: null, // Biến để lưu vòng lặp
        }
    },
    mounted() {
        this.startPolling();
    },
    beforeUnmount() {
        clearInterval(this.polling); // Tắt vòng lặp khi rời trang
    },
    methods: {
        startPolling() {
            // Cứ 3 giây (3000ms) gọi hàm kiểm tra 1 lần
            this.polling = setInterval(() => {
                this.checkStatus();
            }, 3000);
        },

        checkStatus() {
            axios.post(apiUrl('client/don-hang/check-status'), {
                ma_don_hang: this.ma_don_hang
            })
                .then((res) => {
                    // Giả sử server trả về status: true khi Admin đã xác nhận
                    if (res.data.status) {

                        // 1. Dừng vòng lặp kiểm tra ngay lập tức
                        clearInterval(this.polling);

                        this.isThanhCong = true;
                        this.$toast.success("Thanh toán thành công! Đang chuyển sang in vé...");

                        // 2. Đợi 1 chút (tuỳ chọn) rồi chuyển sang trang In Vé
                        setTimeout(() => {
                            this.$router.push({
                                name: 'admin-in-ve', // Tên route của trang InHoaDon (xem Bước 2)
                                params: { ma_hoa_don: this.ma_don_hang }
                            });
                        }, 1000);
                    }
                })
                .catch(() => {
                    // Lỗi mạng thì bỏ qua, chờ lần check tiếp theo
                });
        }
    }
}
</script>