<template>
    <div class="row">
        <div class="col-lg-12">
            <div class="card radius-10 border-top border-0 border-3 border-info">
                <div class="card-header d-flex justify-content-between">
                    <h4 class="mt-2"><b>DANH SÁCH PHIM</b></h4>
                    <button class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#addModal">
                        Thêm Phim
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
                                    <th class="text-center">Hình Ảnh</th>
                                    <th class="text-center">Đạo Diễn</th>
                                    <th class="text-center">Ngày Phát Hành</th>
                                    <th class="text-center">Quốc Gia</th>
                                    <th class="text-center">Trạng Thái</th>
                                    <th class="text-center">Action</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr class="text-nowrap" v-for="(item, index) in list_phim" :key="index">
                                    <th class="align-middle text-center">{{ index + 1 }}</th>
                                    <td class="align-middle">{{ item.ten_phim }}</td>
                                    <td class="align-middle text-center">
                                        <img v-bind:src="item.hinh_anh" alt="" class="img-fluid"
                                            style="height: 80px; width: 70px;">
                                    </td>
                                    <td class="align-middle">{{ item.dao_dien }}</td>
                                    <td class="align-middle text-center">{{ item.ngay_phat_hanh }}</td>
                                    <td class="align-middle text-center">{{ item.quoc_gia }}</td>
                                    <td class="align-middle text-center">
                                        <button v-if="item.tinh_trang == 2" class="btn btn-success w-100">Đang
                                            Chiếu</button>
                                        <button v-else-if="item.tinh_trang == 1" class="btn btn-danger w-100">Sắp
                                            Chiếu</button>
                                        <button v-else class="btn btn-warning text-light w-100">Ngừng Chiếu</button>
                                    </td>
                                    <td class="align-middle text-center" style="width: 200px;">
                                        <button class="btn btn-secondary text-light me-2" data-bs-toggle="modal"
                                            data-bs-target="#detailModal" v-on:click="Object.assign(detail_phim, item)">
                                            Chi Tiết
                                        </button>
                                        <button class="btn btn-info text-light me-2" data-bs-toggle="modal"
                                            data-bs-target="#updateModal" v-on:click="Object.assign(update_phim, item)">
                                            Cập nhật
                                        </button>
                                        <button class="btn btn-danger" data-bs-toggle="modal"
                                            data-bs-target="#deleteModal" v-on:click="Object.assign(delete_phim, item)">
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
                    <h5 class="modal-title">Thêm Thông Tin Phim</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">
                    <div class="row g-3">
                        <!-- Tên phim -->
                        <div class="col-md-6">
                            <label class="form-label fw-semibold">Tên Phim</label>
                            <input v-model="create_phim.ten_phim" type="text" class="form-control" placeholder="Nhập tên phim">
                        </div>

                        <!-- Hình ảnh -->
                        <div class="col-md-6">
                            <label class="form-label fw-semibold">Hình Ảnh</label>
                            <input v-model="create_phim.hinh_anh" type="text" class="form-control" placeholder="Nhập URL hình ảnh">
                        </div>

                        <!-- Đạo diễn -->
                        <div class="col-md-3">
                            <label class="form-label fw-semibold">Đạo Diễn</label>
                            <input v-model="create_phim.dao_dien" type="text" class="form-control" placeholder="Nhập tên đạo diễn">
                        </div>

                        <!-- Diễn viên -->
                        <div class="col-md-6">
                            <label class="form-label fw-semibold">Diễn Viên</label>
                            <input v-model="create_phim.dien_vien" type="text" class="form-control" placeholder="Nhập tên diễn viên">
                        </div>

                        <!-- Nhà sản xuất -->
                        <div class="col-md-3">
                            <label class="form-label fw-semibold">Nhà Sản Xuất</label>
                            <input v-model="create_phim.nha_san_xuat" type="text" class="form-control" placeholder="Nhập nhà sản xuất">
                        </div>

                        <!-- Quốc gia -->
                        <div class="col-md-4">
                            <label class="form-label fw-semibold">Quốc Gia</label>
                            <input v-model="create_phim.quoc_gia" type="text" class="form-control" placeholder="Nhập quốc gia">
                        </div>

                        <!-- Ngôn ngữ -->
                        <div class="col-md-4">
                            <label class="form-label fw-semibold">Ngôn Ngữ</label>
                            <input v-model="create_phim.ngon_ngu" type="text" class="form-control" placeholder="Nhập ngôn ngữ">
                        </div>

                        <!-- Ngày phát hành -->
                        <div class="col-md-4">
                            <label class="form-label fw-semibold">Ngày Phát Hành</label>
                            <input v-model="create_phim.ngay_phat_hanh" type="date" class="form-control">
                        </div>

                        <!-- Thể loại -->
                        <div class="col-md-6">
                            <label class="form-label fw-semibold">Thể Loại</label>
                            <input v-model="create_phim.the_loai" type="text" class="form-control" placeholder="Nhập thể loại">
                        </div>

                        <!-- Thời lượng -->
                        <div class="col-md-3">
                            <label class="form-label fw-semibold">Thời Lượng</label>
                            <input v-model="create_phim.thoi_luong" type="number" class="form-control" placeholder="Phút">
                        </div>

                        <!-- Trạng thái -->
                        <div class="col-md-3">
                            <label class="form-label fw-semibold">Trạng Thái</label>
                            <select v-model="create_phim.tinh_trang" class="form-select">
                                <option value="0">Ngừng Chiếu</option>
                                <option value="2">Đang Chiếu</option>
                                <option value="1">Sắp Chiếu</option>
                            </select>
                        </div>

                        <!-- Mô tả -->
                        <div class="col-12">
                            <label class="form-label fw-semibold">Mô Tả</label>
                            <textarea v-model="create_phim.noi_dung" rows="3" class="form-control" placeholder="Nhập mô tả phim"></textarea>
                        </div>

                        <!-- Trailer -->
                        <div class="col-12">
                            <label class="form-label fw-semibold">Trailer</label>
                            <input v-model="create_phim.trailer" type="text" class="form-control" placeholder="Nhập URL trailer">
                        </div>
                    </div>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">
                        Đóng
                    </button>
                    <button type="button" class="btn btn-primary" data-bs-dismiss="modal" @click="addPhim()">
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
                    <h5 class="modal-title">Cập Nhật Thông Tin Phim</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">
                    <div class="row g-3">
                        <!-- Tên phim -->
                        <div class="col-md-6">
                            <label class="form-label fw-semibold">Tên Phim</label>
                            <input v-model="update_phim.ten_phim" type="text" class="form-control">
                        </div>

                        <!-- Hình ảnh -->
                        <div class="col-md-6">
                            <label class="form-label fw-semibold">Hình Ảnh</label>
                            <input v-model="update_phim.hinh_anh" type="text" class="form-control">
                        </div>

                        <!-- Đạo diễn -->
                        <div class="col-md-3">
                            <label class="form-label fw-semibold">Đạo Diễn</label>
                            <input v-model="update_phim.dao_dien" type="text" class="form-control">
                        </div>

                        <!-- Diễn viên -->
                        <div class="col-md-6">
                            <label class="form-label fw-semibold">Diễn Viên</label>
                            <input v-model="update_phim.dien_vien" type="text" class="form-control">
                        </div>

                        <!-- Nhà sản xuất -->
                        <div class="col-md-3">
                            <label class="form-label fw-semibold">Nhà Sản Xuất</label>
                            <input v-model="update_phim.nha_san_xuat" type="text" class="form-control">
                        </div>

                        <!-- Quốc gia -->
                        <div class="col-md-4">
                            <label class="form-label fw-semibold">Quốc Gia</label>
                            <input v-model="update_phim.quoc_gia" type="text" class="form-control">
                        </div>

                        <!-- Ngôn ngữ -->
                        <div class="col-md-4">
                            <label class="form-label fw-semibold">Ngôn Ngữ</label>
                            <input v-model="update_phim.ngon_ngu" type="text" class="form-control">
                        </div>

                        <!-- Ngày phát hành -->
                        <div class="col-md-4">
                            <label class="form-label fw-semibold">Ngày Phát Hành</label>
                            <input v-model="update_phim.ngay_phat_hanh" type="text" class="form-control">
                        </div>

                        <!-- Thể loại -->
                        <div class="col-md-6">
                            <label class="form-label fw-semibold">Thể Loại</label>
                            <input v-model="update_phim.the_loai" type="text" class="form-control">
                        </div>

                        <!-- Thời lượng -->
                        <div class="col-md-3">
                            <label class="form-label fw-semibold">Thời Lượng</label>
                            <input v-model="update_phim.thoi_luong" type="number" class="form-control">
                        </div>

                        <!-- Trạng thái -->
                        <div class="col-md-3">
                            <label class="form-label fw-semibold">Trạng Thái</label>
                            <select v-model="update_phim.tinh_trang" class="form-select">
                                <option value="0">Ngừng Chiếu</option>
                                <option value="2">Đang Chiếu</option>
                                <option value="1">Sắp Chiếu</option>
                            </select>
                        </div>

                        <!-- Mô tả -->
                        <div class="col-12">
                            <label class="form-label fw-semibold">Mô Tả</label>
                            <textarea v-model="update_phim.noi_dung" rows="3" class="form-control"></textarea>
                        </div>

                        <!-- Trailer -->
                        <div class="col-12">
                            <label class="form-label fw-semibold">Trailer</label>
                            <input v-model="update_phim.trailer" type="text" class="form-control">
                        </div>
                    </div>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">
                        Đóng
                    </button>
                    <button type="button" class="btn btn-success" data-bs-dismiss="modal" @click="updatePhim()">
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
                    <h5 class="modal-title">Xóa Phim</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">
                    <div class="alert alert-danger" role="alert">
                        Bạn có chắc chắn muốn xóa phim
                        <strong>{{ delete_phim.ten_phim }}</strong>?
                    </div>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">
                        Đóng
                    </button>
                    <button type="button" class="btn btn-danger" data-bs-dismiss="modal" @click="deletePhim()">
                        Xác nhận
                    </button>
                </div>
            </div>
        </div>
    </div>

    <!-- Model Chi Tiết -->
    <div class="modal fade" id="detailModal" tabindex="-1" aria-hidden="true">
        <div class="modal-dialog modal-xl">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title">Chi Tiết Phim</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">
                    <div class="row">
                        <div class="col-lg-12">
                            <iframe class="w-100 rounded" height="315"
                                v-bind:src="detail_phim.trailer"
                                title="YouTube video player" frameborder="0"
                                allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
                                allowfullscreen>
                            </iframe>
                        </div>
                    </div>
                    <div class="row mt-3">
                        <div class="col-lg-4 d-flex justify-content-center">
                            <img v-bind:src="detail_phim.hinh_anh" alt="" class="img-fluid rounded-3" style="height: 350px; width: 260px;">
                        </div>
                        <div class="col-lg-8">
                            <table class="table table-bordered table-hover">
                                <tbody>
                                    <tr>
                                        <th>Tên Phim</th>
                                        <td>{{ detail_phim.ten_phim }}</td>
                                    </tr>
                                    <tr>
                                        <th>Đạo Diễn</th>
                                        <td>{{ detail_phim.dao_dien }}</td>
                                    </tr>
                                    <tr>
                                        <th>Diễn Viên</th>
                                        <td>{{ detail_phim.dien_vien }}</td>
                                    </tr>
                                    <tr>
                                        <th>Nhà Sản Xuất</th>
                                        <td>{{ detail_phim.nha_san_xuat }}</td>
                                    </tr>
                                    <tr>
                                        <th class="text-nowrap">Ngày Phát Hành</th>
                                        <td>{{ detail_phim.ngay_phat_hanh }}</td>
                                    </tr>
                                    <tr>
                                        <th>Thể Loại</th>
                                        <td>{{ detail_phim.the_loai }}</td>
                                    </tr>
                                    <tr>
                                        <th>Ngôn Ngữ</th>
                                        <td>{{ detail_phim.ngon_ngu }}</td>
                                    </tr>
                                    <tr>
                                        <th>Thời Lượng</th>
                                        <td>{{ detail_phim.thoi_luong }} phút</td>
                                    </tr>
                                </tbody>
                            </table>
                        </div>
                    </div>
                    <div class="row mt-4">
                        <div class="col-lg-12">
                            <p class="fw-bold fs-4">Nội dung phim</p>
                            <p>{{ detail_phim.noi_dung }}</p>
                        </div>
                    </div>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">
                        Đóng
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
            list_phim: [],
            create_phim: {
                ten_phim: '',
                dao_dien: '',
                dien_vien: '',
                hinh_anh: '',
                quoc_gia: '',
                nha_san_xuat: '',
                trailer: '',
                ngay_phat_hanh: '',
                ngon_ngu: '',
                the_loai: '',
                thoi_luong: '',
                noi_dung: '',
                tinh_trang: '',
            },
            update_phim: {
                id: '',
                ten_phim: '',
                dao_dien: '',
                dien_vien: '',
                hinh_anh: '',
                quoc_gia: '',
                nha_san_xuat: '',
                trailer: '',
                ngay_phat_hanh: '',
                ngon_ngu: '',
                the_loai: '',
                thoi_luong: '',
                noi_dung: '',
                tinh_trang: '',
            },
            delete_phim: {
                id: '',
                ten_phim: '',
                dao_dien: '',
                dien_vien: '',
                hinh_anh: '',
                quoc_gia: '',
                nha_san_xuat: '',
                trailer: '',
                ngay_phat_hanh: '',
                ngon_ngu: '',
                the_loai: '',
                thoi_luong: '',
                noi_dung: '',
                tinh_trang: '',
            },
            detail_phim: {
                id: '',
                ten_phim: '',
                dao_dien: '',
                dien_vien: '',
                hinh_anh: '',
                quoc_gia: '',
                nha_san_xuat: '',
                trailer: '',
                ngay_phat_hanh: '',
                ngon_ngu: '',
                the_loai: '',
                thoi_luong: '',
                noi_dung: '',
                tinh_trang: '',
            },
        }
    },
    methods: {
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
        addPhim() {
            axios
                .post(apiUrl('admin/phim/add-data'), this.create_phim, {
                    headers: {
                        Authorization: "Bearer " + localStorage.getItem('key_admin')
                    }
                })
                .then((res) => {
                    if (res.data.status) {
                        this.loadDataPhim();
                        this.create_phim = {
                            
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
        updatePhim() {
            axios
                .post(apiUrl('admin/phim/update'), this.update_phim, {
                    headers: {
                        Authorization: "Bearer " + localStorage.getItem('key_admin')
                    }
                })
                .then((res) => {
                    if (res.data.status) {
                        this.loadDataPhim();
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
        deletePhim() {
            axios
                    .post(apiUrl('admin/phim/delete'), this.delete_phim, {
                        headers: {
                            Authorization: "Bearer " + localStorage.getItem('key_admin')
                        }
                    })
                .then((res) => {
                    if (res.data.status) {
                        this.loadDataPhim();
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
        }
    },
    mounted() {
        this.loadDataPhim();
    },
}
</script>
<style></style>