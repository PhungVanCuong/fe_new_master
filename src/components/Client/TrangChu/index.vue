<template>
    <!-- Slide-->
    <div id="carouselExample" class="carousel slide" data-bs-ride="carousel" data-bs-interval="3000">
        <div class="carousel-inner">
            <template v-for="(value, index) in list_slide" :key="index">
                <div class="carousel-item" :class="{ 'active': index === 0 }">
                    <img :src="value.hinh_anh" class="d-block w-100" :alt="value.tieu_de || 'Slide'"
                        style="height: 700px; object-fit: contain; object-position: center; background-color: #000;">
                </div>
            </template>
        </div>
        <button class="carousel-control-prev" type="button" data-bs-target="#carouselExample" data-bs-slide="prev">
            <span class="carousel-control-prev-icon" aria-hidden="true"></span>
            <span class="visually-hidden">Previous</span>
        </button>
        <button class="carousel-control-next" type="button" data-bs-target="#carouselExample" data-bs-slide="next">
            <span class="carousel-control-next-icon" aria-hidden="true"></span>
            <span class="visually-hidden">Next</span>
        </button>
    </div>
    <!-- Dịch vụ nổi bật -->
    <div class="container my-5">
        <div class="row g-4 text-center">
            <div class="col-md-3">
                <div class="service-card p-4 rounded-4 h-100"
                    style="background-image: url('https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSLyg8w7yRqHIAFBkTzMPy_TumLgDvMG66dXw&s'); border: 1px solid rgba(227, 204, 129, 0.3);">
                    <div class="icon-box mx-auto mb-3"
                        style="background: linear-gradient(135deg, #E3CC81, #d4b455); width: 60px; height: 60px; border-radius: 50%; display: flex; align-items: center; justify-content: center;">
                        <i class="fa-solid fa-bolt fs-3 text-dark service-icon"></i>
                    </div>
                    <h5 class="fw-bold text-white mb-1">Đặt vé nhanh chóng</h5>
                    <p class="text-white-50 small mb-0 text-uppercase tracking-widest" style="letter-spacing: 1px;">Chọn
                        ghế
                        trực tuyến, không phải chờ đợi, chỉ với vài thao tác đơn
                        giản.</p>
                </div>
            </div>
            <div class="col-md-3">
                <div class="service-card p-4 rounded-4 h-100"
                    style="background-image: url('https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSLyg8w7yRqHIAFBkTzMPy_TumLgDvMG66dXw&s'); border: 1px solid rgba(227, 204, 129, 0.3);">
                    <div class="icon-box mx-auto mb-3"
                        style="background: linear-gradient(135deg, #E3CC81, #d4b455); width: 60px; height: 60px; border-radius: 50%; display: flex; align-items: center; justify-content: center;">
                        <i class="fa-solid fa-film fs-3 text-dark service-icon"></i>
                    </div>
                    <h5 class="fw-bold text-white mb-2">Kho phim đa dạng</h5>
                    <p class="text-white-50 small mb-0 text-uppercase tracking-widest" style="letter-spacing: 1px;">Từ
                        bom
                        tấn Hollywood đến phim Việt và hoạt hình cho trẻ em.</p>
                </div>
            </div>
            <div class="col-md-3">
                <div class="service-card p-4 rounded-4 h-100"
                    style="background-image: url('https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSLyg8w7yRqHIAFBkTzMPy_TumLgDvMG66dXw&s'); border: 1px solid rgba(227, 204, 129, 0.3);">
                    <div class="icon-box mx-auto mb-3"
                        style="background: linear-gradient(135deg, #E3CC81, #d4b455); width: 60px; height: 60px; border-radius: 50%; display: flex; align-items: center; justify-content: center;">
                        <i class="fa-solid fa-calendar-check fs-3 text-dark service-icon"></i>
                    </div>
                    <h5 class="fw-bold text-white mb-2">Lịch chiếu linh hoạt</h5>
                    <p class="text-white-50 small mb-0 text-uppercase tracking-widest" style="letter-spacing: 1px;">Các
                        suất
                        chiếu trải dài cả ngày, phù hợp mọi khung giờ rảnh rỗi.</p>
                </div>
            </div>
            <div class="col-md-3">
                <div class="service-card p-4 rounded-4 h-100"
                    style="background-image: url('https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSLyg8w7yRqHIAFBkTzMPy_TumLgDvMG66dXw&s'); border: 1px solid rgba(227, 204, 129, 0.3);">
                    <div class="icon-box mx-auto mb-3"
                        style="background: linear-gradient(135deg, #E3CC81, #d4b455); width: 60px; height: 60px; border-radius: 50%; display: flex; align-items: center; justify-content: center;">
                        <i class="fa-solid fa-martini-glass fs-3 text-dark service-icon"></i>
                    </div>
                    <h5 class="fw-bold text-white mb-2">Combo bắp nước ưu đãi</h5>
                    <p class="text-white-50 small mb-0 text-uppercase tracking-widest" style="letter-spacing: 1px;">
                        Nhiều
                        lựa chọn hấp dẫn, ưu đãi riêng cho thành viên thân thiết.</p>
                </div>
            </div>
        </div>
    </div>
    <!-- Đặt vé nhanh -->
    <div class="container p-0 mt-5 mb-4 rounded-pill"
        style=" margin: 0 auto; z-index: 10; position: relative; border: 1px solid #eee; ">
        <div class="row g-0 align-items-center" style="height: 50px;">
            <div class="col-lg-3 col-md-6 h-100" style="border-right: 1px solid #eee;">
                <div class="d-flex flex-column justify-content-center px-3 h-100">
                    <label class="mb-1 text-truncate" style="color: white; font-size: 13px; font-weight: 700;">
                        1. Chọn Phim
                    </label>
                    <select class="form-select border-0 p-0 shadow-none"
                        style="font-weight: 600; outline: none; background-color: transparent; color: white;"
                        v-model="selected_phim" @change="loadNgayChieu()">
                        <option style="background-color: #323232; color: white;" value="" selected>Chọn phim
                        </option>
                        <option style="background-color: #323232;" v-for="(v, k) in list_phim_dropdown" :key="k"
                            :value="v.id">{{ v.ten_phim }}
                        </option>
                    </select>
                </div>
            </div>
            <div class="col-lg-3 col-md-6 h-100" style="border-right: 1px solid #eee;">
                <div class="d-flex flex-column justify-content-center px-3 h-100">
                    <label class="mb-1 text-truncate" style="color: white; font-size: 13px; font-weight: 700;">
                        2. Chọn Ngày
                    </label>
                    <select class="form-select border-0 p-0 shadow-none"
                        style="font-weight: 600; outline: none; background-color: transparent; color: white;"
                        v-model="selected_ngay" @change="loadSuatChieu()" :disabled="!selected_phim">
                        <option style="background-color: #323232; color: white;" value="" selected>Chọn ngày
                        </option>
                        <option style="background-color: #323232; " v-for="(v, k) in list_ngay" :key="k"
                            :value="v.ngay_chieu">{{
                                formatDate(v.ngay_chieu)
                            }}</option>
                    </select>
                </div>
            </div>
            <div class="col-lg-3 col-md-6 h-100" style="border-right: 1px solid #eee;">
                <div class="d-flex flex-column justify-content-center px-3 h-100">
                    <label class="mb-1 text-truncate" style="color: white; font-size: 13px; font-weight: 700;">
                        3. Chọn Suất
                    </label>
                    <select class="form-select border-0 p-0 shadow-none"
                        style="font-weight: 600; outline: none; background-color: transparent; color: white;"
                        v-model="selected_suat" :disabled="!selected_ngay">
                        <option style="background-color: #323232; color: white;" value="" selected>Chọn suất
                        </option>
                        <option style="background-color: #323232; " v-for="(v, k) in list_suat" :key="k" :value="v.id">
                            {{
                                formatTime(v.thoi_gian_bat_dau) }}
                        </option>
                    </select>
                </div>
            </div>
            <div class="col-lg-3 col-md-6 h-100 ">

                <button class="btn w-100 h-100 rounded-start rounded-pill" @click="muaVeNgay()"
                    :disabled="!selected_suat"
                    style="background-color: transparent; color: #E4445E; border: none; font-weight: 700; text-transform: uppercase; font-size: 1.1rem; transition: all 0.3s;"
                    :style="!selected_suat ? { color: '#999', cursor: 'not-allowed' } : {}"
                    onmouseover="if(!this.disabled) { this.style.backgroundColor='#E4445E'; this.style.color='white'; }"
                    onmouseout="if(!this.disabled) { this.style.backgroundColor='transparent'; this.style.color='#E4445E'; }">
                    MUA VÉ NGAY
                </button>
            </div>
        </div>
    </div>
    <!-- Phim đang chiếu -->
    <h3 class="text-center text-white mt-5 fw-bold">PHIM ĐANG CHIẾU</h3>
    <div class="container">
        <div class="row mt-4">
            <template v-for="(value, index) in list_phim.filter(x => x.tinh_trang === 2).slice(0, 4)" :key="index">
                <div class="col-lg-3 col-md-6 mb-4 mt-2">
                    <div class="now-showing-card rounded-4 overflow-hidden position-relative "
                        style="height: 500px; width: 300px; border: 1px solid rgba(227, 204, 129, 0.3);">
                        <img :src="value.hinh_anh" class="w-100 h-100 object-fit-cover transition-transform"
                            alt="Poster">

                        <div class="info-overlay p-3 d-flex flex-column justify-content-end">
                            <h5 class="fw-bold text-white mb-1 text-truncate">{{ value.ten_phim }}</h5>
                            <div class="d-flex justify-content-between align-items-center">
                                <span style="color: #FF8D00; font-size: 13px; font-weight: 600;">{{ value.the_loai
                                }}</span>
                                <div style="color: #FEE715; font-size: 13px;">
                                    <i class="fa-solid fa-star me-1"></i><span class="text-white fw-bold">{{
                                        Number(value.danh_gias_avg_so_sao || 0).toFixed(1) }}</span>
                                </div>
                            </div>
                        </div>

                        <div class="hover-overlay d-flex flex-column align-items-center justify-content-center">
                            <a :href="value.trailer" target="_blank" class="text-decoration-none mb-4"
                                title="Xem Trailer Phim">
                                <i class="fa-regular fa-circle-play text-white hover-play-icon"
                                    style="font-size: 4.5rem;"></i>
                            </a>

                            <router-link :to="`/chi-tiet-phim/${value.id}`" class="text-decoration-none">
                                <button class="btn fw-bold text-uppercase px-4 py-2 hover-btn-detail"
                                    style="background-color: #E4445E; color: white; border-radius: 4px; border: 1px solid #E4445E;">
                                    Xem chi tiết phim
                                </button>
                            </router-link>
                        </div>
                    </div>
                </div>
            </template>
        </div>
        <div class="text-center mb-5 mt-3">
            <router-link to="/client/phim/dang-chieu">
                <button class="btn rounded-pill"
                    style="background-color: transparent; border: 1px solid #E4445E; color: #E4445E; transition: all 0.3s ease;"
                    onmouseover="this.style.backgroundColor='#E4445E'; this.style.color='white'"
                    onmouseout="this.style.backgroundColor='transparent'; this.style.color='#E4445E'">Xem thêm
                    phim</button>
            </router-link>
        </div>
    </div>
    <!-- Về chúng tôi -->
    <div class="container my-5">
        <div class="card p-5 shadow-lg rounded-4 " style="background: url('https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSLyg8w7yRqHIAFBkTzMPy_TumLgDvMG66dXw&s'); 
             border: 1px solid #E3CC81;">
            <h3 class="text-center text-uppercase fw-bold mb-5"
                style="color: white; letter-spacing: 2px; text-shadow: 0 0 10px rgba(227, 204, 129, 0.5);">
                Về chúng tôi
            </h3>
            <div class="row align-items-center">
                <div class="col-md-7">
                    <h2 class="fw-bold mb-3 display-6 text-white">
                        Mua vé xem phim Online trên <br>
                        <span style="color: #E3CC81; text-shadow: 2px 2px 4px rgba(0,0,0,0.5);">Tr0ondCinema</span>
                    </h2>
                    <p class="fs-5 mb-4" style="color: rgba(255, 255, 255, 0.85); line-height: 1.6;">
                        Trải nghiệm mua vé trực tuyến dễ dàng, nhiều ưu đãi hấp dẫn và kết nối với các cụm rạp lớn trên
                        toàn
                        quốc.
                    </p>
                    <ul class="list-unstyled fs-5">
                        <li class="mb-3 d-flex align-items-center">
                            <i class="fa fa-check-circle me-3 fs-4" style="color: #E3CC81;"></i>
                            <span style="color: #f0f0f0;">Mua vé Online, trải nghiệm phim hay</span>
                        </li>
                        <li class="mb-3 d-flex align-items-center">
                            <i class="fa fa-check-circle me-3 fs-4" style="color: #E3CC81;"></i>
                            <span style="color: #f0f0f0;">Đặt vé an toàn, tiện lợi trong vài bước</span>
                        </li>
                        <li class="mb-3 d-flex align-items-center">
                            <i class="fa fa-check-circle me-3 fs-4" style="color: #E3CC81;"></i>
                            <span style="color: #f0f0f0;">Tự do chọn chỗ ngồi, combo bắp nước siêu hot</span>
                        </li>
                        <li class="mb-3 d-flex align-items-center">
                            <i class="fa fa-check-circle me-3 fs-4" style="color: #E3CC81;"></i>
                            <span style="color: #f0f0f0;">Lưu lịch sử đặt vé, quản lý vé dễ dàng</span>
                        </li>
                    </ul>
                </div>
                <div class="col-md-5 text-center">
                    <img src="../../../assets/images/1.png" alt="Mua vé online" class="img-fluid rounded-3 shadow">
                </div>
            </div>
        </div>
    </div>
    <!-- Phim sắp chiếu -->
    <h3 class="text-center mb-4 text-white mt-5 fw-bold">PHIM SẮP CHIẾU</h3>
    <div class="container">
        <div class="row">
            <template v-for="(value, index) in list_phim.filter(x => x.tinh_trang === 1).slice(0, 4)" :key="index">
                <div class="col-md-3 mb-4 d-flex">
                    <router-link :to="`/chi-tiet-phim/${value.id}`" class="text-decoration-none w-100">
                        <div class="coming-soon-card rounded-4 overflow-hidden position-relative "
                            style="height: 500px; width: 300px; border: 1px solid rgba(227, 204, 129, 0.3);">
                            <img :src="value.hinh_anh" class="w-100 h-100 object-fit-cover transition-transform"
                                alt="Poster">
                            <div class="coming-soon-overlay p-3 d-flex flex-column justify-content-end">
                                <span class="badge bg-light text-dark align-self-start mb-2 fw-bold px-2 py-1"
                                    style="font-size: 12px;">{{ value.ngay_phat_hanh }}</span>
                                <h5 class="fw-bold text-white mb-0 text-truncate">{{ value.ten_phim }}</h5>
                            </div>
                        </div>
                    </router-link>
                </div>
            </template>
        </div>
        <div class="text-center mb-5 mt-2">
            <router-link to="/client/phim/sap-chieu">
                <button class="btn rounded-pill"
                    style="background-color: transparent; border: 1px solid #E4445E; color: #E4445E; transition: all 0.3s ease;"
                    onmouseover="this.style.backgroundColor='#E4445E'; this.style.color='white'"
                    onmouseout="this.style.backgroundColor='transparent'; this.style.color='#E4445E'">Xem tất cả phim
                    sắp
                    chiếu</button>
            </router-link>
        </div>
    </div>
    <!-- Bài viết -->
    <h3 class="text-center mb-4 mt-5 text-white fw-bold">BÀI VIẾT</h3>
    <div class="container">
        <div class="row">
            <template v-for="(value, index) in list_bv.slice(0, 3)" :key="index">
                <div class="col-lg-4 mb-4">
                    <router-link :to="`/chi-tiet-bai-viet/${value.id}`" class="text-decoration-none">
                        <article class="article-card group cursor-pointer h-100">
                            <div class="overflow-hidden rounded-4 mb-3 position-relative"
                                style="height: 250px; border: 1px solid rgba(227, 204, 129, 0.3);">
                                <img :src="value.hinh_anh"
                                    class="w-100 h-100 object-fit-cover transition-transform article-img">
                                <span class="position-absolute top-0 start-0 m-3 badge"
                                    style="background-color: #191970; color: #E3CC81; border: 1px solid #E3CC81;">Tin
                                    Tức</span>
                            </div>
                            <h5 class="fw-bold text-white mb-2 article-title transition-colors line-clamp-2">{{
                                value.tieu_de }}</h5>
                            <p class="text-white-50 small mb-0 line-clamp-2">{{ value.mo_ta_ngan }}</p>
                        </article>
                    </router-link>
                </div>
            </template>
        </div>
        <div class="text-center mb-5 mt-3">
            <router-link to="/client/bai-viet">
                <button class="btn rounded-pill"
                    style="background-color: transparent; border: 1px solid #E4445E; color: #E4445E; transition: all 0.3s ease;"
                    onmouseover="this.style.backgroundColor='#E4445E'; this.style.color='white'"
                    onmouseout="this.style.backgroundColor='transparent'; this.style.color='#E4445E'">Xem thêm bài
                    viết</button>
            </router-link>
        </div>
    </div>
    <!-- Review -->
    <div class=" container review-section my-5 p-4 rounded-4" @mouseenter="stopAutoSlide" @mouseleave="startAutoSlide"
        style="background: url('https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSLyg8w7yRqHIAFBkTzMPy_TumLgDvMG66dXw&s');
            border: 2px solid rgb(227, 204, 129); padding: 60px 0; font-family: 'Roboto', sans-serif; border-radius: 20px;  background-position: center;">
        <div class="container">
            <div class="text-start mb-5" style="position: relative;">
                <h3 class="fw-bold text-white display-5">
                    Khách Hàng Nói Gì <br> Về <span style="color: #E3CC81;">Tr0ondCinema</span>?
                </h3>
                <div class="mt-2" style="height: 4px; width: 80px; background-color: #E3CC81; border-radius: 2px;">
                </div>

                <div class="mt-3"
                    style="width: 100px; height: 3px; background: rgba(255,255,255,0.2); border-radius: 2px; overflow: hidden;">
                    <div :style="{ width: progressWidth + '%', transition: 'width 0.1s linear' }"
                        style="height: 100%; background-color: #E3CC81;"></div>
                </div>

                <p class="text-white-50 mt-3 fs-5">Những trải nghiệm thực tế từ khán giả sau khi thưởng thức phim.</p>
            </div>

            <div class="row g-4 align-items-center">
                <div class="col-lg-7">
                    <div class="review-display-card"
                        style="background-color: #F0EEE9; border: 2px solid #E3CC81; border-radius: 15px; box-shadow: 0 10px 30px rgba(0,0,0,0.3); padding: 40px; position: relative; min-height: 300px; display: flex; flex-direction: column; justify-content: center;">

                        <i class="fa-solid fa-quote-left"
                            style="position: absolute; top: 20px; left: 30px; font-size: 6rem; color: rgba(255, 255, 255, 0.5); z-index: 0; opacity: 0.1;"></i>

                        <transition name="fade" mode="out-in">
                            <div v-if="activeTab === 1" key="1" class="position-relative" style="z-index: 1;">
                                <div class="mb-3 text-warning fs-5">
                                    <i class="fa-solid fa-star"></i><i class="fa-solid fa-star"></i><i
                                        class="fa-solid fa-star"></i><i class="fa-solid fa-star"></i><i
                                        class="fa-solid fa-star"></i>
                                </div>
                                <p class="fst-italic mb-4 text-dark lh-base" style="font-size: 1.2rem;">
                                    "Rạp Tr0ondCinema có không gian hiện đại, âm thanh sống động và dịch vụ tuyệt vời.
                                    Tôi và gia đình luôn chọn nơi đây mỗi cuối tuần để thư giãn."
                                </p>
                                <div class="d-flex align-items-center mt-4 pt-3"
                                    style="border-top: 1px solid rgba(0,0,0,0.1);">
                                    <img src="https://randomuser.me/api/portraits/men/32.jpg"
                                        class="rounded-circle me-3" width="60" height="60"
                                        style="object-fit: cover; border: 2px solid #E3CC81;">
                                    <div>
                                        <h5 class="mb-0 fw-bold" style="color: #191970;">Nguyễn Văn A</h5>
                                        <small class="text-muted">CEO Công ty ABC</small>
                                    </div>
                                </div>
                            </div>

                            <div v-else-if="activeTab === 2" key="2" class="position-relative" style="z-index: 1;">
                                <div class="mb-3 text-warning fs-5">
                                    <i class="fa-solid fa-star"></i><i class="fa-solid fa-star"></i><i
                                        class="fa-solid fa-star"></i><i class="fa-solid fa-star"></i><i
                                        class="fa-solid fa-star"></i>
                                </div>
                                <p class="fst-italic mb-4 text-dark lh-base" style="font-size: 1.2rem;">
                                    "Tôi rất thích đặt vé online trên website Tr0ondCinema – giao diện trực quan, thanh
                                    toán dễ dàng và đặc biệt là chọn được chỗ ngồi yêu thích."
                                </p>
                                <div class="d-flex align-items-center mt-4 pt-3"
                                    style="border-top: 1px solid rgba(0,0,0,0.1);">
                                    <img src="https://randomuser.me/api/portraits/women/44.jpg"
                                        class="rounded-circle me-3" width="60" height="60"
                                        style="object-fit: cover; border: 2px solid #E3CC81;">
                                    <div>
                                        <h5 class="mb-0 fw-bold" style="color: #191970;">Trần Thị B</h5>
                                        <small class="text-muted">Giám đốc XYZ Shop</small>
                                    </div>
                                </div>
                            </div>

                            <div v-else-if="activeTab === 3" key="3" class="position-relative" style="z-index: 1;">
                                <div class="mb-3 text-warning fs-5">
                                    <i class="fa-solid fa-star"></i><i class="fa-solid fa-star"></i><i
                                        class="fa-solid fa-star"></i><i class="fa-solid fa-star"></i><i
                                        class="fa-solid fa-star"></i>
                                </div>
                                <p class="fst-italic mb-4 text-dark lh-base" style="font-size: 1.2rem;">
                                    "Combo bắp nước giá hợp lý, phim mới cập nhật liên tục, hệ thống ghế ngồi êm ái –
                                    Tr0ondCinema là lựa chọn hàng đầu của mình khi đi xem phim cùng bạn bè."
                                </p>
                                <div class="d-flex align-items-center mt-4 pt-3"
                                    style="border-top: 1px solid rgba(0,0,0,0.1);">
                                    <img src="https://randomuser.me/api/portraits/men/65.jpg"
                                        class="rounded-circle me-3" width="60" height="60"
                                        style="object-fit: cover; border: 2px solid #E3CC81;">
                                    <div>
                                        <h5 class="mb-0 fw-bold" style="color: #191970;">Lê Văn C</h5>
                                        <small class="text-muted">Chủ cửa hàng DEF</small>
                                    </div>
                                </div>
                            </div>
                        </transition>

                    </div>
                </div>

                <div class="col-lg-5 ">
                    <div class="d-flex flex-column gap-3">
                        <div @click="changeTab(1)" class="d-flex align-items-center p-3 rounded"
                            :style="activeTab === 1 ? 'background-color: #191970; border: 1px solid #E3CC81; transform: translateX(-10px); cursor: pointer; transition: all 0.3s;' : 'background-color: rgba(255,255,255,0.1); border: 1px solid transparent; cursor: pointer; transition: all 0.3s;'">
                            <img src="https://randomuser.me/api/portraits/men/32.jpg" class="rounded-circle me-3"
                                width="50" height="50" style="object-fit: cover;">
                            <div class="flex-grow-1">
                                <h6 class="mb-0 fw-bold text-white">Nguyễn Văn A</h6>
                                <small class="text-white-50">CEO Công ty ABC</small>
                            </div>
                            <div v-if="activeTab === 1" class="text-warning"><i class="fa-solid fa-circle-check"></i>
                            </div>
                        </div>

                        <div @click="changeTab(2)" class="d-flex align-items-center p-3 rounded"
                            :style="activeTab === 2 ? 'background-color: #191970; border: 1px solid #E3CC81; transform: translateX(-10px); cursor: pointer; transition: all 0.3s;' : 'background-color: rgba(255,255,255,0.1); border: 1px solid transparent; cursor: pointer; transition: all 0.3s;'">
                            <img src="https://randomuser.me/api/portraits/women/44.jpg" class="rounded-circle me-3"
                                width="50" height="50" style="object-fit: cover;">
                            <div class="flex-grow-1">
                                <h6 class="mb-0 fw-bold text-white">Trần Thị B</h6>
                                <small class="text-white-50">Giám đốc XYZ Shop</small>
                            </div>
                            <div v-if="activeTab === 2" class="text-warning"><i class="fa-solid fa-circle-check"></i>
                            </div>
                        </div>

                        <div @click="changeTab(3)" class="d-flex align-items-center p-3 rounded"
                            :style="activeTab === 3 ? 'background-color: #191970; border: 1px solid #E3CC81; transform: translateX(-10px); cursor: pointer; transition: all 0.3s;' : 'background-color: rgba(255,255,255,0.1); border: 1px solid transparent; cursor: pointer; transition: all 0.3s;'">
                            <img src="https://randomuser.me/api/portraits/men/65.jpg" class="rounded-circle me-3"
                                width="50" height="50" style="object-fit: cover;">
                            <div class="flex-grow-1">
                                <h6 class="mb-0 fw-bold text-white">Lê Văn C</h6>
                                <small class="text-white-50">Chủ cửa hàng DEF</small>
                            </div>
                            <div v-if="activeTab === 3" class="text-warning"><i class="fa-solid fa-circle-check"></i>
                            </div>
                        </div>
                    </div>
                </div>

            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
