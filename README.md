
<html lang="mn">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Big Five Personality Test</title>
<style>
    body {
        font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
        background: #fff8f4;
        color: #333;
        margin: 0;
        padding: 20px;
    }

    h1 {
        text-align: center;
        color: #ce2b7f;
        margin-bottom: 10px;
    }

    p.description {
        text-align: center;
        color: #555;
        margin-bottom: 30px;
    }

    form {
        max-width: 800px;
        margin: 0 auto;
    }

    .question {
        display: flex;
        align-items: center;
        justify-content: space-between;
        background-color: #fff;
        border: 1px solid #ffb3c6;
        border-radius: 10px;
        padding: 10px 15px;
        margin-bottom: 8px;
        box-shadow: 0 2px 5px rgba(255, 105, 180, 0.1);
        transition: transform 0.2s ease;
    }

    .question:hover {
        transform: scale(1.01);
        background-color: #fff3f7;
    }

    .question p {
        margin: 0;
        flex: 1;
        font-size: 15px;
        color: #444;
    }

    .answers {
        margin-left: 15px;
        flex-shrink: 0;
    }

    input[type="number"] {
        width: 60px;
        padding: 5px;
        font-size: 15px;
        text-align: center;
        border: 1px solid #ffb3c6;
        border-radius: 8px;
        outline: none;
        transition: 0.2s;
    }

    input[type="number"]:focus {
        border-color: #e74c8b;
        box-shadow: 0 0 5px rgba(231, 76, 139, 0.5);
    }

    button {
        display: block;
        margin: 25px auto;
        padding: 12px 25px;
        background-color: #e74c8b;
        color: white;
        border: none;
        border-radius: 25px;
        font-size: 16px;
        cursor: pointer;
        transition: 0.3s;
    }

    button:hover {
        background-color: #d63384;
    }

    #result {
        max-width: 800px;
        margin: 30px auto;
        padding: 20px;
        border: 2px dashed #ffd1aa;
        border-radius: 10px;
        background-color: #fffaf3;
        display: none;
        animation: fadeIn 0.4s ease-in;
    }

    @keyframes fadeIn {
        from { opacity: 0; transform: translateY(5px); }
        to { opacity: 1; transform: translateY(0); }
    }

    #result h2 {
        text-align: center;
        color: #ce2b7f;
        margin-bottom: 15px;
    }

    #result p {
        margin-bottom: 10px;
        color: #444;
        line-height: 1.6;
    }

    @media screen and (max-width: 600px) {
        .question {
            flex-direction: column;
            align-items: flex-start;
        }
        .answers {
            margin-left: 0;
            margin-top: 5px;
        }
    }
</style>
</head>

<body>
<h1>Big Five Personality Test</h1>
<p class="description">Доорх 50 асуултад 1–5 оноогоор хариулна уу.<br>
1 = санал нийлэхгүй &nbsp;&nbsp;|&nbsp;&nbsp; 3 = төвийг сахисан &nbsp;&nbsp;|&nbsp;&nbsp; 5 = санал бүрэн нийлнэ</p>

<form id="testForm">
    <div id="questions"></div>
    <button type="submit">Үр дүн харах</button>
</form>

<div id="result"></div>

