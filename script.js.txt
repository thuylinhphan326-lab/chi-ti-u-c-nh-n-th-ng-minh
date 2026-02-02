function formatMoney(num) {
    return num.toLocaleString("vi-VN") + " VNĐ";
}

function cleanNumber(num) {
    return Math.floor(num / 1000) * 1000; // luôn làm tròn xuống ngàn
}

function generatePlan() {
    let income = Number(document.getElementById("income").value);
    let age = Number(document.getElementById("age").value);
    let job = document.getElementById("job").value;

    if (!income || !age) {
        alert("Vui lòng nhập đầy đủ thông tin!");
        return;
    }

    income = cleanNumber(income);

    let categories;

    // 🎯 CHIA THEO ĐỐI TƯỢNG
    if (job === "student") {
        categories = {
            "Học tập – đầu tư bản thân": 0.25,
            "Tiết kiệm cho tương lai": 0.15,
            "Ăn uống – đi chơi Tết": 0.18,
            "Trang phục – mua sắm": 0.15,
            "Quà biếu – thăm hỏi": 0.12,
            "Lì xì & giao lưu bạn bè": 0.10,
            "Quỹ dự phòng": 0.05
        };

    } else if (job === "worker") {
        categories = {
            "Sinh hoạt gia đình & Tết": 0.30,
            "Tiết kiệm – dự phòng": 0.20,
            "Quà biếu – thăm hỏi": 0.18,
            "Lì xì": 0.15,
            "Mua sắm cá nhân": 0.10,
            "Giao lưu – bạn bè": 0.05,
            "Phát sinh khác": 0.02
        };

    } else {
        // người cao tuổi
        categories = {
            "Sức khỏe – thuốc men": 0.35,
            "Tiết kiệm – dự phòng": 0.25,
            "Gia đình – con cháu": 0.18,
            "Hội họp – thăm hỏi": 0.12,
            "Ăn uống – vui chơi nhẹ": 0.07,
            "Khác": 0.03
        };
    }

    let html = `<div class="card"><h3>Tổng thu nhập: ${formatMoney(income)}</h3></div>`;

    // 🎯 TẠO DANH SÁCH CHI TIÊU
    for (let [name, percent] of Object.entries(categories)) {
        let amount = cleanNumber(income * percent);

        html += `
            <div class="card">
                <h3>${name} — <b>${formatMoney(amount)}</b></h3>
                <ul>${createDetails(name, amount).join("")}</ul>
            </div>
        `;
    }

    document.getElementById("plan").innerHTML = html;
}

function createDetails(category, amount) {

    let list = [];

    if (category === "Quà biếu – thăm hỏi") {
        let items = {
            "Biếu bố mẹ": 0.35,
            "Biếu ông bà": 0.28,
            "Biếu họ hàng – thầy cô": 0.22,
            "Thăm hỏi hàng xóm – bạn bè": 0.15
        };

        for (let [name, r] of Object.entries(items)) {
            list.push(`<li>${name}: <b>${formatMoney(cleanNumber(amount * r))}</b></li>`);
        }
        return list;
    }

    if (category === "Lì xì" || category === "Lì xì & giao lưu bạn bè") {
        let items = {
            "Trẻ nhỏ": 0.35,
            "Anh chị em": 0.25,
            "Bố mẹ": 0.20,
            "Bạn bè – giao lưu": 0.15,
            "Phát sinh": 0.05
        };

        for (let [name, r] of Object.entries(items)) {
            list.push(`<li>${name}: <b>${formatMoney(cleanNumber(amount * r))}</b></li>`);
        }
        return list;
    }

    // Mặc định
    let fallback = {
        "Khoản chính": 0.45,
        "Khoản phụ 1": 0.30,
        "Khoản phụ 2": 0.25
    };

    for (let [name, r] of Object.entries(fallback)) {
        list.push(`<li>${name}: <b>${formatMoney(cleanNumber(amount * r))}</b></li>`);
    }

    return list;
}
