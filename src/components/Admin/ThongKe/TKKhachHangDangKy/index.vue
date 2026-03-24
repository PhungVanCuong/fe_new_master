<template>
    <div class="row">
        <div class="col-lg-12">
            <div class="card">
                <div class="card-header mt-2">
                    <h4 class="fw-bold text-primary">THỐNG KÊ KHÁCH HÀNG MỚI</h4>
                </div>
                <div class="card-body">
                    <div class="mb-3">
                        <div class="row g-3 align-items-center">
                            <div class="col-lg-5 col-md-6">
                                <label for="">Từ ngày</label>
                                <input v-model="search.begin" type="date" class="form-control mt-2 mb-2 w-100" />
                            </div>
                            <div class="col-lg-5 col-md-6">
                                <label for="">Đến ngày</label>
                                <input v-model="search.end" type="date" class="form-control mt-2 mb-2" />
                            </div>
                            <div class="col-lg-2 col-md-12">
                                <label for="">&nbsp;</label>
                                <button @click="thongKe()" class="btn btn-primary w-100">
                                    Thống Kê
                                </button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
    <div class="row">
        <div class="col-lg-4">
            <div class="card">
                <div class="card-body">
                    <div class="table-responsive">
                        <table class="table table-hover table-bordered">
                            <thead class="table-light">
                                <tr class="table-warning">
                                    <th class="text-center">Ngày</th>
                                    <th class="text-center">Số Lượng Khách Hàng Mới</th>
                                </tr>
                            </thead>
                            <tbody>
                                <template v-for="(v, i) in list_data" :key="i">
                                    <tr>
                                        <td class="text-center">{{ v.ngay_dang_ky }}</td>
                                        <td class="text-center">{{ v.so_khach_hang_moi }}</td>
                                    </tr>
                                </template>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </div>
        <div class="col-lg-8">
            <div class="card">
                <div class="card-body" style="height: 500px">
                    <Line v-if="is_view == true" id="my-chart-id" :options="chartOptions" :data="chartData" />
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import { Line } from "vue-chartjs";
import {
    Chart as ChartJS,
    Title,
    Tooltip,
    Legend,
    LineElement,
    PointElement,
    CategoryScale,
    LinearScale,
} from "chart.js";
import axios from "axios";
import apiUrl from "../../../../utils/api";

ChartJS.register(
    Title,
    Tooltip,
    Legend,
    LineElement,
    PointElement,
    CategoryScale,
    LinearScale
);
export default {
    name: "ThongKeKhachHangDangKyChart",
    components: { Line },
    data() {
        return {
            search: {},
            list_data: [],
            is_view: false,
            chartData: {
                labels: [],
                datasets: [],
            },
            chartOptions: {
                responsive: true,
                maintainAspectRatio: false,
                scales: {
                    y: {
                        beginAtZero: true,
                    },
                },
                plugins: {
                    legend: {
                        display: true,
                    },
                },
            },
        };
    },
    methods: {
        thongKe() {
            axios
                .post(apiUrl("admin/thong-ke/khach-hang-dang-ky"), this.search, {
                    headers: {
                        Authorization: "Bearer " + localStorage.getItem("key_admin"),
                    },
                })
                .then((res) => {
                    if (res.data.status) {
                        this.is_view = true;
                        this.list_data = res.data.data;
                        this.chartData = {
                            labels: res.data.labels,
                            datasets: res.data.datasets.map((dataset) => ({
                                ...dataset,
                                tension: 0.4,
                                pointRadius: 5,
                                pointHoverRadius: 7,
                                fill: true,
                            })),
                        };
                    } else {
                        this.$toast.error(res.data.message);
                    }
                });
        },
    },
};
</script>
<style></style>