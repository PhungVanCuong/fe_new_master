<template>
    <div class="row">
        <div class="col-lg-12">
            <div class="card radius-10 border-top border-0 border-3 border-info">
                <div class="card-header d-flex justify-content-between">
                    <h4 class="mt-2"><b>DANH SÁCH SUẤT CHIẾU</b></h4>
                    <button class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#addModal">
                        Thêm Suất Chiếu
                    </button>
                </div>
                <div class="card-body table-responsive">
                    <div class="input-group mb-3">
                        <input type="text" class="form-control" placeholder="Tìm kiếm khách hàng...">
                        <button class="btn btn-success input-group-text" style="width: 155px;">Tìm
                            kiếm</button>
                    </div>
                    <div class="table-responsive">
                        <table class="table table-bordered table-hover">
                            <thead>
                                <tr class="bg-primary text-light text-nowrap">
                                    <th class="text-center">#</th>
                                    <th class="text-center">Tên Phim</th>
                                    <th class="text-center">Phòng Chiếu</th>
                                    <th class="text-center">Ngày Chiếu</th>
                                    <th class="text-center">Giờ Bắt Đầu</th>
                                    <th class="text-center">Giờ Kết Thúc</th>
                                    <th class="text-center">Tình Trạng</th>
                                    <th class="text-center">Action</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr class="text-nowrap" v-for="(item, index) in list_suat_chieu" :key="index">
                                    <th class="align-middle text-center">{{ index + 1 }}</th>
                                    <td class="align-middle">{{ item.ten_phim }}</td>
                                    <td class="align-middle text-center">{{ item.ten_phong }}</td>
                                    <td class="align-middle text-center">{{ item.ngay_chieu }}</td>
                                    <td class="align-middle text-center">{{ item.thoi_gian_bat_dau }}</td>
                                    <td class="align-middle text-center">{{ item.thoi_gian_ket_thuc }}</td>
                                    <td class="align-middle text-center">
                                        <button v-if="item.tinh_trang == 1" class="btn btn-success w-100">
                                            Hoạt Động
                                        </button>
                                        <button v-else class="btn btn-danger w-100">
                                            Đã Hủy
                                        </button>
                                    </td>
                                    <td class="align-middle text-center" style="width: 200px;">
                                        <button class="btn btn-primary me-2" data-bs-toggle="modal"
                                            data-bs-target="#createModal"
                                            v-on:click="setSuatChieuSelected(item)">
                                            Tạo Vé
                                        </button>
                                        <button class="btn btn-info text-light me-2" data-bs-toggle="modal"
                                            data-bs-target="#updateModal"
                                            v-on:click="Object.assign(update_suat_chieu, item)">
                                            Cập nhật
                                        </button>
                                        <button class="btn btn-danger" data-bs-toggle="modal"
                                            data-bs-target="#deleteModal"
                                            v-on:click="Object.assign(delete_suat_chieu, item)">
                                            Xóa
                                        </button>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Modal Thêm Mới -->
    <div class="modal fade" id="addModal" tabindex="-1" aria-hidden="true">
        <div class="modal-dialog modal-xl">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title">Thêm Suất Chiếu Mới</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">
                    <div class="row">
                        <div class="col-md-6 mb-3">
                            <label class="form-label">Phim</label>
                            <select v-model="create_suat_chieu.id_phim" class="form-select">
                                <option v-for="(item, index) in list_phim" :key="index" :value="item.id">
                                    {{ item.ten_phim }}
                                </option>
                            </select>
                        </div>
                        <div class="col-md-6 mb-3">
                            <label class="form-label">Phòng Chiếu</label>
                            <select v-model="create_suat_chieu.id_phong_chieu" class="form-select">
                                <option v-for="(item, index) in list_phong_chieu" :key="index" :value="item.id">
                                    {{ item.ten_phong }}
                                </option>
                            </select>
                        </div>
                        <div class="col-md-6 mb-3">
                            <label class="form-label">Giờ Bắt Đầu</label>
                            <input v-model="create_suat_chieu.thoi_gian_bat_dau" type="time" class="form-control" />
                        </div>
                        <div class="col-md-6 mb-3">
                            <label class="form-label">Giờ Kết Thúc</label>
                            <input v-model="create_suat_chieu.thoi_gian_ket_thuc" type="time" class="form-control" />
                        </div>
                        <div class="col-md-6 mb-3">
                            <label class="form-label">Ngày Chiếu</label>
                            <input v-model="create_suat_chieu.ngay_chieu" type="date" class="form-control" />
                        </div>
                        <div class="col-md-6 mb-3">
                            <label class="form-label">Tình Trạng</label>
                            <select v-model="create_suat_chieu.tinh_trang" class="form-select">
                                <option value="1">Hoạt Động</option>
                                <option value="0">Tạm Tắt</option>
                            </select>
                        </div>
                    </div>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">
                        Đóng
                    </button>
                    <button type="button" class="btn btn-primary" data-bs-dismiss="modal" @click="addSuatChieu()">
                        Thêm mới
                    </button>
                </div>
            </div>
        </div>
    </div>

    <!-- Modal Cập Nhật -->
    <div class="modal fade" id="updateModal" tabindex="-1" aria-hidden="true">
        <div class="modal-dialog modal-xl">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title">Cập Nhật Thông Tin Suất Chiếu</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">
                    <div class="row">
                        <div class="col-md-6 mb-3">
                            <label class="form-label">Phim</label>
                            <select v-model="update_suat_chieu.id_phim" class="form-select">
                                <option value="0">Chọn phim</option>
                                <option v-for="(item, index) in list_phim" :key="index" :value="item.id">
                                    {{ item.ten_phim }}
                                </option>
                            </select>
                        </div>
                        <div class="col-md-6 mb-3">
                            <label class="form-label">Phòng Chiếu</label>
                            <select v-model="update_suat_chieu.id_phong_chieu" class="form-select">
                                <option value="0">Chọn phòng chiếu</option>
                                <option v-for="(item, index) in list_phong_chieu" :key="index" :value="item.id">
                                    {{ item.ten_phong }}
                                </option>
                            </select>
                        </div>
                        <div class="col-md-6 mb-3">
                            <label class="form-label">Giờ Bắt Đầu</label>
                            <input v-model="update_suat_chieu.thoi_gian_bat_dau" type="time" class="form-control" />
                        </div>
                        <div class="col-md-6 mb-3">
                            <label class="form-label">Giờ Kết Thúc</label>
                            <input v-model="update_suat_chieu.thoi_gian_ket_thuc" type="time" class="form-control" />
                        </div>
                        <div class="col-md-6 mb-3">
                            <label class="form-label">Ngày Chiếu</label>
                            <input v-model="update_suat_chieu.ngay_chieu" type="date" class="form-control" />
                        </div>
                        <div class="col-md-6 mb-3">
                            <label class="form-label">Tình Trạng</label>
                            <select v-model="update_suat_chieu.tinh_trang" class="form-select">
                                <option value="1">Hoạt Động</option>
                                <option value="0">Tạm Tắt</option>
                            </select>
                        </div>
                    </div>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">
                        Đóng
                    </button>
                    <button type="button" class="btn btn-success" data-bs-dismiss="modal" @click="updateSuatChieu()">
                        Cập nhật
                    </button>
                </div>
            </div>
        </div>
    </div>

    <!-- Modal Xóa -->
    <div class="modal fade" id="deleteModal" tabindex="-1" aria-hidden="true">
        <div class="modal-dialog modal-lg">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title">Xóa Suất Chiếu</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">
                    <div class="alert alert-danger" role="alert">
                        Bạn có chắc chắn muốn xóa suất chiếu phim
                        <strong>{{ delete_suat_chieu.ten_phim }}</strong> vào ngày <strong>{{
                            delete_suat_chieu.ngay_chieu }}</strong>?
                    </div>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">
                        Đóng
                    </button>
                    <button type="button" class="btn btn-danger" data-bs-dismiss="modal" @click="deleteSuatChieu()">
                        Xác nhận
                    </button>
                </div>
            </div>
        </div>
    </div>

    <!-- Modal Tạo Vé -->
    <div class="modal fade" id="createModal" tabindex="-1" aria-hidden="true">
        <div class="modal-dialog modal-lg">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title">Tạo Vé Cho Suất Chiếu</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">
                    <div class="alert alert-danger" role="alert">
                        Bạn có chắc chắn muốn tạo vé cho phim
                        <strong>{{ create_ve_suat_chieu.ten_phim }}</strong> vào ngày <strong>{{
                            create_ve_suat_chieu.ngay_chieu }}</strong>?
                        <p class="mt-2 mb-0">
                            Số vé sẽ dựa trên toàn bộ ghế của phòng
                            <strong>{{ create_ve_suat_chieu.ten_phong }}</strong>.
                        </p>
                    </div>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">
                        Đóng
                    </button>
                    <button type="button" class="btn btn-danger" data-bs-dismiss="modal" @click="taoVeAuto">
                        Xác nhận
                    </button>
                </div>
            </div>
        </div>
    </div>

