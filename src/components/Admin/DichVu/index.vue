<template>
    <div class="row">
        <div class="col-lg-3">
            <div class="card radius-10 border-top border-0 border-3 border-info">
                <div class="card-header">
                    <h4 class="mt-2"><b>THÊM DỊCH VỤ</b></h4>
                </div>
                <div class="card-body">
                    <div class="row">
                        <div class="col-md-12 mb-3">
                            <label class="form-label">Tên Dịch Vụ</label>
                            <input v-model="create_dich_vu.ten_dich_vu" type="text" class="form-control" placeholder="Nhập tên dịch vụ" />
                        </div>
                    </div>
                    <div class="row">
                        <div class="col-md-12 mb-3">
                            <label class="form-label">Hình Ảnh</label>
                            <input v-model="create_dich_vu.hinh_anh" type="text" class="form-control" placeholder="Nhập link hình ảnh" />
                        </div>
                    </div>
                    <div class="row">
                        <div class="col-md-12 mb-3">
                            <label class="form-label">Giá Dịch Vụ</label>
                            <input v-model="create_dich_vu.gia" type="number" class="form-control" placeholder="Nhập giá dịch vụ" />
                        </div>
                    </div>
                    <div class="row">
                        <div class="col-md-12 mb-3">
                            <label class="form-label">Mô Tả Dịch Vụ</label>
                            <textarea v-model="create_dich_vu.mo_ta" class="form-control" rows="3" placeholder="Nhập mô tả dịch vụ"></textarea>
                        </div>
                    </div>
                    <div class="row">
                        <div class="col-md-12 mb-3">
                            <label class="form-label">Tình Trạng</label>
                            <select v-model="create_dich_vu.tinh_trang" class="form-select">
                                <option value="1">Hiển Thị</option>
                                <option value="0">Tạm Tắt</option>
                            </select>
                        </div>
                    </div>
                </div>
                <div class="card-footer text-end">
                    <button class="btn btn-primary" @click="addDichVu()">Thêm dịch vụ</button>
                </div>
            </div>
        </div>
        <div class="col-lg-9">
            <div class="card radius-10 border-top border-0 border-3 border-info">
                <div class="card-header d-flex justify-content-between">
                    <h4 class="mt-2"><b>DANH SÁCH DỊCH VỤ</b></h4>
                </div>
                <div class="card-body table-responsive">
                    <div class="input-group mb-3">
                        <input type="text" class="form-control" placeholder="Tìm kiếm dịch vụ...">
                        <button class="btn btn-primary input-group-text" style="width: 155px;">Tìm
                            kiếm</button>
                    </div>
                    <div class="table-responsive">
                        <table class="table table-bordered table-hover">
                            <thead>
                                <tr class="bg-primary text-light text-nowrap">
                                    <th class="text-center">#</th>
                                    <th class="text-center">Tên Dịch Vụ</th>
                                    <th class="text-center">Hình Ảnh</th>
                                    <th class="text-center">Giá Dịch Vụ</th>
                                    <th class="text-center">Mô Tả Dịch Vụ</th>
                                    <th class="text-center" style="width: 15%;">Tình Trạng</th>
                                    <th class="text-center">Action</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr class="text-nowrap" v-for="(item, index) in list_dich_vu" :key="index">
                                    <th class="align-middle text-center">{{ index + 1 }}</th>
                                    <td class="align-middle">{{ item.ten_dich_vu }}</td>
                                    <td class="align-middle text-center">
                                        <img :src="item.hinh_anh" alt="Hình ảnh" style="width: 100px; height: 100px; object-fit: cover;">
                                    </td>
                                    <td class="align-middle text-center">{{ formatMoney(item.gia) }}</td>
                                    <td class="align-middle">{{ item.mo_ta }}</td>
                                    <td class="align-middle text-center">
                                        <button v-if="item.tinh_trang == 1" class="btn btn-success w-100">
                                            <i class="fa-solid fa-circle-check"></i> Hiển Thị
                                        </button>
                                        <button v-else class="btn btn-warning text-light w-100">
                                            <i class="fa-solid fa-circle-xmark"></i> Tạm Tắt
                                        </button>
                                    </td>
                                    <td class="align-middle text-center" style="width: 200px;">
                                        <button class="btn btn-info text-light me-2" data-bs-toggle="modal"
                                            data-bs-target="#updateModal"
                                            v-on:click="Object.assign(update_dich_vu, item)">
                                            Cập nhật
                                        </button>
                                        <button class="btn btn-danger" data-bs-toggle="modal"
                                            data-bs-target="#deleteModal"
                                            v-on:click="Object.assign(delete_dich_vu, item)">
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
                    <h5 class="modal-title">Cập Nhật Thông Tin Dịch Vụ</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">
                    <div class="row">
                        <div class="col-md-12 mb-3">
                            <label class="form-label">Tên Dịch Vụ</label>
                            <input  v-model="update_dich_vu.ten_dich_vu" type="text" class="form-control" placeholder="Nhập tên dịch vụ" />
                        </div>
                    </div>
                    <div class="row">
                        <div class="col-md-12 mb-3">
                            <label class="form-label">Hình Ảnh</label>
                            <input v-model="update_dich_vu.hinh_anh" type="text" class="form-control" placeholder="Nhập link hình ảnh" />
                        </div>
                    </div>
                    <div class="row">
                        <div class="col-md-12 mb-3">
                            <label class="form-label">Giá Dịch Vụ</label>
                            <input v-model="update_dich_vu.gia" type="number" class="form-control" placeholder="Nhập giá dịch vụ" />
                        </div>
                    </div>
                    <div class="row">
                        <div class="col-md-12 mb-3">
                            <label class="form-label">Mô Tả Dịch Vụ</label>
                            <textarea v-model="update_dich_vu.mo_ta" class="form-control" rows="3" placeholder="Nhập mô tả dịch vụ"></textarea>
                        </div>
                    </div>
                    <div class="row">
                        <div class="col-md-12 mb-3">
                            <label class="form-label">Tình Trạng</label>
                            <select v-model="update_dich_vu.tinh_trang" class="form-select">
                                <option value="1">Hiển Thị</option>
                                <option value="0">Tạm Tắt</option>
                            </select>
                        </div>
                    </div>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">
                        Đóng
                    </button>
                    <button @click="updateDichVu()" type="button" class="btn btn-success" data-bs-dismiss="modal">
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
                    <h5 class="modal-title">Xóa Dịch Vụ</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">
                    <div class="alert alert-danger" role="alert">
                        Bạn có chắc chắn muốn xóa dịch vụ
                        <strong class="text-danger">{{ delete_dich_vu.ten_dich_vu }}</strong> này không?
                        <p class="mt-2"><strong><i class="fa-solid fa-circle-xmark"></i> Điều này không thể hoàn tác !!.</strong></p>
                    </div>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">
                        Đóng
                    </button>
                    <button @click="deleteDichVu()" type="button" class="btn btn-danger" data-bs-dismiss="modal">
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
import { createToaster } from "@meforma/vue-toaster";
const toaster = createToaster({ position: "top-right" });
export default {
    data() {
        return {
            list_dich_vu: [],
            create_dich_vu: {
                ten_dich_vu: '',
                hinh_anh: '',
                gia: '',
                mo_ta: '',
                tinh_trang: '',
            },
            update_dich_vu: {
                id: '',
                ten_dich_vu: '',
                gia: '',
                mo_ta: '',
                hinh_anh: ''
            },
            delete_dich_vu: {
                id: '',
                ten_dich_vu: '',
                
            },
        }
    },
    methods: {
        loadDataDichVu() {
            axios
                .get(apiUrl('admin/dich-vu/get-data'), {
                    headers: {
                        Authorization: "Bearer " + localStorage.getItem('key_admin')
                    }
                })
                .then((res) => {
                    if(res.data.status) {
                        this.list_dich_vu = res.data.data;
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
        addDichVu() {
            axios
                .post(apiUrl('admin/dich-vu/add-data'), this.create_dich_vu, {
                    headers: {
                        Authorization: "Bearer " + localStorage.getItem('key_admin')
                    }
                })
                .then((res) => {
                    if (res.data.status) {
                        this.loadDataDichVu();
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
        updateDichVu() {
            axios
                .post(apiUrl('admin/dich-vu/update'), this.update_dich_vu, {
                    headers: {
                        Authorization: "Bearer " + localStorage.getItem('key_admin')
                    }
                })
                .then((res) => {
                    if (res.data.status) {
                        this.loadDataDichVu();
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
        deleteDichVu() {
            axios
                .post(apiUrl('admin/dich-vu/delete'), this.delete_dich_vu, {
                    headers: {
                        Authorization: "Bearer " + localStorage.getItem('key_admin')
                    }
                })
                .then((res) => {
                    if (res.data.status) {
                        this.loadDataDichVu();
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
            return new Intl.NumberFormat("vi-VI", { style: "currency", currency: "VND" }).format(number).replace('₫', 'VND');
        }
    },
    mounted() {
        this.loadDataDichVu();
    },
}
</script>
<style></style>