<template>
    <div class="card border-top border-0 border-4 border-info">
        <div class="card-body">
            <div class="row align-items-center">
                <div class="col-lg-4 col-md-6">
                    <label for="">Từ ngày</label>
                    <input type="date" class="form-control mt-2 mb-2 w-100">
                </div>
                <div class="col-lg-4 col-md-6">
                    <label for="">Đến ngày</label>
                    <input type="date" class="form-control mt-2 mb-2">
                </div>
                <div class="col-lg-2"></div>
                <div class="col-lg-1 col-md-12">
                    <label for="">&nbsp;</label>
                    <button class="btn btn-primary w-100">Lọc</button>
                </div>
                <div class="col-lg-1 col-md-12">
                    <label for="">&nbsp;</label>
                    <button class="btn btn-secondary w-100">Đặt Lại</button>
                </div>
            </div>
        </div>
    </div>
    <div class="card border-top border-0 border-4 border-success">
        <div class="card-header">
            <h5 class="mt-2">DANH SÁCH ĐƠN ĐẶT HÀNG</h5>
        </div>
        <div class="card-body">
            <div class="table-responsive">
                <table class="table table-bordered table-hover">
                    <thead class="align-middle">
                        <tr class="text-center table-success">
                            <th class="align-middle">#</th>
                            <th style="width: 150px;">Mã Hóa Đơn</th>
                            <th>Tên Khách Hàng</th>
                            <th>Tổng Tiền</th>
                            <th>Giảm Giá</th>
                            <th>Tiền Thực Nhận</th>
                            <th>Ngày Đặt</th>
                            <th>Trạng thái</th>
                            <th>Chi Tiết</th>
                            <th>Action</th>
                        </tr>
                    </thead>
                    <tbody>
                        <template v-for="(value, index) in list_don_hang" :key="index">
                            <tr class="align-middle">
                                <td class="align-middle text-center">{{ index + 1 }}</td>
                                <td class="align-middle">{{ value.ma_don_hang }}</td>
                                <td class="align-middle">{{ value.ten_khach_hang }}</td>
                                <td class="align-middle text-end">{{ formatVND(value.tong_tien) }}</td>
                                <td class="align-middle text-end">{{ formatVND(value.giam_gia) }}</td>
                                <td class="align-middle text-end">{{ formatVND(value.tien_thuc_nhan) }}</td>
                                <td class="align-middle text-center">{{ formatDate(value.created_at) }}</td>
                                <td class="align-middle text-center" style="width: 100px;">
                                    <button v-if="value.is_thanh_toan == 0"
                                        class="btn btn-warning btn-sm w-100 text-light">Chờ Thanh
                                        Toán</button>
                                    <button v-else class="btn btn-success btn-sm w-100">Đã Thanh
                                        Toán</button>
                                </td>
                                <td class="align-middle text-center">
                                    <button v-on:click="Object.assign(getChiTietDichVu(value.id))"
                                        class="btn btn-info w-100 btn-sm me-2 px-0" data-bs-toggle='modal'
                                        data-bs-target='#chiTietModal' style="color: white;">
                                        <i class="fas fa-eye"></i>
                                    </button>
                                </td>
                                <td class="align-middle text-center">
                                    <button class="btn btn-success btn-sm pe-1 me-2" data-bs-toggle='modal'
                                        data-bs-target='#capNhatTrangThaiModal'
                                        v-on:click="Object.assign(update_don_hang, value)">
                                        <i class="fa-solid fa-pen-nib"></i>
                                    </button>
                                    <a target="_blank" :href="`/admin/in-ve/` + value.ma_don_hang"
                                        class="btn btn-secondary btn-sm pe-1 me-2">
                                        <i class="fa-solid fa-print"></i>
                                    </a>
                                    <button v-on:click="Object.assign(don_huy, value)"
                                        class="btn btn-danger btn-sm pe-1" data-bs-toggle='modal'
                                        data-bs-target='#huyModal'>
                                        <i class="fas fa-times "></i>
                                    </button>
                                </td>
                            </tr>
                        </template>
                    </tbody>
                </table>
            </div>
        </div>
    </div>
    <!-- Modal xem chi tiết -->
    <div class="modal fade" id="chiTietModal" tabindex="-1" aria-labelledby="chiTietModalLabel" aria-hidden="true">
        <div class="modal-dialog modal-xl">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title" id="chiTietModalLabel">Chi tiết đơn hàng
                    </h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">
                    <div class="row">
                        <div class="col-lg-5">
                            <div class="table-responsive">
                                <table class="table table-bordered table-stripped">
                                    <thead>
                                        <tr class="text-center">
                                            <th>#</th>
                                            <th>Dịch Vụ</th>
                                            <th>Đơn Giá</th>
                                            <th>Ghi Chú</th>
                                        </tr>
                                    </thead>
                                    <tbody>
                                        <template v-for="(value, index) in list_dich_vu" :key="index">
                                            <tr>
                                                <td class="text-center align-middle">{{ index + 1 }}</td>
                                                <td class="align-middle">{{ value.ten_dich_vu }}</td>
                                                <td class="text-center align-middle">{{ formatVND(value.gia) }}</td>
                                                <td class="align-middle">{{ value.ghi_chu }}</td>
                                            </tr>
                                        </template>
                                    </tbody>
                                </table>
                            </div>
                        </div>
                        <div class="col-lg-7">
                            <div class="table-responsive">
                                <table class="table table-bordered table-stripped">
                                    <thead>
                                        <tr class="text-center">
                                            <th>#</th>
                                            <th>Tên Ghế</th>
                                            <th>Tên Phim</th>
                                            <th>Suất Chiếu</th>
                                            <th>Giá Vé</th>
                                        </tr>
                                    </thead>
                                    <tbody>
                                        <template v-for="(value, index) in list_ve" :key="index">
                                            <tr>
                                                <td class="text-center align-middle">{{ index + 1 }}</td>
                                                <td class="align-middle">Ghế {{ value.ten_ghe }}</td>
                                                <td class="text-center align-middle">{{ value.ten_phim }}</td>
                                                <td class="text-center align-middle">{{ value.ngay_chieu }} - {{
                                                    value.thoi_gian_bat_dau }}</td>
                                                <td class="text-center align-middle">{{ formatVND(value.gia_ve) }}</td>
                                            </tr>
                                        </template>
                                    </tbody>
                                </table>
                            </div>
                        </div>
                    </div>

                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Đóng</button>
                </div>
            </div>
        </div>
    </div>
    <!-- modal hủy đơn hàng -->
    <div class="modal fade" id="huyModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
        <div class="modal-dialog">
            <div class="modal-content">
                <div class="modal-header">
                    <h1 class="modal-title fs-5" id="exampleModalLabel">Hủy Đơn Hàng</h1>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">
                    <div class="alert alert-warning border-0 bg-warning alert-dismissible fade show py-2">
                        <div class="d-flex align-items-center">
                            <div class="font-35 text-dark"><i class="bx bx-info-circle"></i>
                            </div>
                            <div class="ms-3">
                                <h6 class="mb-0 text-dark">Cảnh Báo</h6>
                                <div class="text-dark">
                                    <span>Bạn muốn xóa hóa đơn đơn hàng <b></b> này</span> <br>
                                    <span>
                                        <b>Lưu ý:</b> Điều này không thể hoàn tác!
                                    </span>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Đóng</button>
                    <button type="button" class="btn btn-danger" v-on:click="xoaDonHang()" data-bs-dismiss="modal">Xác
                        Nhận</button>
                </div>
            </div>
        </div>
    </div>
    <!-- modal cập nhật trạng thái -->
    <div class="modal fade" id="capNhatTrangThaiModal" tabindex="-1" aria-hidden="true">
        <div class="modal-dialog">
            <div class="modal-content">
                <div class="modal-header bg-success">
                    <h1 class="modal-title fs-5 text-white">Cập Nhật Trạng Thái Đơn Hàng</h1>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">
                    <label class="form-label fw-bold">Trạng Thái Đơn {{ update_don_hang.ma_don_hang }}</label>
                    <select v-model="update_don_hang.is_thanh_toan" class="form-select border-success">
                        <option value="0">Chưa Thanh Toán</option>
                        <option value="1">Đã Thanh Toán</option>

                    </select>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Đóng</button>
                    <button type="button" class="btn btn-success" data-bs-dismiss="modal" @click="thucHienUpdate()">
                        Xác Nhận
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
            list_don_hang: [],
            list_dich_vu: [],
            list_ve: [],
            update_don_hang: {
                id: '',
                is_thanh_toan: ''
            },
            don_huy: {},
        }
    },
    mounted() {
        this.getDonHang();
    },
    methods: {
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
        formatVND(number) {
            return new Intl.NumberFormat("vi-VI", { style: "currency", currency: "VND" }).format(number,);
        },
        getDonHang() {
            axios.get(apiUrl('admin/don-hang/get-data'), {
                headers: {
                    Authorization: "Bearer " + localStorage.getItem('key_admin')
                }
            })
                .then((res) => {
                    if (res.data.status) {
                        this.list_don_hang = res.data.data;
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
        getChiTietDichVu(id) {
            axios.get(apiUrl('admin/don-hang/chi-tiet-dich-vu/' + id), {
                headers: {
                    Authorization: "Bearer " + localStorage.getItem('key_admin')
                }
            })
                .then((res) => {
                    if (res.data.status) {
                        this.list_dich_vu = res.data.data_dich_vu;
                        this.list_ve = res.data.data_ve;
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
        thucHienUpdate() {
            axios.post(apiUrl('admin/don-hang/update'), this.update_don_hang, {
                headers: {
                    Authorization: "Bearer " + localStorage.getItem('key_admin')
                }
            })
                .then((res) => {
                    if (res.data.status) {
                        this.$toast.success(res.data.message);
                        this.getDonHang();
                    } else {
                        this.$toast.error(res.data.message);
                    }
                });
        },
        xoaDonHang() {
            axios.post(apiUrl('admin/don-hang/delete'), this.don_huy, {
                headers: {
                    Authorization: "Bearer " + localStorage.getItem('key_admin')
                }
            })
                .then((res) => {
                    if (res.data.status) {
                        this.$toast.success(res.data.message);
                        this.getDonHang(); // Load lại danh sách sau khi xóa thành công
                    } else {
                        this.$toast.error(res.data.message);
                    }
                })
                .catch((error) => {
                    this.$toast.error("Có lỗi xảy ra khi xóa đơn hàng!");
                });
        }
    },
}
</script>
<style></style>