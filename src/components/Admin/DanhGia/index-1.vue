<!-- <template>
    <div class="row">
        <div class="col-lg-12">
            <div class="card radius-10 border-top border-0 border-3 border-info">
                <div class="card-header d-flex justify-content-between">
                    <h4 class="mt-2"><b>QUẢN LÝ ĐÁNH GIÁ PHIM</b></h4>
                </div>
                <div class="card-body table-responsive">
                    <div v-if="loading" class="text-center py-5">
                        <div class="spinner-border text-primary" role="status">
                            <span class="visually-hidden">Đang tải...</span>
                        </div>
                    </div>

                    <div v-else-if="error" class="alert alert-danger">
                        <i class="fas fa-exclamation-triangle me-2"></i>{{ error }}
                    </div>

                    <table v-else class="table table-bordered table-hover">
                        <thead>
                            <tr class="bg-primary text-light text-nowrap">
                                <th class="text-center">#</th>
                                <th class="text-center">Khách Hàng</th>
                                <th class="text-center">Tên Phim</th>
                                <th class="text-center">Số Sao</th>
                                <th class="text-center">Nội Dung</th>
                                <th class="text-center">Ngày Đánh Giá</th>
                                <th class="text-center">Trạng Thái</th>
                                <th class="text-center">Hành Động</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr v-if="listDanhGia.length === 0">
                                <td colspan="8" class="text-center py-4 text-muted">
                                    <i class="fas fa-star-half-alt fa-2x mb-2"></i>
                                    <p>Chưa có đánh giá nào</p>
                                </td>
                            </tr>
                            <tr v-for="(item, index) in listDanhGia" :key="item.id" class="text-nowrap">
                                <th class="align-middle text-center">{{ index + 1 }}</th>
                                <td class="align-middle">{{ item.ho_va_ten }}</td>
                                <td class="align-middle">{{ item.ten_phim }}</td>
                                <td class="align-middle text-center">
                                    <span class="badge bg-warning text-dark px-3 py-2">
                                        {{ item.so_sao }} <i class="fa-solid fa-star"></i>
                                    </span>
                                </td>
                                <td class="align-middle" style="max-width: 350px; white-space: normal;">
                                    {{ item.noi_dung }}
                                </td>
                                <td class="align-middle text-center">
                                    {{ formatDate(item.created_at) }}
                                </td>
                                <td @click="doiTrangThai(item)" class="align-middle text-center"
                                    style="width: 150px; cursor: pointer;">
                                    <button v-if="item.tinh_trang == 1" type="button"
                                        class="btn btn-success w-100 btn-sm">
                                        Đang Hiện
                                    </button>
                                    <button v-else type="button" class="btn btn-secondary w-100 btn-sm">
                                        Đang Ẩn
                                    </button>
                                </td>
                                <td class="align-middle text-center">
                                    <button @click="selectDelete(item)" class="btn btn-danger btn-sm"
                                        data-bs-toggle="modal" data-bs-target="#deleteModal">
                                        <i class="fa-solid fa-trash-can"></i>
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
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title">Xác Nhận Xóa Đánh Giá</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">
                    <div class="alert alert-danger" role="alert">
                        <p><strong>Hành động này không thể hoàn tác!</strong></p>
                        <hr>
                        <p class="mb-0"><strong>Khách hàng:</strong> {{ selectedItem?.ho_va_ten }}</p>
                        <p class="mb-0"><strong>Đánh giá:</strong> {{ selectedItem?.so_sao }} sao</p>
                        <p class="mb-0"><strong>Nội dung:</strong> {{ selectedItem?.noi_dung }}</p>
                    </div>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Hủy</button>
                    <button type="button" class="btn btn-danger" @click="deleteDanhGia" data-bs-dismiss="modal">
                        Xác nhận xóa
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
            this.error = null;
            axios
                .get(apiUrl('admin/danh-gia/get-data'), {
                    headers: {
                        Authorization: "Bearer " + localStorage.getItem('key_admin')
                    }
                })
                .then((res) => {
                    if (res.data.status) {
                        this.listDanhGia = res.data.data;
                    } else {
                        this.error = res.data.message || 'Không thể tải dữ liệu';
                        this.$toast.error(res.data.message);
                    }
                    this.loading = false;
                })
                .catch((err) => {
                    this.loading = false;
                    this.error = 'Có lỗi xảy ra khi tải dữ liệu đánh giá.';
                    console.error(err);
                });
        },

        doiTrangThai(item) {
            axios
                .post(apiUrl('admin/danh-gia/doi-tinh-trang'), { id: item.id }, {
                    headers: {
                        Authorization: "Bearer " + localStorage.getItem('key_admin')
                    }
                })
                .then((res) => {
                    if (res.data.status) {
                        this.$toast.success(res.data.message);
                        // Cập nhật giao diện ngay lập tức
                        item.tinh_trang = item.tinh_trang === 1 ? 0 : 1;
                    } else {
                        this.$toast.error(res.data.message);
                    }
                });
        },

        selectDelete(item) {
            this.selectedItem = item;
        },

        deleteDanhGia() {
            if (!this.selectedItem) return;

            axios
                .post(apiUrl('admin/danh-gia/delete'), { id: this.selectedItem.id }, {
                    headers: {
                        Authorization: "Bearer " + localStorage.getItem('key_admin')
                    }
                })
                .then((res) => {
                    if (res.data.status) {
                        this.$toast.success(res.data.message);
                        this.listDanhGia = this.listDanhGia.filter(
                            i => i.id !== this.selectedItem.id
                        );
                        this.selectedItem = null;
                    } else {
                        this.$toast.error(res.data.message);
                    }
                });
        },

        formatDate(dateString) {
            if (!dateString) return '';
            const date = new Date(dateString);
            return date.toLocaleString('vi-VN');
        }
    }
};
</script> -->