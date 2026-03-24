<template>
    <div class="row">
        <div class="col">
            <h2 class="text-white text-center mt-3">DANH SÁCH BÀI VIẾT</h2>
        </div>
    </div>

    <div class="container mt-4">
        <div class="row" v-if="list_bv.length > 0">

            <div class="col-lg-6">
                <div class="card mb-4 border-0 shadow-sm">
                    <div class="row g-0">
                        <div class="col-lg-12">
                            <router-link :to="`/chi-tiet-bai-viet/${list_bv[0].id}`"
                                class="text-decoration-none text-dark">
                                <div class="card h-100">
                                    <img :src="list_bv[0].hinh_anh" class="card-img-top"
                                        style="height: 350px; object-fit: cover;" alt="...">
                                    <div class="card-body">
                                        <h5 class="card-title fw-bold">{{ list_bv[0].tieu_de }}</h5>
                                        <p class="card-text text-secondary text-truncate-3">
                                            {{ list_bv[0].mo_ta_ngan }}
                                        </p>
                                    </div>
                                </div>
                            </router-link>
                        </div>
                    </div>
                </div>

                <h5 class="fw-bold border-start border-4 border-primary ps-2 mb-3 text-white">Tin Mới Khác</h5>
                <hr>

                <div class="row">
                    <template v-for="(v, k) in list_bv.slice(1, 3)" :key="k">
                        <div class="col-lg-6 d-flex mb-3">
                            <router-link :to="`/chi-tiet-bai-viet/${v.id}`"
                                class="text-decoration-none text-dark w-100">
                                <div class="card h-100 shadow-sm">
                                    <img :src="v.hinh_anh" class="card-img-top"
                                        style="height: 180px; object-fit: cover;" alt="...">
                                    <div class="card-body">
                                        <h6 class="card-title fw-bold text-truncate-2">{{ v.tieu_de }}</h6>
                                        <p class="card-text small text-secondary text-truncate-2">
                                            {{ v.mo_ta_ngan }}
                                        </p>
                                    </div>
                                </div>
                            </router-link>
                        </div>
                    </template>
                </div>
            </div>

            <div class="col-lg-6">
                <div class="card p-3 shadow-sm h-100">
                    <h5 class="fw-bold mb-3">Danh Sách Tin Tức</h5>

                    <template v-for="(v, k) in list_bv.slice(3)" :key="k">
                        <div class="card mb-3 border-0">
                            <router-link :to="`/chi-tiet-bai-viet/${v.id}`" class="text-decoration-none text-dark">
                                <div class="row g-2">
                                    <div class="col-4">
                                        <img :src="v.hinh_anh" class="img-fluid rounded"
                                            style="height: 100px; width: 100%; object-fit: cover;" alt="...">
                                    </div>
                                    <div class="col-8">
                                        <div class="card-body p-0 ps-2">
                                            <h6 class="card-title fw-bold text-truncate-2 mb-1">{{ v.tieu_de }}</h6>
                                            <p class="card-text small text-secondary text-truncate-2 mb-0">
                                                {{ v.mo_ta_ngan }}
                                            </p>
                                            <small class="text-muted" style="font-size: 11px;">
                                                <i class="fa-regular fa-clock me-1"></i> {{ formatDate(v.created_at) }}
                                            </small>
                                        </div>
                                    </div>
                                </div>
                            </router-link>
                        </div>
                        <hr class="my-2 text-muted" v-if="k < list_bv.slice(3).length - 1">
                    </template>

                </div>
            </div>
        </div>

        <div v-else class="text-center py-5">
            <div class="spinner-border text-primary" role="status">
                <span class="visually-hidden">Loading...</span>
            </div>
            <p class="mt-2 text-muted">Đang tải danh sách bài viết...</p>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
import apiUrl from '../../../utils/api'

export default {
    data() {
        return {
            list_bv: [],
        }
    },
    mounted() {
        this.loadBaiViet();
    },
    methods: {
        loadBaiViet() {
            axios.get(apiUrl('client/trang-chu/get-data'))
                .then((res) => {
                    // Giả sử API trả về { status: true, data: [...] }
                    this.list_bv = res.data.data_bv;
                })
                .catch((err) => {
                    console.error("Lỗi tải bài viết:", err);
                });
        },
        formatDate(dateString) {
            if (!dateString) return '';
            const date = new Date(dateString);
            return date.toLocaleDateString('vi-VN');
        }
    }
}
</script>

<style scoped>
/* CSS hỗ trợ cắt chữ nếu quá dài */
.text-truncate-2 {
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
    overflow: hidden;
}

.text-truncate-3 {
    display: -webkit-box;
    -webkit-line-clamp: 3;
    -webkit-box-orient: vertical;
    overflow: hidden;
}

/* Hiệu ứng hover cho card */
.card {
    transition: transform 0.2s;
    background-color: #F0EEE9;
    border: 2px solid #E3CC81;
}

.card:hover {
    transform: translateY(-2px);

}
</style>