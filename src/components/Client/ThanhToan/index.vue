<template>
    <div class="container mt-4">
        <div v-if="loading" class="text-center py-5">
            <div class="spinner-border text-primary" role="status">
                <span class="visually-hidden">Đang tải...</span>
            </div>
            <p class="mt-3 text-muted">Đang tải thông tin thanh toán...</p>
        </div>

        <div v-else-if="error" class="alert alert-danger text-center">
            <i class="fas fa-exclamation-triangle me-2"></i>
            {{ error }}
            <div class="mt-3">
                <button @click="goBack" class="btn btn-secondary">
                    <i class="fas fa-arrow-left me-2"></i>Quay lại
                </button>
            </div>
        </div>

        <div v-else>
            <div class="row g-4">
                <div class="col-lg-7">
                    <div class="card shadow-sm">
                        <div class="card-header bg-primary text-white">
                            <h5 class="mb-0 text-white">
                                <i class="fas fa-ticket-alt me-2"></i>
                                Chi Tiết Đơn Hàng
                            </h5>
                        </div>

                        <div class="card-body">
                            <!-- Movie Info -->
                            <div class="mb-4" v-if="list_don_hang.phim">
                                <div class="row">
                                    <div class="col-md-4">
                                        <img :src="list_don_hang.phim.hinh_anh" alt="Movie Poster"
                                            class="img-fluid rounded shadow-sm"
                                            style="width: 100%; height: auto; object-fit: cover;">
                                    </div>
                                    <div class="col-md-8">
                                        <h4 class="fw-bold">{{ list_don_hang.phim.ten_phim }}</h4>
                                        <div class="mb-2">
                                            <i class="fas fa-calendar-alt text-primary me-2"></i>
                                            <span>{{ formatDate(list_don_hang.phim.ngay_chieu) }}</span>
                                        </div>
                                        <div class="mb-2">
                                            <i class="fas fa-clock text-primary me-2"></i>
                                            <span>{{ list_don_hang.phim.thoi_gian_bat_dau }}</span>
                                        </div>
                                        <div class="mb-2">
                                            <i class="fas fa-door-open text-primary me-2"></i>
                                            <span>{{ list_don_hang.phim.ten_phong }}</span>
                                        </div>
                                        <div class="mb-2" v-if="list_don_hang.phim.the_loai">
                                            <i class="fas fa-film text-primary me-2"></i>
                                            <span>{{ list_don_hang.phim.the_loai }}</span>
                                        </div>
                                        <div class="mb-2" v-if="list_don_hang.phim.thoi_luong">
                                            <i class="fas fa-hourglass-half text-primary me-2"></i>
                                            <span>{{ list_don_hang.phim.thoi_luong }} phút</span>
                                        </div>
                                    </div>
                                </div>
                            </div>

                            <hr>

                            <!-- Seats -->
                            <div class="mb-4" v-if="list_don_hang.ve && list_don_hang.ve.length > 0">
                                <h5 class="fw-bold">
                                    <i class="fas fa-couch me-2 text-warning"></i>Ghế đã chọn
                                </h5>
                                <div class="list-group">
                                    <div v-for="(ve, index) in list_don_hang.ve" :key="index"
                                        class="list-group-item d-flex justify-content-between align-items-center">
                                        <span>Ghế {{ ve.ten_ghe }}</span>
                                        <span class="badge bg-warning text-dark">{{ formatVND(ve.gia_ve) }}</span>
                                    </div>
                                </div>
                            </div>

                            <!-- Services -->
                            <div v-if="list_don_hang.dich_vu && list_don_hang.dich_vu.length > 0" class="mb-4">
                                <h5 class="fw-bold">
                                    <i class="fas fa-concierge-bell me-2 text-success"></i>Dịch vụ
                                </h5>
                                <div class="list-group">
                                    <div v-for="(dichVu, index) in list_don_hang.dich_vu" :key="index"
                                        class="list-group-item d-flex justify-content-between align-items-center">
                                        <div>
                                            <span>{{ dichVu.ten_dich_vu }}</span>
                                            <span class="badge bg-secondary ms-2">x{{ dichVu.so_luong }}</span>
                                        </div>
                                        <span class="badge bg-success">{{ formatVND(dichVu.thanh_tien) }}</span>
                                    </div>
                                </div>
                            </div>

                            <!-- Voucher Info -->
                            <div v-if="list_don_hang.voucher" class="alert alert-success mb-4">
                                <i class="fas fa-tag me-2"></i>
                                <span>Mã giảm giá: <strong>{{ list_don_hang.voucher.ma_code }}</strong></span>
                                <span class="float-end">Giảm {{ (list_don_hang.voucher.so_giam_gia).toFixed(0)
                                    }}%</span>
                            </div>

                            <hr>

                            <!-- Total -->
                            <div class="bg-light p-3 rounded">
                                <div class="d-flex justify-content-between mb-2">
                                    <span>Tổng vé:</span>
                                    <span class="fw-bold">{{ formatVND(list_don_hang.summary?.tong_tien_ve || 0)
                                        }}</span>
                                </div>
                                <div v-if="list_don_hang.summary?.tong_tien_dich_vu > 0"
                                    class="d-flex justify-content-between mb-2">
                                    <span>Tổng dịch vụ:</span>
                                    <span class="fw-bold">{{ formatVND(list_don_hang.summary.tong_tien_dich_vu)
                                        }}</span>
                                </div>
                                <div v-if="list_don_hang.summary?.giam_gia > 0"
                                    class="d-flex justify-content-between mb-2 text-success">
                                    <span>Giảm giá:</span>
                                    <span class="fw-bold">-{{ formatVND(list_don_hang.summary.giam_gia) }}</span>
                                </div>
                                <hr>
                                <div class="d-flex justify-content-between">
                                    <strong class="fs-5">Tổng thanh toán:</strong>
                                    <strong class="fs-5 text-danger">{{ formatVND(list_don_hang.summary?.tong_thanh_toan
                                        || 0) }}</strong>
                                </div>
                            </div>

                            <!-- Order Info -->
                            <div class="mt-3">
                                <div class="row">
                                    <div class="col-6">
                                        <small class="text-muted">Mã đơn hàng:</small><br>
                                        <strong>{{ list_don_hang.don_hang?.ma_don_hang }}</strong>
                                    </div>
                                    <div class="col-6 text-end">
                                        <small class="text-muted">Trạng thái:</small><br>
                                        <span v-if="list_don_hang.don_hang?.is_thanh_toan" class="badge bg-success">
                                            Đã thanh toán
                                        </span>
                                        <span v-else class="badge bg-warning">
                                            Chờ thanh toán
                                        </span>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Right Column - Payment Methods -->
                <div class="col-lg-5">
                    <div class="card shadow-sm">
                        <div class="card-header bg-warning text-dark">
                            <h5 class="mb-0 text-white">
                                <i class="fas fa-qrcode me-2"></i>
                                Thanh Toán QR Code
                            </h5>
                        </div>

                        <div class="card-body">
                            <!-- QR Code Display -->
                            <div class="mb-4">
                                <div class="card bg-light">
                                    <div class="card-body text-center">
                                        <h5 class="card-title">
                                            <i class="fas fa-qrcode me-2"></i>Quét mã QR để thanh toán
                                        </h5>
                                        <div class="bg-white p-3 rounded d-inline-block shadow-sm my-3">
                                            <img :src="list_don_hang.thanh_toan?.link_qr_code" alt="QR Code"
                                                style="width: 280px; height: 280px;">
                                        </div>
                                        <div class="text-start">
                                            <table class="table table-borderless table-sm">
                                                <tbody>
                                                    <tr>
                                                        <td><strong>Ngân hàng:</strong></td>
                                                        <td>{{ list_don_hang.thanh_toan?.ngan_hang || 'MB Bank' }}</td>
                                                    </tr>
                                                    <tr>
                                                        <td><strong>Số tài khoản:</strong></td>
                                                        <td>{{ list_don_hang.thanh_toan?.so_tai_khoan || '0394425076' }}
                                                        </td>
                                                    </tr>
                                                    <tr>
                                                        <td><strong>Chủ TK:</strong></td>
                                                        <td>{{ list_don_hang.thanh_toan?.chu_tai_khoan
                                                            || 'RAP CHIEU PHIM K24' }}</td>
                                                    </tr>
                                                    <tr>
                                                        <td><strong>Số tiền:</strong></td>
                                                        <td class="text-danger fw-bold">{{
                                                            formatVND(list_don_hang.summary?.tong_thanh_toan || 0) }}
                                                        </td>
                                                    </tr>
                                                    <tr>
                                                        <td><strong>Nội dung:</strong></td>
                                                        <td><code>{{ list_don_hang.thanh_toan?.noi_dung }}</code></td>
                                                    </tr>
                                                </tbody>
                                            </table>
                                        </div>
                                        <div class="alert alert-info mt-3 mb-0">
                                            <h6><i class="fas fa-info-circle me-2"></i>Hướng dẫn thanh toán:</h6>
                                            <ol class="mb-0 text-start">
                                                <li>Mở ứng dụng ngân hàng của bạn</li>
                                                <li>Chọn chức năng quét mã QR</li>
                                                <li>Quét mã QR phía trên</li>
                                                <li>Xác nhận thông tin và hoàn tất thanh toán</li>
                                            </ol>
                                        </div>
                                    </div>
                                </div>
                            </div>

                            <!-- Action Buttons -->
                            <div class="d-grid gap-2">
                                <button @click="checkThanhToan" class="btn btn-success btn-lg">
                                    <i class="fas fa-check-circle me-2"></i>
                                    Xác Nhận Đã Thanh Toán
                                </button>
                                <button class="btn btn-outline-secondary" @click="huyDonVaQuayLai()"
                                    :disabled="processing">
                                    <span v-if="processing" class="spinner-border spinner-border-sm me-1"></span>
                                    <i class="fa-solid fa-arrow-left me-1"></i> Quay lại chọn vé (Hủy đơn này)
                                </button>
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
    name: 'ThanhToan',
    props: ['ma_don_hang'],
    data() {
        return {
            loading: true,
            error: null,
            list_don_hang: {
                don_hang: null,
                khach_hang: null,
                phim: null,
                ve: [],
                dich_vu: [],
                voucher: null,
                summary: null,
                thanh_toan: null
            }
        };
    },
    mounted() {
        this.loadOrderDetails();
    },
    watch: {
        '$route.params.ma_don_hang'(newVal) {
            if (newVal) {
                this.loadOrderDetails();
            }
        }
    },
    methods: {

        loadOrderDetails() {
            this.loading = true;
            this.error = null;
            const maDonHang = this.ma_don_hang || this.$route.params.ma_don_hang;
            axios
                .get(apiUrl(`client/dat-ve/chi-tiet-thanh-toan-don-hang/${maDonHang}`), {
                    headers: {
                        Authorization: "Bearer " + localStorage.getItem('key_client')
                    }
                })
                .then((res) => {
                    if (res.data.status) {
                        this.list_don_hang = res.data.data;
                    } else {
                        this.error = res.data.message;
                    }
                    this.loading = false;
                })
                .catch((res) => {
                    const list = Object.values(res.response.data.errors);
                    list.forEach((v, i) => {
                        this.$toast.error(v[0]);
                    });
                });
        },
        huyDonVaQuayLai() {
            if (!confirm("Bạn có chắc muốn hủy đơn hàng này để chọn lại vé không?")) {
                return;
            }

            this.processing = true;

            axios.post(apiUrl('client/don-hang/quay-ve'), {
                ma_don_hang: this.list_don_hang.don_hang?.ma_don_hang
            }, {
                headers: {
                    Authorization: "Bearer " + localStorage.getItem('key_client')
                }
            })
                .then((res) => {
                    if (res.data.status) {
                        // Xóa thành công -> Quay lại trang đặt vé (hoặc trang trước đó)
                        this.$router.go(-1);
                    } else {
                        alert(res.data.message);
                    }
                })
                .catch((err) => {
                    console.error(err);
                    alert("Có lỗi khi hủy đơn hàng.");
                })
                .finally(() => {
                    this.processing = false;
                });
        },
        checkThanhToan() {
            const maDonHang = this.list_don_hang.don_hang?.ma_don_hang;
            if (maDonHang) {
                // Chuyển sang trang "Chờ xác nhận" để polling
                this.$router.push('/client/thanh-toan/cho-xac-nhan/' + maDonHang);
            } else {
                this.$toast.error("Không tìm thấy mã đơn hàng!");
            }
        },
        formatVND(number) {
            if (!number) return '0₫';
            return new Intl.NumberFormat('vi-VN', {
                style: 'currency',
                currency: 'VND'
            }).format(number);
        },

        formatDate(date) {
            if (!date) return '';
            const thuVN = ['Chủ Nhật', 'Thứ Hai', 'Thứ Ba', 'Thứ Tư', 'Thứ Năm', 'Thứ Sáu', 'Thứ Bảy'];
            const d = new Date(date);
            return `${thuVN[d.getDay()]}, ${d.getDate()}/${d.getMonth() + 1}/${d.getFullYear()}`;
        }
    }
};
</script>

<style></style>
