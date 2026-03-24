<template>
    <div class="container py-4">
        <div v-if="loading" class="text-center py-5">
            <div class="spinner-border text-warning" role="status">
                <span class="visually-hidden">Loading...</span>
            </div>
            <p class="text-muted mt-3">Đang tải chi tiết đơn hàng...</p>
        </div>

        <div v-else-if="error" class="text-center">
            <div class="alert alert-danger">{{ error }}</div>
            <button class="btn btn-outline-secondary" @click="backToBooking">
                Quay lại đặt vé
            </button>
        </div>

        <div v-else>
            <div class="card shadow-sm mb-4">
                <div class="card-body">
                    <div class="d-flex justify-content-between align-items-center flex-wrap">
                        <div>
                            <h5 class="fw-bold mb-1">Chi tiết đơn hàng</h5>
                            <p class="text-muted small mb-0">
                                Mã đơn:
                                <strong>{{ order.ma_don_hang }}</strong> |
                                Ngày đặt:
                                <strong>{{ formatDateTime(order.ngay_dat) }}</strong>
                            </p>
                        </div>
                        <span :class="statusBadgeClass" class="mt-2 mt-md-0">
                            {{ order.is_thanh_toan ? 'Đã thanh toán' : 'Chưa thanh toán' }}
                        </span>
                    </div>
                </div>
            </div>

            <div class="row g-4">
                <div class="col-lg-8">
                    <div class="card shadow-sm mb-4">
                        <div class="card-body">
                            <div class="row mb-3">
                                <div class="col-md-3 mb-3 mb-md-0">
                                    <img :src="moviePoster" alt="Movie" class="img-fluid rounded">
                                </div>
                                <div class="col-md-9">
                                    <h5 class="fw-bold mb-2">{{ movie?.ten_phim || '---' }}</h5>
                                    <p class="text-muted small mb-2">
                                        <i class="fa-solid fa-building me-1"></i>{{ movie?.ten_phong || '---' }}
                                    </p>
                                    <p class="text-muted small mb-2">
                                        <i class="fa-regular fa-calendar me-1"></i>{{ movieDate }}
                                        <i class="fa-regular fa-clock ms-3 me-1"></i>{{ movieTime }}
                                    </p>
                                    <p class="text-muted small mb-0">
                                        <i class="fa-solid fa-tag me-1"></i>{{ movie?.the_loai || '---' }}
                                        <i class="fa-solid fa-clock ms-3 me-1"></i>{{ movie?.thoi_luong || '---' }} phút
                                    </p>
                                </div>
                            </div>
                        </div>
                    </div>

                    <div class="card shadow-sm mb-4">
                        <div class="card-body">
                            <h6 class="fw-bold mb-3">Vé đã đặt</h6>
                            <div v-if="tickets.length" class="table-responsive mb-4">
                                <table class="table table-sm mb-0">
                                    <thead class="table-light">
                                        <tr>
                                            <th>Mã vé</th>
                                            <th>Ghế</th>
                                            <th class="text-end">Giá vé</th>
                                        </tr>
                                    </thead>
                                    <tbody>
                                        <tr v-for="(ticket, index) in tickets" :key="index">
                                            <td>{{ ticket.ma_ve }}</td>
                                            <td><span class="badge bg-primary">{{ ticket.ten_ghe }}</span></td>
                                            <td class="text-end">{{ formatCurrency(ticket.gia_ve) }}</td>
                                        </tr>
                                    </tbody>
                                </table>
                            </div>
                            <div v-else class="text-muted fst-italic">Chưa có vé nào.</div>

                            <h6 class="fw-bold mb-3 mt-4">Dịch vụ đã đặt</h6>
                            <div v-if="services.length" class="table-responsive">
                                <table class="table table-sm mb-0">
                                    <thead class="table-light">
                                        <tr>
                                            <th>Tên dịch vụ</th>
                                            <th class="text-end">Đơn giá</th>
                                            <th class="text-center">SL</th>
                                            <th class="text-end">Thành tiền</th>
                                        </tr>
                                    </thead>
                                    <tbody>
                                        <tr v-for="(service, index) in services" :key="'dv-' + index">
                                            <td>{{ service.ten_dich_vu }}</td>
                                            <td class="text-end">{{ formatCurrency(service.don_gia) }}</td>
                                            <td class="text-center">{{ service.so_luong }}</td>
                                            <td class="text-end">{{ formatCurrency(service.thanh_tien) }}</td>
                                        </tr>
                                    </tbody>
                                </table>
                            </div>
                            <div v-else class="text-muted fst-italic">Không có dịch vụ kèm theo.</div>
                        </div>
                    </div>
                </div>

                <div class="col-lg-4">
                    <div class="card shadow-sm mb-4">
                        <div class="card-body">
                            <h6 class="fw-bold mb-3">Tóm tắt thanh toán</h6>
                            <div class="d-flex justify-content-between mb-2">
                                <span class="text-muted">Tổng tiền vé</span>
                                <span>{{ formatCurrency(summary.tong_tien_ve) }}</span>
                            </div>
                            <div class="d-flex justify-content-between mb-2">
                                <span class="text-muted">Tổng tiền dịch vụ</span>
                                <span>{{ formatCurrency(summary.tong_tien_dich_vu) }}</span>
                            </div>
                            <div class="d-flex justify-content-between mb-2 text-success">
                                <span>Giảm giá</span>
                                <span>-{{ formatCurrency(summary.giam_gia) }}</span>
                            </div>
                            <hr>
                            <div class="d-flex justify-content-between">
                                <span class="fw-bold">Tổng thanh toán</span>
                                <span class="fw-bold text-success fs-5">{{ formatCurrency(summary.tong_thanh_toan) }}</span>
                            </div>
                        </div>
                    </div>

                    <div class="card shadow-sm mb-4">
                        <div class="card-body">
                            <h6 class="fw-bold mb-3">Thông tin khách hàng</h6>
                            <p class="mb-2">
                                <i class="fa-solid fa-user me-2 text-muted"></i>
                                <strong>{{ customer.ho_va_ten }}</strong>
                            </p>
                            <p class="mb-2">
                                <i class="fa-regular fa-envelope me-2 text-muted"></i>
                                {{ customer.email }}
                            </p>
                            <p class="mb-0">
                                <i class="fa-solid fa-phone me-2 text-muted"></i>
                                {{ customer.so_dien_thoai }}
                            </p>
                        </div>
                    </div>

                    <div class="card shadow-sm">
                        <div class="card-body">
                            <h6 class="fw-bold mb-3">Mã giảm giá</h6>
                            <div v-if="voucher" class="d-flex justify-content-between align-items-center">
                                <div>
                                    <strong>{{ voucher.ma_code }}</strong>
                                    <small class="text-muted d-block">
                                        Giảm {{ voucher.so_giam_gia * 100 }}% - tối đa {{ formatCurrency(voucher.so_tien_toi_da) }}
                                    </small>
                                </div>
                                <span class="badge bg-success">Đã áp dụng</span>
                            </div>
                            <div v-else class="text-muted fst-italic">Không áp dụng voucher.</div>
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
    props: ['ma_don_hang'],
    data() {
        return {
            loading: true,
            error: '',
            order: {},
            customer: {},
            movie: null,
            tickets: [],
            services: [],
            voucher: null,
            summary: {
                tong_tien_ve: 0,
                tong_tien_dich_vu: 0,
                giam_gia: 0,
                tong_thanh_toan: 0
            },
            defaultPoster: 'https://via.placeholder.com/200x300?text=Poster'
        }
    },
    computed: {
        moviePoster() {
            return (this.movie && this.movie.hinh_anh) ? this.movie.hinh_anh : this.defaultPoster;
        },
        movieDate() {
            if (!this.movie?.ngay_chieu) return '---';
            const d = new Date(this.movie.ngay_chieu);
            return d.toLocaleDateString('vi-VN');
        },
        movieTime() {
            if (!this.movie?.thoi_gian_bat_dau) return '---';
            return `${this.movie.thoi_gian_bat_dau} - ${this.movie.thoi_gian_ket_thuc}`;
        },
        statusBadgeClass() {
            return this.order.is_thanh_toan ? 'badge bg-success' : 'badge bg-warning text-dark';
        }
    },
    watch: {
        '$route.params.ma_don_hang': {
            immediate: false,
            handler(newVal) {
                if (newVal) {
                    this.fetchOrderDetail(newVal);
                }
            }
        }
    },
    mounted() {
        this.fetchOrderDetail(this.$route.params.ma_don_hang);
    },
    methods: {
        fetchOrderDetail(maDonHang) {
            this.loading = true;
            this.error = '';
            axios.get(apiUrl(`client/chi-tiet-don-hang/${maDonHang}`), {
                headers: {
                    Authorization: "Bearer " + localStorage.getItem('key_client')
                }
            })
                .then((res) => {
                    if (res.data.status) {
                        const data = res.data.data;
                        this.order = data.don_hang;
                        this.customer = data.khach_hang;
                        this.movie = data.phim;
                        this.tickets = data.ve || [];
                        this.services = data.dich_vu || [];
                        this.voucher = data.voucher;
                        this.summary = data.summary;
                    } else {
                        this.error = res.data.message || 'Không thể tải chi tiết đơn hàng.';
                    }
                })
                .catch((error) => {
                    if (error.response && error.response.status === 401) {
                        this.error = 'Bạn cần đăng nhập để xem đơn hàng.';
                    } else if (error.response && error.response.status === 404) {
                        this.error = 'Không tìm thấy đơn hàng.';
                    } else {
                        this.error = 'Có lỗi xảy ra khi tải dữ liệu.';
                    }
                })
                .finally(() => {
                    this.loading = false;
                });
        },
        formatCurrency(value) {
            if (!value && value !== 0) return '0 ₫';
            return new Intl.NumberFormat('vi-VN', { style: 'currency', currency: 'VND' }).format(value);
        },
        formatDateTime(date) {
            if (!date) return '---';
            const d = new Date(date);
            return d.toLocaleString('vi-VN');
        },
        backToBooking() {
            this.$router.back();
        }
    }
}
</script>

<style></style>