<script>
const questions = [
"Би бол үдэшлэгийн гол хүн.", "Би бусдын төлөө санаа зовох нь бага.",
"Би үргэлж бэлтгэлтэй байдаг.", "Би стресст амархан өртдөг.",
"Би үгийн баялаг сайтай.", "Би тийм ч их ярьдаггүй.",
"Би хүмүүсийн талаар сонирхдог.", "Би өөрийнхөө эд зүйлсэд харам.",
"Би ихэнхдээ амар тайван байдаг.", "Би хийсвэр ухагдахууныг ойлгохдоо тааруу.",
"Би хүмүүсийн дунд чөлөөтэй байж чаддаг.", "Би хүмүүсийг доромжилдог.",
"Би нарийн зүйлсэд анхаарал хандуулдаг.", "Би аливаа зүйлсэд санаа зовдог.",
"Би уран сэтгэмж сайтай.", "Би анхаарлын төвд байдаггүй.",
"Би бусдын мэдрэмжийг мэдэрч чаддаг.", "Би аливаа зүйлийг замбараагүй болгодог.",
"Би сэтгэлээр унах нь ховорхон.", "Би хийсвэр санааг сонирхдоггүй.",
"Би харилцан яриаг эхлүүлдэг.", "Би бусдын асуудлыг сонирхдоггүй.",
"Би аливаа ажлыг цаг алдалгүй дуусгаж чаддаг.", "Би амархан сатаардаг.",
"Би гайхалтай шинэ зүйлсийг санаачилдаг.", "Би хэлэх зүйл багатай байдаг.",
"Би зөөлөн сэтгэлтэй.", "Би аливаа зүйлийг зохих байранд нь буцааж тавихаа мартдаг.",
"Би амархан бухимддаг.", "Би тийм ч сайн уран төсөөлөлтэй биш.",
"Би үдэшлэгт маш олон хүмүүстэй ярилцдаг.", "Би бусдын талаар үнэхээр сонирхдоггүй.",
"Би хэв журамд дуртай.", "Би ааш зангаа өөрчлөх нь олон.",
"Би аливааг хурдан ойлгодог.", "Би өөртөө бусдын анхаарлыг татах дүргүй.",
"Би бусдад цаг гаргадаг.", "Би үүрэг хариуцлагаас зайлсхийдэг.",
"Би байнга сэтгэл санааны тогтворгүй байдалд байдаг.", "Би хүнд үгнүүд ашигладаг.",
"Би олны анхаарлын төвд байхаас санаа зовдоггүй.", "Би бусдын сэтгэл хөдлөлийг мэдэрдэг.",
"Би аливаа хуваарийг дагаж мөрддөг.", "Би амархан уурладаг.",
"Би аливаа зүйлийг тунгаан бодоход цаг зарцуулдаг.", "Би танихгүй хүмүүсийн дэргэд чимээгүй байдаг.",
"Би хүмүүсийг тайвшруулдаг.", "Би ажилдаа шаардлага тавьдаг.",
"Би ихэнх тохиолдолд сэтгэлээр унадаг.", "Би шинэ санаагаар дүүрэн."
];

const container = document.getElementById("questions");
questions.forEach((q, i) => {
    const div = document.createElement("div");
    div.className = "question";
    div.innerHTML = `
        <p><b>${i + 1}.</b> ${q}</p>
        <div class="answers">
            <input type="number" name="q${i + 1}" min="1" max="5" required placeholder="1–5">
        </div>`;
    container.appendChild(div);
});

document.getElementById("testForm").addEventListener("submit", function (event) {
    event.preventDefault();
    const data = new FormData(event.target);
    const s = {};
    data.forEach((v, k) => s[k] = parseInt(v));

    const O = 8 + s.q5 - s.q10 + s.q15 - s.q20 + s.q25 - s.q30 + s.q35 + s.q40 + s.q45 + s.q50;
    const C = 14 + s.q3 - s.q8 + s.q13 - s.q18 + s.q23 - s.q28 + s.q33 - s.q38 + s.q43 + s.q48;
    const E = 20 + s.q1 - s.q6 + s.q11 - s.q16 + s.q21 - s.q26 + s.q31 - s.q36 + s.q41 - s.q46;
    const A = 14 - s.q2 + s.q7 - s.q12 + s.q17 - s.q22 + s.q27 - s.q32 + s.q37 + s.q42 + s.q47;
    const N = 38 - s.q4 + s.q9 - s.q14 + s.q19 - s.q24 - s.q29 - s.q34 - s.q39 - s.q44 - s.q49;

    const desc = {
        O: "Нээлттэй байдал (Openness): Шинэ санаа, бүтээлч байдал, уран сэтгэмжийн илрэл.",
        C: "Хариуцлага (Conscientiousness): Зохион байгуулалт, хариуцлага, төлөвлөгөөтэй байдал.",
        E: "Гадагш чиглэсэн байдал (Extraversion): Нийгмийн идэвх, нийтэч зан.",
        A: "Хүлцэнгүй байдал (Agreeableness): Найрсаг, бусдад тусархаг, эвсэг байдал.",
        N: "Сэтгэл тогтворгүй байдал (Neuroticism): Сэтгэл хөдлөл, эмзэг, тогтворгүй зан чанар."
    };

    const resultDiv = document.getElementById("result");
    resultDiv.style.display = "block";
    resultDiv.innerHTML = `
        <h2>Таны тестийн үр дүн</h2>
        <p><b>O:</b> ${O} — ${desc.O}</p>
        <p><b>C:</b> ${C} — ${desc.C}</p>
        <p><b>E:</b> ${E} — ${desc.E}</p>
        <p><b>A:</b> ${A} — ${desc.A}</p>
        <p><b>N:</b> ${N} — ${desc.N}</p>
    `;
});
</script>
</body>
</html>
