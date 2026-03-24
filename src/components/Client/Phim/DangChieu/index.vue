<template>
    <div id="carouselExample" class="carousel slide" data-bs-ride="carousel" data-bs-interval="2000">
        <div class="carousel-inner">
            <template v-for="(value, index) in list_slide" :key="index">
                <div class="carousel-item" :class="{ 'active': index === 0 }">
                    <img :src="value.hinh_anh" class="d-block w-100" :alt="value.tieu_de || 'Slide'"
                        style="height: 700px; object-fit: contain; object-position: center; background-color: #000;">
                </div>
            </template>
        </div>

        <button class="carousel-control-prev" type="button" data-bs-target="#carouselExample" data-bs-slide="prev">
            <span class="carousel-control-prev-icon" aria-hidden="true"></span>
            <span class="visually-hidden">Previous</span>
        </button>
        <button class="carousel-control-next" type="button" data-bs-target="#carouselExample" data-bs-slide="next">
            <span class="carousel-control-next-icon" aria-hidden="true"></span>
            <span class="visually-hidden">Next</span>
        </button>
    </div>
    <div class="container mt-3">
        <div class="row">
            <div class="text-center mb-3">
                <h4 class="text-uppercase fs-3 text-white">Phim Đang Chiếu</h4>
            </div>
            <template v-for="(value, index) in list_phim" :key="index">
                <div class="col-lg-3 col-md-4 rounded mb-3" style="flex: 0 0 auto;">
                    <div class="rounded position-relative"
                        style="transition: transform 0.3s ease, box-shadow 0.3s ease; overflow: hidden; height: 100%;"
                        onmouseover="this.style.transform='translateY(-8px)'; this.style.boxShadow='0 8px 16px rgba(0,0,0,0.2)'; this.querySelector('.btn-overlay').style.opacity = '1'"
                        onmouseout="this.style.transform='none'; this.style.boxShadow='none'; this.querySelector('.btn-overlay').style.opacity = '0'">

                        <div class="card-img-top">
                            <img :src="value.hinh_anh" class="img-fluid" alt=""
                                style="height: 500px; object-fit: cover;">
                        </div>

                        <div class="btn-overlay text-center position-absolute w-100"
                            style="top: 50%; left: 50%; transform: translate(-50%, -50%); opacity: 0; transition: opacity 0.3s ease;">
                            <router-link :to="`/chi-tiet-phim/${value.id}`">
                                <button class="btn btn-warning p-2 " style="width: 170px;"><i
                                        class="fa-solid fa-ticket"></i>Chi tiết</button>
                            </router-link>
                            <br>
                            <a :href="value.trailer" target="_blank">
                                <button class="btn btn-outline-light p-2 mt-2" style="width: 170px;"><i
                                        class="fa-solid fa-circle-play"></i>Trailer</button>
                            </a>

                        </div>
                        <span class="text-truncate text-white fs-6 fw-bold px-2 pt-2 pb-0">{{ value.ten_phim }}</span>
                    </div>
                </div>
            </template>
        </div>
    </div>

</template>
<script>
import axios from 'axios';
import apiUrl from '../../../../utils/api'
export default {
    data() {
        return {
            list_phim: [],
            list_slide: []

        }
    },
    mounted() {
        this.getPhim();
        this.loadData();
    },
    methods: {
        getPhim() {
            axios
                .get(apiUrl('client/phim-dang-chieu/get-data'), {
                    headers: {
                        Authorization: "Bearer " + localStorage.getItem("key_client"),
                    },
                })
                .then((res) => {
                    if (res.data.status) {
                        this.list_phim = res.data.data;
                    }
                })
        },
        loadData() {
            axios
                .get(apiUrl('client/trang-chu/get-data'), {
                    headers: {
                        Authorization: "Bearer " + localStorage.getItem("key_client"),
                    },
                })
                .then((res) => {
                    this.list_slide = res.data.data_slide || [];
                })
        }
    },
};
</script>
<style></style>