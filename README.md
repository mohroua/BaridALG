<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8">
  <title>اختبار بريد الجزائر - الجانب المالي والإداري</title>
  <style>
    body {
      font-family: 'arial', sans-serif;
      background-color: #fff;
      margin: 0;
      padding: 0;
      direction: rtl;
    }
    .header {
      text-align: right;
      margin-bottom: 30px;
    }
    .header img {
      width: 900px;
      height: auto;
      display: block;
      margin: 0 auto;
      border-radius: 30px;
    }
    .title {
      font-size: 70px;
      font-weight: bold;
      margin: 50px auto;
      text-align: center;
      width: fit-content;
      color: #004a99;
    }
    .container {
      padding: 20px;
    }
    .question {
      background-color: #fff;
      padding: 15px;
      margin-bottom: 15px;
      border: 5px solid #004a99;
      border-right: 20px solid #ffc107;
      border-radius: 10px;
      box-shadow: 2px 2px 8px rgba(0, 0, 0, 0.1);
    }
    .question p {
      margin: 0 0 10px;
      font-weight: bold;
      color: #004a99;
    }
    .question label {
      display: block;
      margin: 4px 0;
      cursor: pointer;
    }
    h2 {
      color: #FF0000;
    }
    button {
      padding: 10px 20px;
      margin: 20px;
      font-size: 18px;
      background-color: #004a99;
      color: white;
      border: none;
      border-radius: 8px;
      cursor: pointer;
    }
    .result {
      font-size: 24px;
      margin: 10px;
    }
  </style>
