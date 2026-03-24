<template>
	<div class="container mt-5">
		<div class="row">
			<div class="col-lg-4">
				<div class="card shadow border-0">
					<div class="card-body p-0">
						<!-- Profile Header -->
						<div class="text-center p-4 bg-light border-bottom">
							<div class="mb-3">
								<img :src="profile.avatar"
									alt="Avatar" class="rounded-circle border border-2" width="100" height="100"
									style="object-fit: cover" />
							</div>
							<h5 class="fw-bold mb-1">{{ profile.ho_va_ten }}</h5>
							<p class="text-muted small mb-0">
								<i class="fa-regular fa-envelope me-1"></i>{{ profile.email }}
							</p>
						</div>

						<!-- Stats -->
						<div class="p-4">
							<div class="d-flex justify-content-between align-items-center mb-3 pb-3 border-bottom">
								<div class="d-flex align-items-center">
									<div class="bg-light rounded-circle p-2 me-3">
										<i class="fa-solid fa-receipt text-primary"></i>
									</div>
									<div>
										<div class="text-muted small mb-1">Tổng đơn hàng</div>
										<div class="fw-bold fs-5 mb-0">{{ tongDonHang }}</div>
									</div>
								</div>
							</div>
							<div class="d-flex justify-content-between align-items-center">
								<div class="d-flex align-items-center">
									<div class="bg-light rounded-circle p-2 me-3">
										<i class="fa-solid fa-wallet text-success"></i>
									</div>
									<div>
										<div class="text-muted small mb-1">Tổng chi tiêu</div>
										<div class="fw-bold fs-5 mb-0 text-success">{{ formatVND(tongChiTieu) }}</div>
									</div>
								</div>
							</div>
						</div>
					</div>
				</div>
			</div>
			<div class="col-lg-8">
				<div class="card">
					<div class="card-body">
						<ul class="nav nav-tabs nav-primary nav-fill text-center" role="tablist">
							<li class="nav-item" role="presentation">
								<a class="nav-link active" data-bs-toggle="tab" href="#profile" role="tab"
									aria-selected="true">
									<div class="d-flex align-items-center justify-content-center">
										<i class="bx bx-home font-18 me-1"></i>
										<div class="tab-title">Thông Tin Cá Nhân</div>
									</div>
								</a>
							</li>
							<li class="nav-item" role="presentation">
								<a class="nav-link" data-bs-toggle="tab" href="#changepassword" role="tab"
									aria-selected="false" tabindex="-1">
									<div class="d-flex align-items-center justify-content-center">
										<i class="bx bx-user-pin font-18 me-1"></i>
										<div class="tab-title">Đổi Mật Khẩu</div>
									</div>
								</a>
							</li>
							<li class="nav-item" role="presentation">
								<a class="nav-link" data-bs-toggle="tab" href="#history" role="tab"
									aria-selected="false" tabindex="-1" @click="loadDonHangs">
									<div class="d-flex align-items-center justify-content-center">
										<i class="fa-solid fa-clock-rotate-left me-1"></i>
										<div class="tab-title">Lịch Sử Giao Dịch</div>
									</div>
								</a>
							</li>
						</ul>
						<div class="tab-content py-3">
							<div class="tab-pane fade active show" id="profile" role="tabpanel">
								<div class="row">
									<div class="col-6">
										<div class="mb-2">
											<label for="">Họ và tên</label>
											<input v-model="update_profile.ho_va_ten" type="text"
												class="form-control mt-2" />
										</div>
									</div>
									<div class="col-6">
										<div class="mb-2">
											<label for="">Email</label>
											<input v-model="update_profile.email" type="text" class="form-control mt-2"
												disabled />
										</div>
									</div>
									<div class="col-6">
										<div class="mb-2">
											<label for="">Ngày sinh</label>
											<input v-model="update_profile.ngay_sinh" type="date"
												class="form-control mt-2" />
										</div>
									</div>
									<div class="col-6">
										<div class="mb-2">
											<label for="">Số điện thoại</label>
											<input v-model="update_profile.so_dien_thoai" type="text"
												class="form-control mt-2" maxlength="10" />
										</div>
									</div>
									<div class="col-12">
										<div class="mb-2">
											<label for="">Avatar</label>
											<input v-model="update_profile.avatar" type="text" class="form-control mt-2" />
										</div>
									</div>
								</div>
								<div class="row mt-3">
									<div class="col-lg-12 text-end">
										<button @click="updateProfile()" class="btn btn-success">Cập nhật</button>
									</div>
								</div>
							</div>
							<div class="tab-pane fade" id="changepassword" role="tabpanel">
								<div class="card">
									<div class="row g-0">
										<div class="col-lg-6 border-end">
											<div class="card-body">
												<div class="">
													<div class="mb-3 mt-2">
														<label class="form-label">Mật Khẩu Hiện Tại</label>
														<div class="input-group">
															<input :type="showPassword.password ? 'text' : 'password'"
																v-model="update_password_profile.password"
																class="form-control" placeholder="Nhập mật khẩu cũ" />
															<button type="button"
																@click="showPassword.password = !showPassword.password"
																class="btn btn-outline-secondary password-toggle">
																<i
																	:class="showPassword.password ? 'fas fa-eye-slash' : 'fas fa-eye'"></i>
															</button>
														</div>
													</div>
													<div class="mb-3 mt-2">
														<label class="form-label">Mật Khẩu Mới</label>
														<div class="input-group">
															<input
																:type="showPassword.new_password ? 'text' : 'password'"
																v-model="update_password_profile.new_password"
																class="form-control" placeholder="Nhập mật khẩu mới"
																required />
															<button type="button"
																@click="showPassword.new_password = !showPassword.new_password"
																class="btn btn-outline-secondary password-toggle">
																<i
																	:class="showPassword.new_password ? 'fas fa-eye-slash' : 'fas fa-eye'"></i>
															</button>
														</div>
													</div>
													<div class="mb-3">
														<label class="form-label">Xác Nhận Mật Khẩu</label>
														<div class="input-group">
															<input
																:type="showPassword.confirm_password ? 'text' : 'password'"
																v-model="update_password_profile.confirm_password"
																class="form-control"
																placeholder="Nhập xác nhận mật khẩu mới" required />
															<button type="button"
																@click="showPassword.confirm_password = !showPassword.confirm_password"
																class="btn btn-outline-secondary password-toggle">
																<i
																	:class="showPassword.confirm_password ? 'fas fa-eye-slash' : 'fas fa-eye'"></i>
															</button>
														</div>
													</div>
													<div class="mb-3 text-end">
														<router-link to="/client/quen-mat-khau">
															Quên mật khẩu
														</router-link>
													</div>
													<div class="d-grid gap-2">
														<button @click="updatePasswordProfile()" type="button"
															class="btn btn-primary">
															Đổi Mật Khẩu
														</button>
													</div>
												</div>
											</div>
										</div>
										<div class="col-lg-6">
											<img src="https://cdn.tgdd.vn/Files/2019/12/07/1225665/huong-dan-thay-doi-mat-khau-tai-khoan-google-don-gian-tren-dien-thoai-may-tinh-7.jpg"
												class="card-img login-img" alt="..." style="height: 325px" />
										</div>
									</div>
								</div>
							</div>
							<div class="tab-pane fade" id="history" role="tabpanel">
								<div v-for="(order, index) in donHangs" :key="order.id"
									class="card mb-3 border-0 shadow-sm">
									<div class="card-body">
										<div class="row align-items-center g-3">
											<!-- Movie Poster -->
											<div class="col-md-2 text-center">
												<img v-if="order.phim" :src="order.phim.hinh_anh" alt="Poster"
													class="img-fluid rounded shadow-sm"
													style="max-height: 150px; object-fit: cover;">
											</div>

											<!-- Order Info -->
											<div class="col-md-6">
												<!-- Order Code & Status -->
												<div class="mb-2">
													<span class="badge bg-secondary me-2">
														<i class="fas fa-barcode me-1"></i>{{ order.ma_don_hang }}
													</span>
													<span v-if="order.is_thanh_toan == 1" class="badge bg-success">
														<i class="fas fa-check-circle me-1"></i>Đã thanh toán
													</span>
													<span v-else class="badge bg-warning text-dark">
														<i class="fas fa-clock me-1"></i>Chưa thanh toán
													</span>

												</div>

												<!-- Movie Title -->
												<h5 class="mb-2" v-if="order.phim">
													<i class="fas fa-film text-danger me-2"></i>
													{{ order.phim.ten_phim }}
												</h5>

												<!-- Show Info -->
												<p class="mb-1 text-muted" v-if="order.phim">
													<i class="fas fa-calendar-day me-2"></i>
													<strong>{{ formatDateLong(order.phim.ngay_chieu) }}</strong>
													<span class="mx-2">•</span>
													<i class="fas fa-clock me-1"></i>{{ order.phim.thoi_gian_bat_dau }}
												</p>

												<!-- Room & Seats -->
												<p class="mb-1 text-muted" v-if="order.phim">
													<i class="fas fa-door-open me-2"></i>{{ order.phim.ten_phong }}
													<span class="mx-2" v-if="order.danh_sach_ghe">•</span>
													<i class="fas fa-couch me-1" v-if="order.danh_sach_ghe"></i>
													<strong v-if="order.danh_sach_ghe">{{ order.so_ve }} vé</strong>
												</p>

												<!-- Voucher -->
												<p class="mb-0" v-if="order.voucher">
													<span class="badge bg-success">
														<i class="fas fa-tag me-1"></i>{{ order.voucher.ma_code }}
													</span>
												</p>
											</div>

											<!-- Price & Actions -->
											<div class="col-md-4">
												<div class="text-md-end">
													<!-- Pricing -->
													<div class="mb-3">
														<div class="text-muted small mb-1">
															<del v-if="order.giam_gia > 0">{{ formatVND(order.tong_tien)
															}}</del>
														</div>
														<div class="fs-4 fw-bold text-danger mb-1">
															{{ formatVND(order.tien_thuc_nhan) }}
														</div>
														<div v-if="order.giam_gia > 0" class="badge bg-success">
															Tiết kiệm {{ formatVND(order.giam_gia) }}
														</div>
													</div>

													<!-- Action Buttons -->
													<div class="d-grid gap-2">
														<button @click="viewChiTiet(order)"
															class="btn btn-outline-primary btn-sm"
															data-bs-toggle='modal' data-bs-target='#chiTietModal'>
															<i class="fas fa-eye me-1"></i>Xem chi tiết
														</button>
														<button v-if="!order.is_thanh_toan"
															@click="goToPayment(order.ma_don_hang)"
															class="btn btn-warning btn-sm">
															<i class="fas fa-qrcode me-1"></i>Thanh toán ngay
														</button>
													</div>
												</div>
											</div>
										</div>
									</div>
								</div>
							</div>
						</div>
					</div>
				</div>
			</div>
		</div>
	</div>

	<!-- Modal xem chi tiết -->
	<div class="modal fade" id="chiTietModal" tabindex="-1" aria-labelledby="chiTietModalLabel" aria-hidden="true">
		<div class="modal-dialog modal-xl">
			<div class="modal-content" v-if="selectedOrder">
				<div class="modal-header bg-primary text-white">
					<h5 class="modal-title text-white" id="chiTietModalLabel">Chi tiết đơn hàng: {{
						selectedOrder.ma_don_hang }}</h5>
					<button type="button" class="btn-close btn-close-white" data-bs-dismiss="modal"
						aria-label="Close"></button>
				</div>
				<div class="modal-body">
					<div class="row mb-3">
						<div class="col-12">
							<div v-if="selectedOrder.is_thanh_toan" class="alert alert-success">
								<i class="fas fa-check-circle me-2"></i>
								<strong>Đã thanh toán</strong>
							</div>
							<div v-else class="alert alert-warning">
								<i class="fas fa-clock me-2"></i>
								<strong>Chờ thanh toán</strong>
							</div>
						</div>
					</div>
					<div class="row">
						<!-- Thông tin phim -->
						<div class="col-lg-5 mb-3">
							<div class="card">
								<div class="card-header bg-info text-white">
									<h6 class="mb-0"><i class="fas fa-film me-2"></i>Thông Tin Phim</h6>
								</div>
								<div class="card-body" v-if="selectedOrder.phim">
									<div class="text-center mb-3">
										<img :src="selectedOrder.phim.hinh_anh" alt="Movie" class="img-fluid rounded"
											style="max-height: 300px;">
									</div>
									<table class="table table-borderless table-sm">
										<tr>
											<td width="40%"><strong>Tên phim:</strong></td>
											<td>{{ selectedOrder.phim.ten_phim }}</td>
										</tr>
										<tr>
											<td><strong>Ngày chiếu:</strong></td>
											<td>{{ formatDateShort(selectedOrder.phim.ngay_chieu) }}</td>
										</tr>
										<tr>
											<td><strong>Giờ chiếu:</strong></td>
											<td>{{ selectedOrder.phim.thoi_gian_bat_dau }}</td>
										</tr>
										<tr>
											<td><strong>Phòng chiếu:</strong></td>
											<td>{{ selectedOrder.phim.ten_phong }}</td>
										</tr>
									</table>
								</div>
							</div>
						</div>

						<!-- Ghế, Dịch vụ, Voucher & Tổng tiền -->
						<div class="col-lg-7">
							<!-- Danh sách ghế -->
							<div class="card mb-3">
								<div class="card-header bg-warning text-dark">
									<h6 class="mb-0"><i class="fas fa-couch me-2"></i>Ghế Đã Chọn</h6>
								</div>
								<div class="card-body"
									v-if="selectedOrder.danh_sach_ghe && selectedOrder.danh_sach_ghe.length > 0">
									<div class="d-flex flex-wrap gap-2">
										<span v-for="ghe in selectedOrder.danh_sach_ghe" :key="ghe"
											class="badge bg-warning text-dark fs-6 px-3 py-2">
											{{ ghe }}
										</span>
									</div>
								</div>
							</div>

							<!-- Dịch vụ -->
							<div class="card mb-3" v-if="selectedOrder.dich_vu && selectedOrder.dich_vu.length > 0">
								<div class="card-header bg-info text-white">
									<h6 class="mb-0"><i class="fas fa-concierge-bell me-2"></i>Dịch Vụ Đã Đặt</h6>
								</div>
								<div class="card-body">
									<div class="table-responsive">
										<table class="table table-sm table-hover mb-0">
											<thead>
												<tr>
													<th>#</th>
													<th>Dịch vụ</th>
													<th class="text-center">SL</th>
													<th class="text-end">Đơn giá</th>
													<th class="text-end">Thành tiền</th>
												</tr>
											</thead>
											<tbody>
												<tr v-for="(dv, index) in selectedOrder.dich_vu" :key="index"
													class="align-middle">
													<td>{{ index + 1 }}</td>
													<td>
														<div class="d-flex align-items-center">
															<img v-if="dv.hinh_anh" :src="dv.hinh_anh" alt="Service"
																class="rounded me-2"
																style="width: 40px; height: 40px; object-fit: cover;">
															<span>{{ dv.ten_dich_vu }}</span>
														</div>
													</td>
													<td class="text-center">{{ dv.so_luong }}</td>
													<td class="text-end">{{ formatVND(dv.don_gia) }}</td>
													<td class="text-end fw-bold">{{ formatVND(dv.thanh_tien) }}</td>
												</tr>
											</tbody>
										</table>
									</div>
								</div>
							</div>

							<!-- Voucher -->
							<div class="card mb-3" v-if="selectedOrder.voucher">
								<div class="card-header bg-success text-white">
									<h6 class="mb-0"><i class="fas fa-tag me-2"></i>Mã Giảm Giá</h6>
								</div>
								<div class="card-body">
									<div class="alert alert-success mb-0">
										<strong>{{ selectedOrder.voucher.ma_code }}</strong>
									</div>
								</div>
							</div>

							<!-- Tổng tiền -->
							<div class="card">
								<div class="card-header bg-secondary text-white">
									<h6 class="mb-0 text-white"><i class="fas fa-receipt me-2"></i>Tổng Thanh Toán</h6>
								</div>
								<div class="card-body">
									<table class="table table-borderless mb-0">
										<tr>
											<td>Tổng tiền:</td>
											<td class="text-end"><strong>{{ formatVND(selectedOrder.tong_tien)
											}}</strong></td>
										</tr>
										<tr v-if="selectedOrder.giam_gia > 0" class="text-success">
											<td>Giảm giá:</td>
											<td class="text-end"><strong>-{{ formatVND(selectedOrder.giam_gia)
											}}</strong></td>
										</tr>
										<tr class="border-top">
											<td class="fs-5"><strong>Thành tiền:</strong></td>
											<td class="text-end fs-4 text-danger"><strong>{{
												formatVND(selectedOrder.tien_thuc_nhan) }}</strong></td>
										</tr>
									</table>
								</div>
							</div>
						</div>
					</div>
				</div>
				<div class="modal-footer">
					<button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Đóng</button>
					<button v-if="!selectedOrder.is_thanh_toan" type="button" class="btn btn-warning"
						@click="goToPaymentFromModal(selectedOrder.ma_don_hang)">
						<i class="fas fa-qrcode me-1"></i>Thanh Toán Ngay
					</button>
					<button v-else type="button" class="btn btn-outline-info" 
						@click="viewTicketFromModal(selectedOrder.ma_don_hang)">
						<i class="fas fa-ticket me-1"></i>Xem Lại Vé
					</button>
				</div>
			</div>
		</div>
	</div>
