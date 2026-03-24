<template>
    <div class="row">
        <div class="col-lg-12">
            <div class="card radius-10 border-top border-0 border-3 border-info">
                <div class="card-header d-flex justify-content-between">
                    <h4 class="mt-2"><b>DANH SÁCH PHÒNG CHIẾU</b></h4>
                    <button class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#addModal">
                        Thêm Phòng Chiếu
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
                                    <th class="text-center">Tên Phòng Chiếu</th>
                                    <th class="text-center">Hàng Dọc</th>
                                    <th class="text-center">Hàng Ngang</th>
                                    <th class="text-center" style="width: 15%;">Tình Trạng</th>
                                    <th class="text-center">Action</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr class="text-nowrap" v-for="(item, index) in list_phong_chieu" :key="index">
                                    <th class="align-middle text-center">{{ index + 1 }}</th>
                                    <td class="align-middle text-center">{{ item.ten_phong }}</td>
                                    <td class="align-middle text-center">{{ item.hang_doc }}</td>
                                    <td class="align-middle text-center">{{ item.hang_ngang }}</td>
                                    <td class="align-middle text-center">
                                        <button v-if="item.tinh_trang == 1" class="btn btn-success w-100">
                                            <i class="fa-solid fa-circle-check"></i> Hoạt Động
                                        </button>
                                        <button v-else class="btn btn-warning text-light w-100">
                                            <i class="fa-solid fa-circle-xmark"></i> Tạm Ngưng
                                        </button>
                                    </td>
                                    <td class="align-middle text-center" style="width: 200px;">
                                        <button class="btn btn-primary me-2" data-bs-toggle="modal"
                                            data-bs-target="#createModal"
                                            v-on:click="setPhongChieuSelected(item)">
                                            Tạo Ghế Auto
                                        </button>
                                        <button class="btn btn-info text-light me-2" data-bs-toggle="modal"
                                            data-bs-target="#updateModal"
                                            v-on:click="Object.assign(update_phong_chieu, item)">
                                            Cập nhật
                                        </button>
                                        <button class="btn btn-danger" data-bs-toggle="modal"
                                            data-bs-target="#deleteModal"
                                            v-on:click="Object.assign(delete_phong_chieu, item)">
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
        <div class="modal-dialog modal-lg">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title">Thêm Phòng Chiếu Mới</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">
                    <div class="row">
                        <div class="col-md-12 mb-3">
                            <label class="form-label">Tên Phòng Chiếu</label>
                            <input v-model="create_phong_chieu.ten_phong" type="text" class="form-control" placeholder="Nhập tên phòng chiếu" />
                        </div>
                    </div>
                    <div class="row">
                        <div class="col-md-12 mb-3">
                            <label class="form-label">Hàng Dọc</label>
                            <input v-model="create_phong_chieu.hang_doc" type="number" class="form-control" placeholder="Nhập số hàng dọc" />
                        </div>
                    </div>
                    <div class="row">
                        <div class="col-md-12 mb-3">
                            <label class="form-label">Hàng Ngang</label>
                            <input v-model="create_phong_chieu.hang_ngang" type="number" class="form-control" placeholder="Nhập số hàng ngang" />
                        </div>
                    </div>
                    <div class="row">
                        <div class="col-md-12 mb-3">
                            <label class="form-label">Trạng Thái</label>
                            <select v-model="create_phong_chieu.tinh_trang" class="form-select">
                                <option value="1">Hoạt Động</option>
                                <option value="0">Tạm Ngưng</option>
                            </select>
                        </div>
                    </div>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">
                        Đóng
                    </button>
                    <button type="button" class="btn btn-primary" data-bs-dismiss="modal" @click="addPhongChieu()">
                        Thêm mới
                    </button>
                </div>
            </div>
        </div>
    </div>

    <!-- Modal Tạo Ghế Auto -->
    <div class="modal fade" id="createModal" tabindex="-1" aria-hidden="true">
        <div class="modal-dialog">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title">Tạo Ghế Tự Động</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">
                    <div class="mb-3">
                        <label class="form-label">Phòng chiếu</label>
                        <input class="form-control" type="text" :value="create_ghe_phong_chieu.ten_phong" disabled />
                    </div>
                    <div class="mb-3">
                        <label class="form-label">Hàng dọc</label>
                        <input v-model.number="create_ghe_phong_chieu.hang_doc" type="number" min="1" class="form-control"
                            placeholder="Nhập số hàng dọc" />
                    </div>
                    <div class="mb-3">
                        <label class="form-label">Hàng ngang</label>
                        <input v-model.number="create_ghe_phong_chieu.hang_ngang" type="number" min="1"
                            class="form-control" placeholder="Nhập số hàng ngang" />
                    </div>
                    <div class="mb-3">
                        <label class="form-label">Giá ghế mặc định (VND)</label>
                        <input v-model.number="create_ghe_phong_chieu.gia_ghe" type="number" min="0"
                            class="form-control" placeholder="Ví dụ: 50000" />
                    </div>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Đóng</button>
                    <button type="button" class="btn btn-primary" data-bs-dismiss="modal" @click="taoGheAuto">
                        Xác nhận
                    </button>
                </div>
            </div>
        </div>
    </div>

    <!-- Modal Cập Nhật -->
    <div class="modal fade" id="updateModal" tabindex="-1" aria-hidden="true">
        <div class="modal-dialog modal-lg">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title">Cập Nhật Thông Tin Phòng Chiếu</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">
                    <div class="row">
                        <div class="col-md-12 mb-3">
                            <label class="form-label">Tên Phòng Chiếu</label>
                            <input v-model="update_phong_chieu.ten_phong" type="text" class="form-control"
                                placeholder="Nhập tên phòng chiếu" />
                        </div>
                    </div>
                    <div class="row">
                        <div class="col-md-12 mb-3">
                            <label class="form-label">Hàng Dọc</label>
                            <input v-model="update_phong_chieu.hang_doc" type="number" class="form-control"
                                placeholder="Nhập số hàng dọc" />
                        </div>
                    </div>
                    <div class="row">
                        <div class="col-md-12 mb-3">
                            <label class="form-label">Hàng Ngang</label>
                            <input v-model="update_phong_chieu.hang_ngang" type="number" class="form-control"
                                placeholder="Nhập số hàng ngang" />
                        </div>
                    </div>
                    <div class="row">
                        <div class="col-md-12 mb-3">
                            <label class="form-label">Trạng Thái</label>
                            <select v-model="update_phong_chieu.tinh_trang" class="form-select">
                                <option value="1">Hoạt Động</option>
                                <option value="0">Tạm Ngưng</option>
                            </select>
                        </div>
                    </div>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">
                        Đóng
                    </button>
                    <button type="button" class="btn btn-success" data-bs-dismiss="modal" @click="updatePhongChieu()">
                        Cập Nhật
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
                    <h5 class="modal-title">Xóa Phòng Chiếu</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">
                    <div class="alert alert-danger" role="alert">
                        Bạn có chắc chắn muốn xóa phòng chiếu
                        <strong class="text-danger">{{ delete_phong_chieu.ten_phong }}</strong> này không?
                        <p class="mt-2"><strong><i class="fa-solid fa-circle-xmark"></i> Nếu xóa phòng chiếu này, tất cả
                                các suất chiếu liên quan cũng sẽ bị xóa.</strong></p>
                    </div>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">
                        Đóng
                    </button>
                    <button type="button" class="btn btn-danger" data-bs-dismiss="modal" @click="deletePhongChieu()">
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
            list_phong_chieu: [],
            create_ghe_phong_chieu: {
                id: '',
                ten_phong: '',
                hang_doc: '',
                hang_ngang: '',
                gia_ghe: '',
            },
            create_phong_chieu: {
                ten_phong: '',
                hang_ngang: '',
                hang_doc: '',
                is_active: '',
                tinh_trang: '',
            },
            update_phong_chieu: {
                id: '',
                ten_phong: '',
                hang_ngang: '',
                hang_doc: '',
                is_active: '',
                tinh_trang: '',
            },
            delete_phong_chieu: {
                id: '',
                ten_phong: '',
                hang_ngang: '',
                hang_doc: '',
                is_active: '',
                tinh_trang: '',
            },
        }
    },
    methods: {
        setPhongChieuSelected(item) {
            this.create_ghe_phong_chieu = {
                id: item.id,
                ten_phong: item.ten_phong,
                hang_doc: item.hang_doc,
                hang_ngang: item.hang_ngang,
                gia_ghe: '',
            };
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
        addPhongChieu() {
            axios
                .post(apiUrl('admin/phong-chieu/add-data'), this.create_phong_chieu, {
                    headers: {
                        Authorization: "Bearer " + localStorage.getItem('key_admin')
                    }
                })
                .then((res) => {
                    if (res.data.status) {
                        this.loadDataPhongChieu();
                        this.create_phong_chieu = {
                           
                        };
                        this.$toast.success(res.data.message);
                    }
                })
                .catch((res) => {
                    const list = Object.values(res.response.data.errors);
                    list.forEach((v, i) => {
                        this.$toast.error(v[0]);
                    });
                });
        },
        updatePhongChieu() {
            axios
                .post(apiUrl('admin/phong-chieu/update'), this.update_phong_chieu, {
                    headers: {
                        Authorization: "Bearer " + localStorage.getItem('key_admin')
                    }
                })
                .then((res) => {
                    if (res.data.status) {
                        this.loadDataPhongChieu();
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
        deletePhongChieu() {
            axios
                .post(apiUrl('admin/phong-chieu/delete'), this.delete_phong_chieu, {
                    headers: {
                        Authorization: "Bearer " + localStorage.getItem('key_admin')
                    }
                })
                .then((res) => {
                    if (res.data.status) {
                        this.loadDataPhongChieu();
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
        taoGheAuto() {
            if (!this.create_ghe_phong_chieu.id) {
                this.$toast.error('Vui lòng chọn phòng chiếu!');
                return;
            }
            axios.post(apiUrl('admin/phong-chieu/tao-ghe-auto'), this.create_ghe_phong_chieu, {
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
                        this.$toast.error('Không thể tạo ghế tự động');
                    }
                });
        }
    },
    mounted() {
        this.loadDataPhongChieu();
    },
}
</script>
<style></style>