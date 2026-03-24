<template>
    <div class="container py-4 py-md-5">
        <div class="row g-4">
            <div class="col-lg-12">
                <div class="bg-white rounded shadow-sm p-3 p-md-4 mb-4">
                    <!-- Tiêu đề bài viết  -->
                    <h1 class="fs-2 fs-md-1 fw-bold text-dark mb-3 text-uppercase">
                        {{ chi_tiet_bai_viet.tieu_de }}
                    </h1>
                    <div class="d-flex flex-wrap align-items-center gap-3 pb-3 mb-4 border-bottom text-secondary small">
                        <span><i class="bi bi-calendar3 text-danger me-1"></i> {{
                            formatDate(chi_tiet_bai_viet.created_at) }}</span>
                        <div class="ms-auto mt-2 mt-sm-0 d-flex align-items-center">
                            <span class="me-2">Chia sẻ:</span>
                            <a href="#"
                                class="d-inline-flex align-items-center justify-content-center rounded-circle bg-primary text-white me-1"
                                style="width: 32px; height: 32px">
                                <i class="fa-brands fa-2x fa-square-facebook"></i>
                            </a>
                            <a href="#"
                                class="d-inline-flex align-items-center justify-content-center rounded-circle bg-dark text-white me-1"
                                style="width: 32px; height: 32px">
                                <i class="fa-brands fa-2x fa-square-twitter"></i>
                            </a>
                            <a href="#"
                                class="d-inline-flex align-items-center justify-content-center rounded-circle bg-danger text-white"
                                style="width: 32px; height: 32px">
                                <i class="fa-solid fa-2x fa-envelope"></i>
                            </a>
                        </div>
                    </div>

                    <div class="mb-4 rounded overflow-hidden text-center" style="height: 500px; overflow: hidden">
                        <img :src="chi_tiet_bai_viet.hinh_anh" alt="" class="w-100 h-100"
                            style="object-fit: cover; object-position: center" />
                    </div>
                    <div class="lh-lg">
                        <h3 class="fs-4 fw-semibold mb-3">Nội dung bài viết:</h3>
                        <div class="bg-warning-subtle border-start border-4 border-warning rounded p-4 my-4">
                            <div
                                v-html="(chi_tiet_bai_viet.noi_dung || '').replace(/<img /g, '<img style=\'max-width:100%;height:auto;display:block;margin:10px auto;border-radius:8px\' ')">
                            </div>
                        </div>
                    </div>
                    <!-- Tags -->
                    <div class="mt-4 pt-3 border-top">
                        <span class="fw-semibold text-secondary me-2">Tags:</span>
                        <a href="#"
                            class="badge rounded-pill bg-light text-secondary text-decoration-none me-1 mb-2 py-2 px-3">khuyến
                            mãi</a>
                        <a href="#"
                            class="badge rounded-pill bg-light text-secondary text-decoration-none me-1 mb-2 py-2 px-3">mua
                            1 tặng 1</a>
                        <a href="#"
                            class="badge rounded-pill bg-light text-secondary text-decoration-none me-1 mb-2 py-2 px-3">DZ
                            member</a>
                        <a href="#"
                            class="badge rounded-pill bg-light text-secondary text-decoration-none me-1 mb-2 py-2 px-3">vé
                            xem phim</a>
                        <a href="#"
                            class="badge rounded-pill bg-light text-secondary text-decoration-none me-1 mb-2 py-2 px-3">ưu
                            đãi</a>
                    </div>
                </div>
            </div>
            <div class="col-lg-12">
                <div class="bg-white rounded shadow-sm p-3 p-md-4 mb-4">
                    <h3 class="mb-4">Các Bài Viết Liên Quan</h3>
                    <div class="row">
                        <div v-for="(bai_viet, index) in bai_viet_lien_quan.slice(0, 3)" :key="index"
                            class="col-lg-4 d-flex mb-3">
                            <router-link :to="`/chi-tiet-bai-viet/${bai_viet.id}`"
                                class="text-decoration-none text-dark d-flex w-100">
                                <div class="card w-100 d-flex flex-column">
                                    <div class="card-img-top" style="height: 250px; overflow: hidden">
                                        <img :src="bai_viet.hinh_anh" class="w-100 h-100" style="object-fit: cover" />
                                    </div>
                                    <div class="card-body d-flex flex-column flex-grow-1">
                                        <h6 class="card-title">{{ bai_viet.tieu_de }}</h6>
                                        <p class="card-text flex-grow-1">
                                            <small>{{ bai_viet.mo_ta_ngan }}</small>
                                        </p>
                                    </div>
                                </div>
                            </router-link>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
import axios from "axios";
import apiUrl from "../../../utils/api";
export default {
    props: ["id_bai_viet"],
    data() {
        return {
            id_bai_viet: this.$route.params.id_bai_viet,
            chi_tiet_bai_viet: {},
            bai_viet_lien_quan: [],
        };
    },
    mounted() {
        this.loadChiTietBaiViet();
    },
    watch: {
        $route(to, from) {
            // Khi route thay đổi, cập nhật id_bai_viet và load lại dữ liệu
            if (to.params.id_bai_viet !== from.params.id_bai_viet) {
                this.id_bai_viet = to.params.id_bai_viet;
                this.loadChiTietBaiViet();
            }
        },
    },
    methods: {
        loadChiTietBaiViet() {
            var payload = {
                id: this.id_bai_viet,
            };
            axios
                .post(apiUrl("client/chi-tiet-bai-viet/get-data"), payload, {
                    headers: {
                        Authorization: "Bearer " + localStorage.getItem("key_client"),
                    },
                })
                .then((res) => {
                    if (res.data.status) {
                        this.chi_tiet_bai_viet = res.data.data;
                        console.log(this.chi_tiet_bai_viet);
                        this.bai_viet_lien_quan = res.data.data_bai_viet_khac;
                        console.log(this.bai_viet_lien_quan);
                    } else {
                        this.$toast.error(res.data.message);
                        this.$router.push("/");
                    }
                })
                .catch((error) => {
                    console.error("Lỗi khi load chi tiết bài viết:", error);
                    if (error.response) {
                        this.$toast.error(error.response.data?.message || "Có lỗi xảy ra");
                    } else {
                        this.$toast.error("Không thể kết nối đến server");
                    }
                });
        },
        formatDate(dateString) {
            if (!dateString) return '';
            const date = new Date(dateString);
            const day = String(date.getDate()).padStart(2, '0');
            const month = String(date.getMonth() + 1).padStart(2, '0');
            const year = date.getFullYear();
            const hours = String(date.getHours()).padStart(2, '0');
            const minutes = String(date.getMinutes()).padStart(2, '0');
            return `${hours}:${minutes} - ${day}/${month}/${year}`;
        },
    },

};

</script>