</template>
<script>
import axios from "axios";
import apiUrl from "../../../utils/api";

export default {
	data() {
		return {
			profile: {
				ho_va_ten: "",
				email: "",
				ngay_sinh: "",
				so_dien_thoai: "",
				avatar: ""
			},
			update_profile: {
				ho_va_ten: "",
				email: "",
				ngay_sinh: "",
				so_dien_thoai: "",
			},
			update_password_profile: {
				password: '',
				new_password: '',
				confirm_password: ''
			},
			showPassword: {
				password: false,
				new_password: false,
				confirm_password: false
			},
			donHangs: [],
			loadingOrders: false,
			selectedOrder: null
		};
	},
	computed: {
		tongDonHang() {
			return this.donHangs ? this.donHangs.length : 0;
		},
		tongChiTieu() {
			if (!this.donHangs || this.donHangs.length === 0) return 0;
			return this.donHangs.reduce((sum, order) => sum + (order.tien_thuc_nhan || 0), 0);
		}
	},
	mounted() {
		this.loadData();
	},
	methods: {
		formatVND(number) {
			return new Intl.NumberFormat("vi-VI", { style: "currency", currency: "VND" }).format(number);
		},
		loadData() {
			axios
				.get(apiUrl("client/profile/get-data"), {
					headers: {
						Authorization: "Bearer " + localStorage.getItem("key_client"),
					},
				})
				.then((res) => {
					if (res.data.status) {
						this.profile = res.data.data;
						this.update_profile = { ...res.data.data };
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
		loadDonHangs() {
			axios
				.get(apiUrl("client/don-hang/danh-sach"), {
					headers: {
						Authorization: "Bearer " + localStorage.getItem("key_client"),
					},
				})
				.then((res) => {
					if (res.data.status) {
						this.donHangs = res.data.data;
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
		updateProfile() {
			axios
				.post(apiUrl("client/profile/update"), this.update_profile, {
					headers: {
						Authorization: "Bearer " + localStorage.getItem("key_client"),
					},
				})
				.then((res) => {
					if (res.data.status) {
						this.loadData();
						this.$toast.success(res.data.message);

						// thay đổi tên và avarar trong profile thì header cũng thay đổi theo
						localStorage.setItem("avatar", this.update_profile.avatar);
                        localStorage.setItem("ho_va_ten", this.update_profile.ho_va_ten);
                        window.dispatchEvent(new Event("profile-updated"));


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
		updatePasswordProfile() {
			axios
				.post(apiUrl("client/doi-mat-khau"), this.update_password_profile, {
					headers: {
						Authorization: "Bearer " + localStorage.getItem("key_client"),
					},
				})
				.then((res) => {
					if (res.data.status) {
						this.$toast.success(res.data.message);
						this.update_password_profile = {
							password: '',
							new_password: '',
							confirm_password: ''
						};
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
		goToPayment(maDonHang) {
			this.$router.push(`/client/thanh-toan/${maDonHang}`);
		},
		viewChiTiet(order) {
			this.selectedOrder = order;
		},
		goToPaymentFromModal(maDonHang) {
			const modalElement = document.getElementById('chiTietModal');
			const modal = bootstrap.Modal.getInstance(modalElement);
			if (modal) {
				modal.hide();
			}
			this.goToPayment(maDonHang);
		},
		viewTicketFromModal(maDonHang) {
			// 1. Đóng modal trước
			const modalElement = document.getElementById('chiTietModal');
			const modal = bootstrap.Modal.getInstance(modalElement);
			if (modal) {
				modal.hide();
			}

			// 2. Chuyển hướng sang trang chi tiết (Vé)
			// Lưu ý: Logic bên trang ThanhToan.vue của bạn đã xử lý việc:
			// Nếu đơn hàng đã thanh toán -> Tự hiển thị vé/thành công.
			this.$router.push({
				name: 'admin-in-ve', 
				params: { ma_hoa_don: maDonHang }
			});
		},
		formatDateTime(dateTime) {
			if (!dateTime) return '';
			const d = new Date(dateTime);
			return `${d.getDate()}/${d.getMonth() + 1}/${d.getFullYear()} ${d.getHours()}:${String(d.getMinutes()).padStart(2, '0')}`;
		},
		formatDateLong(date) {
			if (!date) return '';
			const days = ['CN', 'T2', 'T3', 'T4', 'T5', 'T6', 'T7'];
			const d = new Date(date);
			return `${days[d.getDay()]}, ${d.getDate()}/${d.getMonth() + 1}/${d.getFullYear()}`;
		},
		formatDateShort(date) {
			if (!date) return '';
			const d = new Date(date);
			return `${d.getDate()}/${d.getMonth() + 1}/${d.getFullYear()}`;
		}
	},
};
</script>

<style></style>
