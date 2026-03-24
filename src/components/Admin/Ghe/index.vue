<template>
    <div class="row">
        <div class="col-lg-4">
            <div class="card radius-10 border-top border-0 border-3 border-info">
                <div class="card-header">
                    <h4 class="mt-2"><b>THÊM GHẾ MỚI</b></h4>
                </div>
                <div class="card-body">
                    <div class="row">
                        <div class="col-md-12 mb-3">
                            <label class="form-label">Tên Ghế</label>
                            <input v-model="create_ghe.ten_ghe" type="text" class="form-control" placeholder="Nhập tên ghế" />
                        </div>
                    </div>
                    <div class="row">
                        <div class="col-md-12 mb-3">
                            <label class="form-label">Giá Ghế</label>
                            <input v-model="create_ghe.gia_ghe" type="number" class="form-control" placeholder="Nhập giá ghế" />
                        </div>
                    </div>
                    <div class="row">
                        <div class="col-md-12 mb-3">
                            <label class="form-label">Phòng Chiếu</label>
                            <select v-model="create_ghe.id_phong_chieu" class="form-select">
                                <option value="0">Chọn Phòng Chiếu</option>
                                <option v-for="(item, index) in list_phong_chieu" :value="item.id" :key="index">
                                    {{ item.ten_phong }}
                                </option>
                            </select>
                        </div>
                    </div>
                    <div class="row">
                        <div class="col-md-12 mb-3">
                            <label class="form-label">Tình Trạng</label>
                            <select v-model="create_ghe.tinh_trang" class="form-select">
                                <option value="1">Hoạt Động</option>
                                <option value="0">Tạm Ngưng</option>
                            </select>
                        </div>
                    </div>
                </div>
                <div class="card-footer text-end">
                    <button class="btn btn-primary" @click="addGhe()">Thêm ghế</button>
                </div>
            </div>
        </div>
        <div class="col-lg-8">
            <div class="card radius-10 border-top border-0 border-3 border-info">
                <div class="card-header d-flex justify-content-between">
                    <h4 class="mt-2"><b>DANH SÁCH GHẾ</b></h4>
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
                                    <th class="text-center">Tên Ghế</th>
                                    <th class="text-center">Giá Ghế</th>
                                    <th class="text-center">Phòng Chiếu</th>
                                    <th class="text-center" style="width: 15%;">Tình Trạng</th>
                                    <th class="text-center">Action</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr class="text-nowrap" v-for="(item, index) in list_ghe" :key="index">
                                    <th class="align-middle text-center">{{ index + 1 }}</th>
                                    <td class="align-middle text-center">{{ item.ten_ghe }}</td>
                                    <td class="align-middle text-center">{{ formatMoney(item.gia_ghe) }}</td>
                                    <td class="align-middle text-center">
                                        <template v-for="(item_1, index_1) in list_phong_chieu" :key="index_1">
                                            <span v-if="item_1.id == item.id_phong_chieu">{{ item_1.ten_phong }}</span>
                                        </template>
                                    </td>
                                    <td class="align-middle text-center">
                                        <button v-if="item.tinh_trang == 1" class="btn btn-success w-100">Hoạt
                                            Động</button>
                                        <button v-else class="btn btn-warning text-light w-100">Tạm Ngưng</button>
                                    </td>
                                    <td class="align-middle text-center" style="width: 200px;">
                                        <button class="btn btn-info text-light me-2" data-bs-toggle="modal"
                                            data-bs-target="#updateModal" v-on:click="Object.assign(update_ghe, item)">
                                            Cập nhật
                                        </button>
                                        <button class="btn btn-danger" data-bs-toggle="modal"
                                            data-bs-target="#deleteModal" v-on:click="Object.assign(delete_ghe, item)">
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

    <!-- Modal Cập Nhật -->
    <div class="modal fade" id="updateModal" tabindex="-1" aria-hidden="true">
        <div class="modal-dialog modal-lg">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title">Cập Nhật Thông Tin Ghế</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">
                    <div class="row">
                        <div class="col-md-12 mb-3">
                            <label class="form-label">Tên Ghế</label>
                            <input  v-model="update_ghe.ten_ghe" type="text" class="form-control"/>
                        </div>
                    </div>
                    <div class="row">
                        <div class="col-md-12 mb-3">
                            <label class="form-label">Giá Ghế</label>
                            <input  v-model="update_ghe.gia_ghe" type="number" class="form-control"/>
                        </div>
                    </div>
                    <div class="row">
                        <div class="col-md-12 mb-3">
                            <label class="form-label">Phòng Chiếu</label>
                            <select  v-model="update_ghe.id_phong_chieu" class="form-select">
                                <option value="0">Chọn Phòng Chiếu</option>
                                <option v-for="(item, index) in list_phong_chieu" :value="item.id" :key="index">
                                    {{ item.ten_phong }}
                                </option>
                            </select>
                        </div>
                    </div>
                    <div class="row">
                        <div class="col-md-12 mb-3">
                            <label class="form-label">Tình Trạng</label>
                            <select             v-model="update_ghe.tinh_trang" class="form-select">
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
                    <button type="button" class="btn btn-success" data-bs-dismiss="modal" @click="updateGhe()">
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
                    <h5 class="modal-title">Xóa Ghế</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">
                    <div class="alert alert-danger" role="alert">
                        Bạn có chắc chắn muốn xóa suất chiếu phim
                        <strong>{{ delete_ghe.ten_ghe }}</strong> tại phòng <strong>{{ delete_ghe.ten_phong }}</strong>?
                    </div>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">
                        Đóng
                    </button>
                    <button type="button" class="btn btn-danger" data-bs-dismiss="modal" @click="deleteGhe()">
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
            list_ghe: [],
            create_ghe: {
                ten_ghe: '',
                gia_ghe: '',
                tinh_trang: '',
                id_phong_chieu: '',
            },
            update_ghe: {
                id: '',
                ten_ghe: '',
                gia_ghe: '',
                tinh_trang: '',
                id_phong_chieu: '',
                ten_phong: ''
            },
            delete_ghe: {
                id: '',
                ten_ghe: '',
                gia_ghe: '',
                tinh_trang: '',
                id_phong_chieu: '',
                ten_phong: ''
            },
        }
    },
    methods: {
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
        loadDataGhe() {
            axios
                .get(apiUrl('admin/ghe/get-data'), {
                    headers: {
                        Authorization: "Bearer " + localStorage.getItem('key_admin')
                    }
                })
                .then((res) => {
                    if(res.data.status) {
                        this.list_ghe = res.data.data;
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
        addGhe() {
            axios
                .post(apiUrl('admin/ghe/add-data'), this.create_ghe, {
                    headers: {
                        Authorization: "Bearer " + localStorage.getItem('key_admin')
                    }
                })
                .then((res) => {
                    if (res.data.status) {
                        this.loadDataGhe();
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
        updateGhe() {
            axios
                .post(apiUrl('admin/ghe/update'), this.update_ghe, {
                    headers: {
                        Authorization: "Bearer " + localStorage.getItem('key_admin')
                    }
                })
                .then((res) => {
                    if (res.data.status) {
                        this.loadDataGhe();
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
        deleteGhe() {
            axios
                .post(apiUrl('admin/ghe/delete'), this.delete_ghe, {
                    headers: {
                        Authorization: "Bearer " + localStorage.getItem('key_admin')
                    }
                })
                .then((res) => {
                    if (res.data.status) {
                        this.loadDataGhe();
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
        formatMoney(number) {
            return new Intl.NumberFormat("vi-VI", { style: "currency", currency: "VND" }).format(number).replace('₫','VND');
        }
    },
    mounted() {
        this.loadDataPhongChieu();
        this.loadDataGhe();
    },
}
</script>
<style></style>