</template>
<script>
import axios from 'axios';
import apiUrl from '../../../utils/api'

export default {
    data() {
        return {
            list_suat_chieu: [],
            list_phim: [],
            list_phong_chieu: [],
            update_suat_chieu: {
                id: "",
                id_phim: "",
                id_phong_chieu: "",
                ngay_chieu: "",
                thoi_gian_bat_dau: "",
                thoi_gian_ket_thuc: "",
                tinh_trang: "",
                ten_phim: "",
                ten_phong: ""
            },
            delete_suat_chieu: {
                id: "",
                id_phim: "",
                id_phong_chieu: "",
                ngay_chieu: "",
                thoi_gian_bat_dau: "",
                thoi_gian_ket_thuc: "",
                tinh_trang: "",
                ten_phim: "",
                ten_phong: ""
            },
            create_suat_chieu: {
                id_phim: "",
                id_phong_chieu: "",
                ngay_chieu: "",
                thoi_gian_bat_dau: "",
                thoi_gian_ket_thuc: "",
                tinh_trang: "",
                ten_phim: "",
                ten_phong: ""
            },
            create_ve_suat_chieu: {
                id: "",
                id_phong_chieu: "",
                ten_phim: "",
                ten_phong: "",
                ngay_chieu: ""
            },
        }
    },
    methods: {
        setSuatChieuSelected(item) {
            this.create_ve_suat_chieu = {
                id: item.id,
                id_phong_chieu: item.id_phong_chieu,
                ten_phim: item.ten_phim,
                ten_phong: item.ten_phong,
                ngay_chieu: item.ngay_chieu,
            };
        },
        loadDataSuatChieu() {
            axios
                .get(apiUrl('admin/suat-chieu/get-data'), {
                    headers: {
                        Authorization: "Bearer " + localStorage.getItem('key_admin')
                    }
                })
                .then((res) => {
                    if(res.data.status) {
                        this.list_suat_chieu = res.data.data;
                        this.$toast.success(res.data.message);
                    } else {
                        this.$toast.error(res.data.message);
                    }
                })
                .catch((res) => {
                    const list = Object.values(res.response.data.errors);
                    list.forEach((v, i) => {
                        this.$toast.error(v[0]);
                    });
                });
        },
        loadDataPhim() {
            axios
                .get(apiUrl('admin/phim/get-data'), {
                    headers: {
                        Authorization: "Bearer " + localStorage.getItem('key_admin')
                    }
                })
                .then((res) => {
                    if(res.data.status) {
                        this.list_phim = res.data.data;
                        this.$toast.success(res.data.message);
                    } else {
                        this.$toast.error(res.data.message);
                    }
                })
                .catch((res) => {
                    const list = Object.values(res.response.data.errors);
                    list.forEach((v, i) => {
                        this.$toast.error(v[0]);
                    });
                });
        },
        loadDataPhongChieu() {
            axios
                .get(apiUrl('admin/phong-chieu/get-data'), {
                    headers: {
                        Authorization: "Bearer " + localStorage.getItem('key_admin')
                    }
                })
                .then((res) => {
                    if(res.data.status) {
                        this.list_phong_chieu = res.data.data;
                        this.$toast.success(res.data.message);
                    } else {
                        this.$toast.error(res.data.message);
                    }
                })
                .catch((res) => {
                    const list = Object.values(res.response.data.errors);
                    list.forEach((v, i) => {
                        this.$toast.error(v[0]);
                    });
                });
        },
        addSuatChieu() {
            axios
                .post(apiUrl('admin/suat-chieu/add-data'), this.create_suat_chieu, {
                    headers: {
                        Authorization: "Bearer " + localStorage.getItem('key_admin')
                    }
                })
                .then((res) => {
                    if (res.data.status) {
                        this.loadDataSuatChieu();
                        this.create_suat_chieu = {

                        };
                             this.$toast.success(res.data.message);
                    } else {
                        this.$toast.error(res.data.message);
                    }
                })
                .catch((res) => {
                    const list = Object.values(res.response.data.errors);
                    list.forEach((v, i) => {
                        this.$toast.error(v[0]);
                    });
                });
        },
        updateSuatChieu() {
            axios
                .post(apiUrl('admin/suat-chieu/update'), this.update_suat_chieu, {
                    headers: {
                        Authorization: "Bearer " + localStorage.getItem('key_admin')
                    }
                })
                .then((res) => {
                    if (res.data.status) {
                        this.loadDataSuatChieu();
                             this.$toast.success(res.data.message);
                    } else {
                        this.$toast.error(res.data.message);
                    }
                })
                .catch((res) => {
                    const list = Object.values(res.response.data.errors);
                    list.forEach((v, i) => {
                        this.$toast.error(v[0]);
                    });
                });
        },
        deleteSuatChieu() {
            axios
                .post(apiUrl('admin/suat-chieu/delete'), this.delete_suat_chieu, {
                    headers: {
                        Authorization: "Bearer " + localStorage.getItem('key_admin')
                    }
                })
                .then((res) => {
                    if (res.data.status) {
                        this.loadDataSuatChieu();
                             this.$toast.success(res.data.message);
                    } else {
                        this.$toast.error(res.data.message);
                    }
                })
                .catch((res) => {
                    const list = Object.values(res.response.data.errors);
                    list.forEach((v, i) => {
                        this.$toast.error(v[0]);
                    });
                });
        },
        taoVeAuto() {
            if (!this.create_ve_suat_chieu.id) {
                this.$toast.error('Vui lòng chọn suất chiếu!');
                return;
            }
            axios.post(apiUrl('admin/suat-chieu/tao-ve-auto'), this.create_ve_suat_chieu, {
                headers: {
                    Authorization: "Bearer " + localStorage.getItem('key_admin')
                }
            })
                .then((res) => {
                    if (res.data.status) {
                        this.$toast.success(res.data.message);
                    } else {
                        this.$toast.error(res.data.message);
                    }
                })
                .catch((res) => {
                    if (res.response?.data?.errors) {
                        const list = Object.values(res.response.data.errors);
                        list.forEach((v) => this.$toast.error(v[0]));
                    } else {
                        this.$toast.error('Không thể tạo vé tự động');
                    }
                });
        }
    },
    mounted() {
        this.loadDataSuatChieu();
        this.loadDataPhim();
        this.loadDataPhongChieu();
    },
}
</script>
<style></style>