import apiUrl from '../../../utils/api'

export default {
    data() {
        return {
            activeTab: 1,
            autoSlideInterval: null,
            slideDuration: 4000,
            progressWidth: 0,
            progressInterval: null,

            list_phim: [],
            list_bv: [],
            list_slide: [],

            list_phim_dropdown: [],
            list_ngay: [],
            list_suat: [],

            selected_phim: "",
            selected_ngay: "",
            selected_suat: "",
        }
    },
    mounted() {
        this.loadData();
        this.loadPhim();
        this.startAutoSlide(); // Chạy slide review
    },
    beforeUnmount() {
        this.stopAutoSlide(); // Hủy slide khi rời trang
    },
    methods: {
        loadData() {
            axios.get(apiUrl('client/trang-chu/get-data'))
                .then((res) => {
                    this.list_phim = res.data.data_phim;
                    this.list_bv = res.data.data_bv;
                    this.list_slide = res.data.data_slide || [];
                })
        },
        loadPhim() {
            axios.get(apiUrl('/client/trang-chu/get-phim'))
                .then((res) => {
                    this.list_phim_dropdown = res.data.data;
                })
                .catch((err) => {
                    console.error("Lỗi load phim:", err);
                });
        },
        loadNgayChieu() {
            this.selected_ngay = "";
            this.selected_suat = "";
            this.list_suat = [];
            this.list_ngay = [];
            if (!this.selected_phim) return;

            axios.post(apiUrl('/client/trang-chu/get-ngay-chieu'), {
                id_phim: this.selected_phim
            }).then((res) => {
                this.list_ngay = res.data.data;
            }).catch((err) => {
                console.error("Lỗi load ngày:", err);
            });
        },
        loadSuatChieu() {
            this.selected_suat = "";
            this.list_suat = [];
            if (!this.selected_ngay) return;

            axios.post(apiUrl('/client/trang-chu/get-suat-chieu'), {
                id_phim: this.selected_phim,
                ngay_chieu: this.selected_ngay
            }).then((res) => {
                this.list_suat = res.data.data;
            });
        },
        muaVeNgay() {
            if (this.selected_suat) {
                this.$router.push(`/client/dat-ve/${this.selected_suat}`);
            } else {
                alert("Vui lòng chọn đầy đủ thông tin!");
            }
        },
        formatDate(dateString) {
            if (!dateString) return '';
            const [year, month, day] = dateString.split('-');
            return `${day}/${month}/${year}`;
        },
        formatTime(timeString) {
            if (!timeString) return '';
            return timeString.substring(0, 5);
        },

        // --- Logic Auto Slide Review ---
        startAutoSlide() {
            this.progressWidth = 0;
            this.progressInterval = setInterval(() => {
                if (this.progressWidth >= 100) {
                    this.progressWidth = 0;
                } else {
                    this.progressWidth += (100 / (this.slideDuration / 100));
                }
            }, 100);

            this.autoSlideInterval = setInterval(() => {
                if (this.activeTab < 3) {
                    this.activeTab++;
                } else {
                    this.activeTab = 1;
                }
            }, this.slideDuration);
        },
        stopAutoSlide() {
            clearInterval(this.autoSlideInterval);
            clearInterval(this.progressInterval);
            this.progressWidth = 0;
        },
        changeTab(tabIndex) {
            this.activeTab = tabIndex;
            this.stopAutoSlide();
            this.startAutoSlide();
        }
    }
}
</script>