</head>
<body>

  <div class="header">
    <img src="https://i.postimg.cc/8ChN66JP/Merged-Images.png" alt="شعار بريد الجزائر">
    <h1 class="title">اختبار بريد الجزائر الرقمي</h1>
    <h2>ملاحظة: هذا عبارة عن إختبار تجريبي</h2>
  </div>

  <!-- الأسئلة -->
  <div id="quiz"></div>
  <button onclick="showResult()">عرض النتيجة</button>
  <div class="result" id="result"></div>

  <h2>حاسبة نتيجة اختبار QSM</h2>
  <input type="number" id="total" placeholder="عدد الأسئلة الكلي">
  <input type="number" id="correct" placeholder="عدد الإجابات الصحيحة">
  <br>
  <button onclick="calculateQSM()">احسب النتيجة</button>
  <div class="result" id="output"></div>

  <!-- الكود البرمجي -->
  <script>
    const questions = [];

    function addQuestion(q, options, correct) {
      questions.push({ q, options, correct });
    }

    // إضافة الأسئلة هنا (اختصرت فقط بعضها كمثال)
    
    addQuestion("ما هي الوثائق المطلوبة لفتح حساب بريدي جاري؟", ["نسخة من شهادة الميلاد", "الوثيقة CH1 + نسخة من بطاقة الهوية", "بطاقة إقامة", "شهادة عمل"], 1);
    addQuestion("ما هو الحد الأقصى اليومي للتحويل من حساب إلى حساب عبر الصراف؟", ["20,000 دج", "30,000 دج", "50,000 دج", "100,000 دج"], 2);
    addQuestion("ما هي الخدمة التي تسمح بالسحب بدون بطاقة؟", ["Hawalatic", "Cardless", "Flexy", "Edahabia"], 1);
    addQuestion("في أي سنة تم إطلاق البطاقة الذهبية؟", ["2014", "2016", "2017", "2019"], 1);
    addQuestion("ما هو الرقم الأخضر لبريد الجزائر؟", ["1530", "1055", "1500", "1020"], 0);
    addQuestion("ما نوع الحساب الذي يقدّمه بريد الجزائر لتلقي الأجور؟", ["حساب تجاري", "حساب توفير", "حساب بريدي جاري (CCP)", "حساب مشترك"], 2);
    addQuestion("كم هو الحد الأدنى للرصيد اللازم لفتح حساب CCP؟", ["0 دج", "1000 دج", "500 دج", "100 دج"], 0);
    addQuestion("ما هو الرمز الذي يُستخدم لتحويل الأموال إلى حساب CCP؟", ["RIB", "RIP", "IBAN", "SWIFT"], 1);
    addQuestion("ما هي الرسوم السنوية للبطاقة الذهبية؟", ["مجانية", "350 دج", "500 دج", "1000 دج"], 1);
    addQuestion("ما هي مدة صلاحية البطاقة الذهبية؟", ["سنة واحدة", "سنتان", "ثلاث سنوات", "خمس سنوات"], 2);
    addQuestion("أي من هذه الخدمات يمكن إجراؤها عبر البطاقة الذهبية؟", ["دفع الفواتير", "السحب من الصراف", "شراء عبر الإنترنت", "جميع ما سبق"], 3);
    addQuestion("ما هي الجهة المخوّلة بإصدار دفتر الشيكات البريدي؟", ["وزارة المالية", "البنك المركزي", "بريد الجزائر", "الخزينة العمومية"], 2);
    addQuestion("كيف يتم تفعيل البطاقة الذهبية بعد استلامها؟", ["عبر الهاتف", "عبر تطبيق BaridiMob", "من مكتب البريد", "عبر الصراف الآلي"], 3);
    addQuestion("ما هو المبلغ الأقصى للسحب اليومي من البطاقة الذهبية؟", ["20,000 دج", "30,000 دج", "50,000 دج", "لا يوجد حد"], 2);
    addQuestion("كيف يُمكن للزبون إيقاف البطاقة الذهبية في حالة الضياع؟", ["عبر إرسال بريد إلكتروني", "عبر الاتصال بالرقم 1530", "بزيارة الموقع", "لا يمكن ذلك"], 1)                                                                    // الأسئلة (1–55)
    addQuestion("ما هو البريد؟", ["نقل الرسائل", "نقل الأموال", "نقل الأشخاص", "كل ما سبق"], 0);
    addQuestion("متى تأسس بريد الجزائر؟", ["1962", "1975", "1990", "2000"], 0);
    addQuestion("ما هي الخدمات التي يقدمها بريد الجزائر؟", ["خدمات البريد فقط", "خدمات مالية فقط", "خدمات بريدية ومالية", "خدمات سياحية"], 2);
    addQuestion("ما هو رقم الطوارئ في بريد الجزائر؟", ["15", "19", "14", "17"], 1);
    addQuestion("ما هو البريد السريع؟", ["خدمة توصيل الرسائل خلال يوم", "خدمة توصيل الطرود الدولية", "خدمة توصيل الطرود بسرعة عالية", "خدمة إرسال الرسائل الإلكترونية"], 2);
    addQuestion("أي من التالي هو نوع من الخدمات المالية في بريد الجزائر؟", ["القروض", "الحسابات البريدية", "التأمين", "الاستثمار العقاري"], 1);
    addQuestion("ما هي العملات التي يمكن استخدامها في البريد؟", ["الدينار الجزائري فقط", "الدولار فقط", "الدينار واليورو", "أي عملة"], 2);
    addQuestion("ما هو الهدف من خدمة الحساب البريدي الجاري؟", ["حفظ الأموال", "السحب والإيداع فقط", "إجراء المعاملات المالية", "نقل الأموال فقط"], 2);
    addQuestion("أي من هذه الخدمات متعلقة بالطرود؟", ["إرسال الرسائل", "تتبع الطرود", "خدمة الهاتف", "الإنترنت"], 1);
    addQuestion("ما هو البريد الإلكتروني؟", ["رسالة إلكترونية", "خدمة الطرود", "خدمة البريد السريع", "نوع من الطرود"], 0);
    addQuestion("كيف يمكن للزبون تتبع طرده؟", ["عن طريق رقم التتبع", "عن طريق الهاتف فقط", "عن طريق البريد الإلكتروني", "لا يمكن تتبع الطرد"], 0);
    addQuestion("ما هو عمل موظف البريد؟", ["نقل الرسائل فقط", "تقديم الخدمات المالية فقط", "تقديم الخدمات البريدية والمالية", "خدمة العملاء فقط"], 2);
    addQuestion("ما هي الوثائق المطلوبة لفتح حساب بريدي؟", ["بطاقة التعريف فقط", "شهادة ميلاد فقط", "بطاقة التعريف وشهادة الميلاد", "لا حاجة لوثائق"], 0);
    addQuestion("ما هو البريد المالي؟", ["خدمة تقديم القروض", "خدمة التحويلات المالية", "خدمة التوظيف", "خدمة الطرود"], 1);
    addQuestion("ما هي ساعات عمل بريد الجزائر؟", ["من 8 صباحا إلى 4 مساء", "من 9 صباحا إلى 5 مساء", "24 ساعة", "من 7 صباحا إلى 3 مساء"], 0);
    addQuestion("من يحدد سياسة الأسعار في بريد الجزائر؟", ["الدولة", "المدير العام", "الوزارة الوصية", "مجلس الإدارة"], 2);
    addQuestion("ما هي اللغة الرسمية للمراسلات الإدارية؟", ["العربية", "الفرنسية", "الإنجليزية", "كل ما سبق"], 0);
    addQuestion("أي من هذه التصرفات يُعد مخالفًا في الإدارة؟", ["العمل في الوقت المحدد", "تقديم الخدمات للزبائن", "استغلال المنصب لمصالح شخصية", "إبلاغ الرؤساء عن الأخطاء"], 2);
    addQuestion("ما هو السلم الإداري الصحيح للتظلم داخل المؤسسة؟", ["الإدارة المباشرة ثم المفتشية", "المفتشية ثم القضاء", "الزملاء ثم الإدارة", "الإعلام فقط"], 0);
    addQuestion("ما معنى التوقيت الإداري؟", ["توقيت دخول الزبائن", "توقيت عمل الموظفين", "توقيت تسليم الطرود", "توقيت البريد السريع"], 1);
    addQuestion("ما هو اللباس المهني المطلوب في بريد الجزائر؟", ["غير مهم", "رسمي ومحترم", "حسب الطقس", "رياضي"], 1);
    addQuestion("ما المقصود بالهيكل التنظيمي؟", ["ترتيب المكاتب", "خريطة توضح تسلسل المسؤوليات", "عدد الموظفين", "قائمة العطل"], 1);
    addQuestion("من هو المسؤول عن الانضباط داخل المؤسسة؟", ["المدير", "الموظف نفسه", "المفتش", "كل ما سبق"], 3);
    addQuestion("ما هي الجهة التي تراقب جودة الخدمات؟", ["مفتشية العمل", "المديرية العامة", "مكتب الشكاوي", "كل ما سبق"], 3);
    addQuestion("ما هو التصرف المناسب عند تأخر الزبون؟", ["تجاهله", "مساعدته بسرعة واحترام", "مجادلته", "رفض خدمته"], 1);
    addQuestion("ما هو السلوك المطلوب من الموظف تجاه كبار السن؟", ["السرعة فقط", "الاهتمام والاحترام", "طلب مساعدة آخرين", "تجاهل"], 1);
    addQuestion("من المسؤول عن تسجيل العطل السنوية؟", ["الموظف", "مصلحة المستخدمين", "المدير فقط", "لا أحد"], 1);
    addQuestion("ما هي الوثيقة التي تُقدّم عند المرض؟", ["عذر شفهي", "شهادة طبية", "بطاقة مريض", "وصفة دواء"], 1);
    addQuestion("ما هو الهدف من التكوين داخل المؤسسة؟", ["التسلية", "رفع الأداء والكفاءة", "مراقبة الموظفين", "تحديد العقوبات"], 1);
    addQuestion("من يتخذ القرار في نقل الموظف؟", ["رئيس المصلحة", "الموظف نفسه", "المديرية العامة", "الزملاء"], 2);
    addQuestion("ما هو التصرف الصحيح عند حدوث خطأ في العمل؟", ["إخفاؤه", "إبلاغ المسؤول فوراً", "إلقاء اللوم على الزملاء", "ترك العمل"], 1);
    addQuestion("من يقيّم أداء الموظف؟", ["زميله", "الزبائن", "المسؤول المباشر", "المدير العام"], 2);
    addQuestion("ما المقصود بالأمانة المهنية؟", ["احترام الزملاء", "المحافظة على ممتلكات المؤسسة والسرية", "اللباس الرسمي", "الرد على المكالمات"], 1);
    addQuestion("متى تُعتبر الغيابات غير مبررة؟", ["عند تقديم شهادة طبية", "بعد العطلة السنوية", "بدون تبرير أو وثيقة رسمية", "عند الوصول متأخراً"], 2);
    addQuestion("ما هو السلوك الإداري غير المقبول؟", ["العمل بروح الفريق", "استقبال الزبائن بأدب", "الرد بعنف على الزبائن", "الالتزام بالمهام"], 2);
    addQuestion("ما المقصود بالاتصال الإداري؟", ["مكالمات شخصية", "مراسلات بين الأعوان", "نقل المعلومات داخل المؤسسة", "البريد الإلكتروني فقط"], 2);
    addQuestion("أي من التالي يُعد وسيلة تواصل داخلية؟", ["الصحف الوطنية", "الإذاعة", "المذكرة الإدارية", "الإعلانات التجارية"], 2);
    addQuestion("كيف يُحسن الموظف علاقته مع زملائه؟", ["التعاون والاحترام", "العمل الفردي فقط", "الغيرة والمنافسة", "التحدث كثيرًا"], 0);
    addQuestion("ما هي العقوبة الإدارية الأخف؟", ["الطرد النهائي", "التوبيخ", "الخصم من الأجر", "التوقيف المؤقت"], 1);
    addQuestion("كيف يمكن تطوير الكفاءة المهنية؟", ["التكوين المستمر", "الراحة فقط", "العمل الروتيني", "تجاهل التعليمات"], 0);
    
    const quizDiv = document.getElementById("quiz");
    questions.forEach((q, i) => {
      const div = document.createElement("div");
      div.className = "question";
      div.innerHTML = `<h3>س${i + 1}: ${q.q}</h3>` + q.options.map((opt, j) => `
        <label>
          <input type="radio" name="q${i}" value="${j}"> ${opt}
        </label>`).join("");
      quizDiv.appendChild(div);
    });

    function showResult() {
      let score = 0;
      questions.forEach((q, i) => {
        const selected = document.querySelector(`input[name="q${i}"]:checked`);
        const options = document.getElementsByName(`q${i}`);

        options.forEach(opt => {
          opt.parentElement.style.backgroundColor = "";
          opt.parentElement.style.fontWeight = "normal";
        });

        if (selected) {
          if (parseInt(selected.value) === q.correct) {
            score++;
          }
        }

        options[q.correct].parentElement.style.backgroundColor = "#d4edda";
        options[q.correct].parentElement.style.fontWeight = "bold";

        if (selected && parseInt(selected.value) !== q.correct) {
          selected.parentElement.style.backgroundColor = "#f8d7da";
          selected.parentElement.style.fontWeight = "bold";
        }
      });

      document.getElementById("result").innerText = `✔️ نتيجتك: ${score} من ${questions.length}`;
    }

    function calculateQSM() {
      const total = parseInt(document.getElementById('total').value);
      const correct = parseInt(document.getElementById('correct').value);
      const wrong = total - correct;
      const score = correct - wrong;
      const percent = ((Math.max(score, 0) / total) * 100).toFixed(2);

      let message = '';
      if (score < 0) {
        message = `❌ النتيجة سالبة (${score})، تم اعتبارها 0.`;
      } else {
        message = `✅ نتيجتك: ${score} من ${total} (أي بنسبة ${percent}%)`;
      }

      document.getElementById('output').innerText = message;
    }
  </script>
</body>
</html>
