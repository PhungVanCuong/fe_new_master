<template>
    <div class="row">
        <div class="col-lg-12">
            <div class="card radius-10 border-top border-0 border-3 border-info">
                <div class="card-header d-flex justify-content-between">
                    <h4 class="mt-2"><b>DANH SÁCH BÌNH LUẬN</b></h4>
                </div>
                <div class="card-body table-responsive">
                    <!-- Loading State -->
                    <div v-if="loading" class="text-center py-5">
                        <div class="spinner-border text-primary" role="status">
                            <span class="visually-hidden">Đang tải...</span>
                        </div>
                    </div>

                    <!-- Error State -->
                    <div v-else-if="error" class="alert alert-danger">
                        <i class="fas fa-exclamation-triangle me-2"></i>{{ error }}
                    </div>

                    <!-- Data Table -->
                    <table v-else class="table table-bordered table-hover">
                        <thead>
                            <tr class="bg-primary text-light text-nowrap">
                                <th class="text-center">#</th>
                                <th class="text-center">Khách Hàng</th>
                                <th class="text-center">Tên Phim</th>
                                <th class="text-center">Nội Dung</th>
                                <th class="text-center">Ngày Tạo</th>
                                <th class="text-center">Trạng Thái</th>
                                <th class="text-center">Action</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr v-if="listBinhLuan.length === 0">
                                <td colspan="7" class="text-center py-4 text-muted">
                                    <i class="fas fa-inbox fa-2x mb-2"></i>
                                    <p>Chưa có bình luận nào</p>
                                </td>
                            </tr>
                            <tr v-for="(item, index) in listBinhLuan" :key="item.id" class="text-nowrap">
                                <th class="align-middle text-center">{{ index + 1 }}</th>
                                <td class="align-middle">{{ item.ho_va_ten }}</td>
                                <td class="align-middle">{{ item.ten_phim }}</td>
                                <td class="align-middle" style="max-width: 400px; white-space: normal;">
                                    {{ item.noi_dung }}
                                </td>
                                <td class="align-middle text-center">
                                    {{ formatDate(item.created_at) }}
                                </td>
                                <td @click="doiTrangThai(item)" class="align-middle text-center" style="width: 180px; cursor: pointer;">
                                    <button v-if="item.is_hien_thi == 1" type="button" class="btn btn-success w-100 btn-sm">
                                        Hiển Thị
                                    </button>
                                    <button v-else type="button" class="btn btn-warning w-100 btn-sm">
                                        Tạm Ẩn
                                    </button>
                                </td>
                                <td class="align-middle text-center">
                                    <button @click="selectDelete(item)" class="btn btn-danger btn-sm" data-bs-toggle="modal" data-bs-target="#deleteModal">
                                        <i class="fa-solid fa-square-xmark"></i>
                                    </button>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>

    <!-- Modal Xóa -->
    <div class="modal fade" id="deleteModal" tabindex="-1" aria-hidden="true">
        <div class="modal-dialog">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title">Xóa Bình Luận</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">
                    <div class="alert alert-danger" role="alert">
                        <p><strong>Bạn có chắc chắn muốn xóa bình luận này không?</strong></p>
                        <hr>
                        <p class="mb-0"><strong>Khách hàng:</strong> {{ selectedItem?.ho_va_ten }}</p>
                        <p class="mb-0"><strong>Phim:</strong> {{ selectedItem?.ten_phim }}</p>
                        <p class="mb-0"><strong>Nội dung:</strong> {{ selectedItem?.noi_dung }}</p>
                    </div>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Đóng</button>
                    <button type="button" class="btn btn-danger" @click="deleteBinhLuan" data-bs-dismiss="modal">
                        Xác nhận
                    </button>
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
            error: null,
            listBinhLuan: [],
            selectedItem: null
        };
    },
    mounted() {
        this.loadData();
    },
    methods: {
        loadData() {
            this.loading = true;
            this.error = null;
            axios
                .get(apiUrl('admin/binh-luan/get-data'), {
                    headers: {
                        Authorization: "Bearer " + localStorage.getItem('key_admin')
                    }
                })
                .then((res) => {
                    if (res.data.status === 1) {
                        this.listBinhLuan = res.data.data;
                        this.$toast.success(res.data.message);
                    } else {
                        this.error = res.data.message || 'Không thể tải dữ liệu';
                        this.$toast.error(res.data.message);
                    }
                    this.loading = false;
                })
                .catch((res) => {
                    this.loading = false;
                    if (res.response?.status === 401) {
                        this.error = 'Phiên đăng nhập đã hết hạn. Vui lòng đăng nhập lại.';
                        this.$toast.error(this.error);
                        setTimeout(() => {
                            this.$router.push('/admin/login');
                        }, 2000);
                    } else {
                        this.error = 'Có lỗi xảy ra khi tải dữ liệu bình luận.';
                        const list = Object.values(res.response?.data?.errors || {});
                        list.forEach((v, i) => {
                            this.$toast.error(v[0]);
                        });
                    }
                });
        },

        doiTrangThai(item) {
            axios
                .post(apiUrl('admin/binh-luan/doi-trang-thai'), { id: item.id }, {
                    headers: {
                        Authorization: "Bearer " + localStorage.getItem('key_admin')
                    }
                })
                .then((res) => {
                    if (res.data.status === 1) {
                        this.$toast.success(res.data.message);
                        item.is_hien_thi = item.is_hien_thi === 1 ? 0 : 1;
                    } else {
                        this.$toast.error(res.data.message);
                    }
                })
                .catch((res) => {
                    const list = Object.values(res.response?.data?.errors || {});
                    list.forEach((v, i) => {
                        this.$toast.error(v[0]);
                    });
                });
        },

        selectDelete(item) {
            this.selectedItem = item;
        },

        deleteBinhLuan() {
            if (!this.selectedItem) return;
            
            axios
                .post(apiUrl('admin/binh-luan/delete'), { id: this.selectedItem.id }, {
                    headers: {
                        Authorization: "Bearer " + localStorage.getItem('key_admin')
                    }
                })
                .then((res) => {
                    if (res.data.status === 1) {
                        this.$toast.success(res.data.message);
                        this.listBinhLuan = this.listBinhLuan.filter(
                            item => item.id !== this.selectedItem.id
                        );
                        this.selectedItem = null;
                    } else {
                        this.$toast.error(res.data.message);
                    }
                })
                .catch((res) => {
                    const list = Object.values(res.response?.data?.errors || {});
                    list.forEach((v, i) => {
                        this.$toast.error(v[0]);
                    });
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
            return `${day}/${month}/${year} ${hours}:${minutes}`;
        }
    }
};
</script>
<style></style>