<style scoped>
/* Tiện ích chung */
.object-fit-cover {
    object-fit: cover;
}

.transition-transform {
    transition: transform 0.5s ease;
}

.transition-colors {
    transition: color 0.3s ease;
}

.line-clamp-2 {
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
    overflow: hidden;
}

/* 1. Hiệu ứng Service (Dịch Vụ) */
.service-card {
    transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

.service-card:hover {
    transform: translateY(-10px);
    border-color: #E3CC81 !important;
    box-shadow: 0 15px 30px rgba(0, 0, 0, 0.5), 0 0 15px rgba(227, 204, 129, 0.2);
}

.service-card .icon-box {
    transition: transform 0.4s ease, box-shadow 0.4s ease;
}

.service-card:hover .icon-box {
    transform: scale(1.15);
    box-shadow: 0 0 20px rgba(227, 204, 129, 0.6);
}

.service-card:hover .service-icon {
    animation: pulse-icon 1s infinite alternate;
}

@keyframes pulse-icon {
    0% {
        transform: scale(1);
    }

    100% {
        transform: scale(1.2);
    }
}

/* 2. Movie Cards (Phim Đang Chiếu) */
.now-showing-card img {
    filter: grayscale(20%);
}

.now-showing-card:hover img {
    transform: scale(1.1);
    filter: grayscale(0%);
}

.now-showing-card .info-overlay {
    position: absolute;
    inset: 0;
    background: linear-gradient(to top, rgba(0, 0, 0, 0.9) 0%, rgba(0, 0, 0, 0.3) 40%, transparent 100%);
    z-index: 1;
    transition: opacity 0.3s ease;
}

/* Giữ nguyên lớp overlay chữ bên dưới (không ẩn đi) khi hover */
.now-showing-card .hover-overlay {
    position: absolute;
    inset: 0;
    background: rgba(25, 25, 112, 0.85);
    /* #191970 mờ */
    z-index: 2;
    opacity: 0;
    transition: opacity 0.4s ease;
}

.now-showing-card:hover .hover-overlay {
    opacity: 1;
}

.hover-play-icon {
    transform: scale(0.5);
    transition: transform 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275), color 0.3s ease;
}

