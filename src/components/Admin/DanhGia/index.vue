<template>
    <div>
        <div class="row">
            <div class="col-lg-12">
                <div class="card radius-10 border-top border-0 border-3 border-info">
                    <div class="card-header d-flex justify-content-between">
                        <h4 class="mt-2"><b>QUẢN LÝ ĐÁNH GIÁ</b></h4>
                    </div>
                    <div class="card-body table-responsive">
                        <div v-if="loading" class="text-center py-5">
                            <div class="spinner-border text-primary" role="status"></div>
                        </div>

                        <table v-else class="table table-bordered table-hover">
                            <thead>
                                <tr class="bg-primary text-light text-nowrap text-center">
                                    <th>#</th>
                                    <th>Khách Hàng</th>
                                    <th>Tên Phim</th>
                                    <th>Số Sao</th>
                                    <th>Nội Dung</th>
                                    <th>Ngày Đánh Giá</th>
                                    <th>Trạng Thái</th>
                                    <th>Action</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for="(item, index) in listDanhGia" :key="item.id">
                                    <td class="text-center align-middle">{{ index + 1 }}</td>
                                    <td class="align-middle">{{ item.ho_va_ten }}</td>
                                    <td class="align-middle">{{ item.ten_phim }}</td>
                                    <td class="text-center align-middle">
                                        <span v-for="star in 5" :key="star">
                                            <i class="fa-star" :class="star <= item.so_sao ? 'fa-solid text-warning' : 'fa-regular text-secondary'"></i>
                                        </span>
                                    </td>
                                    <td class="align-middle" style="max-width: 300px; white-space: normal;">
                                        {{ item.noi_dung }}
                                    </td>
                                    <td class="text-center align-middle">{{ formatDate(item.created_at) }}</td>
                                    <td class="text-center align-middle">
                                        <button @click="doiTrangThai(item)" 
                                                :class="item.tinh_trang == 1 ? 'btn btn-success' : 'btn btn-warning'" 
                                                class="btn btn-sm w-100 text-light">
                                            {{ item.tinh_trang == 1 ? 'Hiển Thị' : 'Tạm Ẩn' }}
                                        </button>
                                    </td>
                                    <td class="text-center align-middle">
                                        <button @click="selectedItem = item" class="btn btn-danger btn-sm" data-bs-toggle="modal" data-bs-target="#deleteModal">
                                            <i class="fa-solid fa-trash"></i>
                                        </button>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </div>

        <div class="modal fade" id="deleteModal" tabindex="-1" aria-hidden="true">
            <div class="modal-dialog">
                <div class="modal-content" v-if="selectedItem">
                    <div class="modal-header">
                        <h5 class="modal-title">Xóa Đánh Giá</h5>
                        <button type="button" class="btn-close" data-bs-dismiss="modal"></button>
                    </div>
                    <div class="modal-body">
                        Bạn có chắc chắn muốn xóa đánh giá của <b>{{ selectedItem.ho_va_ten }}</b>?
                    </div>
                    <div class="modal-footer">
                        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Đóng</button>
                        <button type="button" class="btn btn-danger" @click="deleteDanhGia" data-bs-dismiss="modal">Xác nhận</button>
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
            loading: false,
            listDanhGia: [],
            selectedItem: null
        };
    },
    mounted() {
        this.loadData();
    },
    methods: {
        loadData() {
            this.loading = true;
            axios.get(apiUrl('admin/danh-gia/get-data'), {
                headers: { Authorization: "Bearer " + localStorage.getItem('key_admin') }
            }).then((res) => {
                if(res.data.status) {
                    this.listDanhGia = res.data.data;
                }
                this.loading = false;
            }).catch(() => {
                // 4. Đảm bảo nếu API lỗi thì vòng xoay cũng dừng lại
                this.loading = false; 
                this.$toast.error('Lỗi khi tải dữ liệu!');
            });
        },
        doiTrangThai(item) {
            axios.post(apiUrl('admin/danh-gia/doi-trang-thai'), { id: item.id }, {
                headers: { Authorization: "Bearer " + localStorage.getItem('key_admin') }
            }).then((res) => {
                if (res.data.status) {
                    this.$toast.success(res.data.message);
                    item.tinh_trang = item.tinh_trang == 1 ? 0 : 1;
                } else {
                    this.$toast.error(res.data.message);
                }
            }).catch((err) => {
                this.$toast.error(err.response?.data?.message || 'Lỗi hệ thống');
            });
        },
        deleteDanhGia() {
            axios.post(apiUrl('admin/danh-gia/delete'), { id: this.selectedItem.id }, {
                headers: { Authorization: "Bearer " + localStorage.getItem('key_admin') }
            }).then((res) => {
                if (res.data.status) {
                    this.$toast.success(res.data.message);
                    this.loadData();
                } else {
                    this.$toast.error(res.data.message);
                }
            }).catch((err) => {
                this.$toast.error(err.response?.data?.message || 'Lỗi hệ thống');
            });
        },
        formatDate(d) {
            if (!d) return '';
            return new Date(d).toLocaleString('vi-VN');
        }
    }
};
</script>

<style></style>