.now-showing-card:hover .hover-play-icon {
    transform: scale(1);
}

.hover-play-icon:hover {
    color: #E3CC81 !important;
    transform: scale(1.15) !important;
    filter: drop-shadow(0 0 10px rgba(227, 204, 129, 0.8));
}

.hover-btn-detail {
    transition: all 0.3s ease;
}

.hover-btn-detail:hover {
    background-color: transparent !important;
    color: #E3CC81 !important;
    border-color: #E3CC81 !important;
}

/* 3. Phim Sắp Chiếu Cards */
.coming-soon-overlay {
    position: absolute;
    inset: 0;
    background: linear-gradient(to top, rgba(0, 0, 0, 0.9) 0%, transparent 50%);
}

.coming-soon-card img {
    filter: brightness(0.8);
}

.coming-soon-card:hover img {
    transform: scale(1.05);
    filter: brightness(1);
}

/* 4. Bài Viết */
.article-card:hover .article-img {
    transform: scale(1.05);
}

.article-card:hover .article-title {
    color: #E3CC81 !important;
}

/* 5. Hiệu ứng chuyển slide đánh giá */
.fade-enter-active,
.fade-leave-active {
    transition: opacity 0.5s ease;
}

.fade-enter-from,
.fade-leave-to {
    opacity: 0;
}
</style>