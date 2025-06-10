<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>مركز الامتحان - بريد الجزائر</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background-color: #f9f9f9;
      margin: 0;
      padding: 0;
    }

    .header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 5px 10px; /* تقليل البادينغ */
      background-color: #fff;
      border-bottom: 1px solid #ccc;
    }

    .header-content {
      display: flex;
      align-items: center;
      gap: 5px; /* تقليل المسافة بين الشعار والنص */
    }

    .header-content span {
      font-size: 18px; /* تصغير الخط */
      font-weight: bold;
    }

    .header-content img {
      height: 60px; /* تصغير حجم الشعار */
    }

    .content {
      text-align: center;
      padding: 15px; /* تقليل المسافة حول المحتوى */
    }

    .box {
      background-color: #fff;
      border: 1px solid #ddd;
      border-radius: 10px;
      padding: 15px; /* تقليل الفراغ داخل المربع */
      display: inline-block;
      box-shadow: 0 0 5px rgba(0,0,0,0.1);
      width: 70%;
      max-width: 800px;
      min-width: 300px;
      margin: 0 auto;
    }

    .status {
      display: flex;
      justify-content: center;
      align-items: center;
      margin: 5px 0; /* تقليل الهامش العمودي */
    }

    .status span {
      background-color: #f0f0f0;
      border-radius: 10px;
      padding: 3px 8px; /* تقليل الحجم الداخلي */
      margin-right: 5px;
      font-size: 14px;
    }

    .status .orange {
      background-color: orange;
      color: white;
    }

    button {
      padding: 8px 16px;
      margin-top: 10px;
      font-size: 16px;
      border: none;
      border-radius: 8px;
      background-color: #ddd;
      cursor: pointer;
    }

    #exam-section {
      display: none;
      padding: 20px; /* تقليل المسافة */
    }

    .question {
      font-size: 28px; /* تقليل حجم السؤال */
      font-weight: bold;
      margin-bottom: 10px;
    }

    label {
      display: block;
      margin: 5px 0; /* تقليل الفراغ بين الخيارات */
      font-size: 15px;
    }

    .hidden {
      display: none;
    }

    #timer {
      font-weight: bold;
      color: red;
      margin: 5px 0; /* تقليل المسافة حول المؤقت */
    }

    #nav-buttons {
      margin-top: 10px;
    }

    .correct {
      color: green;
      font-weight: bold;
    }
    .wrong {
      color: red;
      font-weight: bold;
    }
    .question-block {
      margin-bottom: 20px;
    }

    /* الأنماط لرسالة الإقصاء */
    #disqualification-container {
      background-color: #ffe6e6; /* خلفية حمراء فاتحة */
      padding: 30px;
      border-radius: 8px;
      box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
      max-width: 600px;
      width: 90%;
      border: 2px solid #ff4d4d; /* حدود حمراء */
      text-align: center;
      margin: 50px auto; /* لتوسيط الرسالة */
    }

    #disqualification-text {
      color: #ff0000; /* نص أحمر للإقصاء */
      font-size: 2em;
      font-weight: bold;
      margin-bottom: 20px;
    }

    #retry-button {
      background-color: #4CAF50; /* خلفية خضراء */
      color: white;
      padding: 15px 30px;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      font-size: 1.2em;
      transition: background-color 0.3s ease;
    }

    #retry-button:hover {
      background-color: #45a049; /* أخضر أغمق عند التحويم */
    }
  </style>

</head>

<body>

  <div class="header">
    <div class="header-content">
      <img src="https://i.postimg.cc/qv9RbZRT/images-15.jpg" alt="Logo">
      <span>مركز الامتحان</span>
    </div>
  </div>


  <div class="content" id="welcome-section">
    <h2 id="username">مرحبا المترشح</h2>
    <div class="box">
      <h3>مكلف بالزبائن</h3>
      <div class="status">
        <span>55 الأسئلة</span>
        <span class="orange">لم يبدأ بعد</span>
      </div>
      <p>امتحان مكلف بالزبائن</p>
      <button onclick="startExam()">بدء الامتحان</button>
    </div>
  </div>

  <div id="exam-section" class="hidden">
    <div class="question" id="question-text"></div>
    <div id="timer"></div>
    <div id="choices"></div>
    <div id="nav-buttons">
      <button id="prevBtn" onclick="prevQuestion()">السابق</button>
      <button onclick="nextQuestion()">التالي</button>
    </div>
    <div id="result" class="hidden"></div>
  </div>

  <div id="correction-section" class="hidden"></div>

  <script>
    // 👇 اختر الوضع هنا: "training" أو "official"
    const mode = "training"; // قم بتغيير هذا إلى "official" عند الحاجة
    let examStarted = false; // متغير لتتبع حالة بدء الامتحان

    document.addEventListener("DOMContentLoaded", function () {
        const welcomeSection = document.getElementById("welcome-section");
        const examSection = document.getElementById("exam-section");

        // دالة لعرض رسالة الإقصاء بناءً على الوضع
        function showDisqualificationMessage() {
            // إخفاء كل أقسام الصفحة
            welcomeSection.classList.add("hidden");
            examSection.classList.add("hidden");

            // مسح المحتوى الحالي لـ body بالكامل لضمان عدم ظهور أي شيء آخر
            document.body.innerHTML = '';

            const div = document.createElement('div');
            div.id = 'disqualification-container';
            div.className = 'disqualification-container';

            if (mode === "training") {
                div.innerHTML = `
                    <p id="disqualification-text">لقد تم إقصاؤك من الامتحان!</p>
                    <p>هذا اختبار تجريبي، يمكنك إعادة المحاولة.</p>
                    <button id="retry-button">إعادة المحاولة</button>
                `;
                document.body.appendChild(div);
                document.getElementById("retry-button").addEventListener("click", function () {
                    // مسح علامة الإقصاء والسماح بإعادة المحاولة
                    localStorage.removeItem("disqualified");
                    location.reload(); // إعادة تحميل الصفحة
                });
            } else if (mode === "official") {
                div.innerHTML = `
                    <p id="disqualification-text">لقد تم إقصاؤك من الامتحان!</p>
                    <p>لا يمكنك إعادة الدخول مرة أخرى.</p>
                `;
                document.body.appendChild(div);
            }
        }

        // التحقق عند تحميل الصفحة إذا كان المستخدم قد تم إقصاؤه مسبقًا
        if (localStorage.getItem("disqualified") === "true") {
            showDisqualificationMessage();
        } else {
            // إظهار المحتوى الأصلي للصفحة إذا لم يتم الإقصاء
            welcomeSection.classList.remove("hidden");
            examSection.classList.add("hidden"); // تأكد من إخفاء قسم الامتحان في البداية
        }

        // معالج الحدث عند محاولة المستخدم مغادرة الصفحة أو تحديثها
        window.addEventListener("beforeunload", function (event) {
            // إذا كان الامتحان قد بدأ ولم ينتهِ بعد (أي، لم تظهر صفحة النتائج)
            // هذا هو الشرط الذي يحدد ما إذا كان يجب تسجيل الإقصاء
            if (examStarted && document.getElementById("result").classList.contains("hidden")) {
                 localStorage.setItem("disqualified", "true");
            }
            // تم إزالة event.preventDefault() و event.returnValue = ''; لعدم إظهار الرسالة
        });

        // التقاط الاسم من الرابط
        function getNameFromURL() {
            const params = new URLSearchParams(window.location.search);
            return params.get("name") || "المترشح";
        }

        // عرض الاسم
        const name = getNameFromURL();
        document.getElementById("username").textContent = `مرحبا ${name.replace(/\./g, ' ')}`;


        // بدء الامتحان (تم جعلها دالة عامة ليمكن استدعاؤها من HTML)
        window.startExam = function() {
            examStarted = true; // تعيين حالة بدء الامتحان إلى صحيح
            welcomeSection.style.display = "none";
            examSection.style.display = "block";
            showQuestion();
        };

        const questions = [];

        function addQuestion(q, options, correct) {
            questions.push({ q, choices: options, correct });
        }

        // أسئلتك هنا

   // أسئلتك هنا
addQuestion("أي من الرموز التالية يُستعمل تحديدًا في تنفيذ تحويل مالي نحو حساب بريد جاري (CCP)؟", ["RIB", "RIP", "IBAN", "SWIFT"], 1);
addQuestion("بعد الاستقلال وإعادة هيكلة قطاع البريد، أي سنة تُعتمد رسميًا كإنشاء لبريد الجزائر؟", ["1962", "1990", "2000", "2002"], 3);
addQuestion("كم هو الحد الأقصى المسموح به للسحب من جهاز الصراف الآلي في عملية واحدة؟", ["10,000 دج", "20,000 دج", "50,000 دج", "100,000 دج"], 2);
addQuestion("في حال تم فقدان البطاقة الذهبية، ما هو أول إجراء واجب اتخاذه؟", ["الاتصال بالشرطة", "مراسلة البنك", "الاتصال بخدمة زبائن بريد الجزائر", "التوجه للضرائب"], 2);
addQuestion("ما هي الوثيقة القانونية الأساسية اللازمة لسحب مبلغ مالي من حساب بريدي؟", ["رخصة السياقة", "بطاقة تعريف وطنية", "جواز سفر بيومتري", "دفتر العائلة"], 1);
addQuestion("وثيقة 'SFPO1' تُستعمل في إطار أي من الخدمات التالية؟", ["تفعيل الحساب", "طلب بطاقة ذهبية", "تقديم طعن", "تنفيذ عمليات مالية من سحب أو تحويل"], 3);
addQuestion("إذا تم إدخال الرمز السري للبطاقة ثلاث مرات بشكل خاطئ، ما هو الإجراء التلقائي؟", ["فتح البطاقة", "إلغاء الحساب", "حجز البطاقة مؤقتًا", "إعادة تهيئة البطاقة"], 2);
addQuestion("بخصوص تتبع الطرود، أي من الطرق التالية تتيح معرفة موقع الطرد بدقة؟", ["الاتصال الهاتفي", "استعمال الموقع الإلكتروني لبريد الجزائر", "التتبع غير ممكن", "التوجه لمركز الفرز"], 1);
addQuestion("ما هي مدة الصلاحية الرسمية للبطاقة الذهبية؟", ["12 شهرًا", "24 شهرًا", "36 شهرًا", "60 شهرًا"], 1);
addQuestion("ما هي التسمية الدقيقة للخدمة التي تُتيح السحب من جهاز الصراف دون بطاقة؟", ["Hawalatic", "Cardless", "Flexy", "Edahabia بدون بطاقة"], 1);
addQuestion("كيف يتم صرف منحة التقاعد بالنسبة للزبائن المسجلين لدى بريد الجزائر؟", ["عبر شيك", "بواسطة حوالة تقليدية", "عن طريق تحويل مباشر لحساب CCP", "سحب نقدي يدوي"], 2);
addQuestion("الرمز السري المعروف بـ PIN يُستعمل في أي من السياقات البريدية التالية؟", ["تشغيل الهاتف", "سحب من الموزع الآلي", "الدخول للمنصة الرقمية", "تسجيل الدخول للموقع"], 1);
addQuestion("من بين الخصائص التالية، ما الذي يميز الحوالة الفورية عن العادية؟", ["لا فرق", "العادية أسرع", "الفورية تُنفذ بشكل لحظي", "الفورية مجانية أكثر"], 2);
addQuestion("ما هي أقصى مدة يُسمح خلالها ببقاء طرد بريدي في المكتب دون استلام؟", ["أسبوع", "15 يومًا", "شهر كامل", "شهرين"], 2);
addQuestion("الجهاز الذي يُعرف باسم 'PIN PAD' يُستخدم من أجل:", ["فواتير الكهرباء", "تعبئة الرصيد", "كشوفات الحساب", "قراءة بيانات البطاقة الذهبية لتأكيد المعاملات"], 3);
addQuestion("هل تُمكن البطاقة الذهبية من إرسال أموال بين الحسابات؟", ["نعم في جميع الحالات", "لا أبدًا", "فقط دوليًا", "فقط بين أفراد الأسرة"], 0);
addQuestion("أي من هذه البطاقات مخصصة حصريًا للمعاملات الإلكترونية؟", ["بطاقة الشفاء", "Edahabia", "بطاقة هوية وطنية", "بطاقة تقاعدية"], 1);
addQuestion("كيف يُمكن للزبون تغيير رمزه السري للبطاقة الذهبية؟", ["من خلال التطبيق", "بإرسال بريد إلكتروني", "عبر الصراف الآلي", "الخدمة غير متاحة"], 2);
addQuestion("عند محاولة إرسال طرد يحتوي على مادة خطرة، ما هو تصرف العون البريدي؟", ["قبوله مباشرة", "رفض العملية تمامًا", "فتحه يدويًا", "السؤال فقط عن محتواه"], 1);
addQuestion("عند نسيان أو فقدان وثيقة تعريف داخل المكتب، ما هو الإجراء الإداري المتبع؟", ["تمزيق الوثيقة", "تبليغ الشرطة", "الاحتفاظ بها والتبليغ عنها", "إعادتها فورًا للمعني"], 2);
addQuestion("ما اسم التطبيق الرسمي المعتمد من بريد الجزائر للخدمات المتنقلة؟", ["BaridNet", "Edahabia App", "BaridiMob", "PostDZ"], 2);
addQuestion("في حالة حدوث عطل في الموزع الآلي بالبريد، ما هو التصرف المتوقع من العامل؟", ["محاولة الإصلاح", "إبلاغ القسم التقني مباشرة", "ترك الجهاز في وضع الانتظار", "فصل التيار الكهربائي"], 1);
addQuestion("كم تدوم صلاحية وصل إيداع مالي في مصلحة البريد؟", ["72 ساعة", "أسبوع", "نصف شهر", "دائمة"], 2);
addQuestion("من هي الهيئة الرقابية المختصة بمراقبة سير الخدمات البريدية؟", ["وزارة الدفاع", "جهاز الأمن", "مفتشية البريد", "هيئة خارجية"], 2);
addQuestion("كم عدد الأرقام التي يحتوي عليها الحساب البريدي الجاري؟", ["10 أرقام", "12 رقمًا", "20 رقمًا", "25 رقمًا"], 1);
addQuestion("رمز مؤسسة بريد الجزائر المعتمد في التحويلات هو:", ["001", "003", "007", "101"], 2);
addQuestion("من المسؤول الإداري الأول عن متابعة الأموال في مكتب البريد؟", ["حارس المكتب", "الساعي", "قابض البريد", "مفتش الولاية"], 2);
addQuestion("في حال نسيان الرقم السري للبطاقة الذهبية، ما الإجراء الرسمي الذي يجب على الزبون اتباعه؟", 
["الاتصال بخدمة الزبائن لطلب رقم جديد", 
"إعادة تعيين الرقم السري مباشرة من تطبيق بريدي موب", 
"طلب إعادة إصدار البطاقة عبر المكتب البريدي", 
"الدخول لحساب CCP وتغيير الرقم السري إلكترونيًا"], 2);

addQuestion("ما المقصود بمصطلح OTP المستخدم في المعاملات عبر الإنترنت؟", ["رمز دائم", "كلمة مرور موحدة", "رمز تعديل البيانات", "رمز يستخدم لمرة واحدة فقط لأمان العمليات"], 3);
addQuestion("وثيقة 'CH25' تُستخدم خصيصًا من أجل:", ["طلب توظيف", "إرسال شحنة", "طلب دفتر صكوك", "تحديث التوقيع الخاص بالصك أو الحساب"], 3);
addQuestion("كم عدد المرات التي يُسمح فيها بمحاولة إدخال الرقم السري في الموزع قبل قفل البطاقة؟", ["مرتين", "ثلاث مرات", "أربع محاولات", "خمس محاولات"], 1);
addQuestion("هل يمكن فتح حساب بريدي دون وثائق تعريف رسمية؟", ["نعم دائمًا", "ممنوع إطلاقًا", "في حالات استثنائية", "مسموح للأجانب فقط"], 1);
addQuestion("ما هي القيمة الحالية للرسوم السنوية الخاصة بالبطاقة الذهبية؟", ["0 دج", "350 دج", "500 دج", "1000 دج"], 1);
addQuestion("عند استقبال شكوى من الزبون، ما هي أول خطوة يجب اتخاذها؟", ["رفض الشكوى", "تقديم استمارة خاصة", "تلقيها شفويًا", "تحويلها للمسؤول الأول"], 1);
addQuestion("عند إرسال طرد دولي، أي وثيقة من التالية تُعد ضرورية؟", ["جواز سفر", "بطاقة هوية", "تصريح جمركي", "دفتر العائلة"], 2);
addQuestion("في أي سنة بدأت خدمة البطاقة الذهبية بشكل رسمي في الجزائر؟", ["2014", "2016", "2017", "2019"], 1);
addQuestion("أي وسيلة يُفضل استخدامها عند إرسال مبلغ مالي دون امتلاك حساب بريدي؟", ["شيك", "حوالة بريدية", "بطاقة Edahabia", "تحويل بنكي"], 1);
addQuestion("أي من هذه المواد يُمنع إرسالها تمامًا عبر خدمات البريد؟", ["ملابس", "أدوية", "أسلحة نارية", "كتب علمية"], 2);
addQuestion("في حال رفض الزبون دفع الرسوم المفروضة على الخدمة، ماذا على العامل فعله؟", ["إلغاء العملية", "استدعاء الشرطة", "شرح الرسوم بدقة", "إرجاع الطرد فورًا"], 2);
addQuestion("ما الحد الأقصى المقبول لوزن طرد عادي داخل الجزائر؟", ["10 كغ", "20 كغ", "30 كغ", "50 كغ"], 2);
addQuestion("هل يُسمح بتوكيل شخص آخر لسحب أموال من حساب زبون؟", ["نعم بتوكيل موثق", "لا أبدًا", "للقريب فقط", "عبر الهاتف فقط"], 0);
addQuestion("عند إدخال بطاقة غير معترف بها من الموزع، ما هي الرسالة الأكثر شيوعًا؟", ["تحميل جارٍ", "خطأ في البطاقة", "انتظر من فضلك", "تم السحب بنجاح"], 1);
addQuestion("أي من الهيئات التالية تُشرف على جودة الخدمات البريدية؟", ["مفتشية العمل", "المديرية العامة للبريد", "قسم الشكاوي", "كل ما سبق صحيح"], 3);
addQuestion("من هي الجهة المخولة بإصدار الطوابع البريدية الرسمية؟", ["الجمارك", "مؤسسة بريد الجزائر", "وزارة المالية", "المكتبة الوطنية الجزائرية"], 1);
addQuestion("لفتح حساب CCP، ما هي الوثائق الإدارية الأساسية المطلوبة؟", ["شهادة ميلاد", "وثيقة CH1 + نسخة من بطاقة الهوية", "بطاقة إقامة", "شهادة عمل"], 1);
addQuestion("ما هو التعريف الأدق للاتصال الإداري داخل مؤسسة بريدية؟", ["مكالمات شخصية", "مراسلات بين موظفين", "نقل المعطيات داخل المرفق", "البريد الإلكتروني المهني فقط"], 2);
addQuestion("هل تُمكن البطاقة الذهبية من سحب رواتب التقاعد؟", ["غير ممكن", "ممكن", "فقط من مكتب البريد", "خاص بالموظفين فقط"], 1);
addQuestion("ما هي المدة القصوى التي تبقى فيها البطاقة الذهبية صالحة قبل التجديد؟", ["دائمة", "تُجدد مرتين", "سنتين", "ثلاث سنوات"], 2);
addQuestion("في حال كان اسم المستفيد في الطرد غير مطابق للوثيقة، يجب:", ["تسليمه", "رفضه", "تمزيقه", "سحبه من الحفظ"], 1);
addQuestion("إذا تم إرسال حوالة بمبلغ 10,000 دج بنسبة عمولة 1.5%، ما هو مبلغ العمولة؟", ["100 دج", "125 دج", "150 دج", "200 دج"], 2);
addQuestion("ما نوع العملية التي تتطلب التحقق المزدوج (Double Authentication)؟", ["سحب عبر ATM", "دفع إلكتروني", "الاطلاع على الرصيد", "تعبئة الهاتف"], 1);
addQuestion("في حال عدم تطابق بيانات المستلم على الطرد، الإجراء الصحيح هو:", ["تسليمه", "إرجاعه للمرسل", "الاحتفاظ به", "إحالته لمصلحة أخرى"], 1);
addQuestion("ما الوسيلة المعتمدة للتواصل الإداري الداخلي؟", ["الصحف", "الراديو", "المذكرة الإدارية", "الإعلانات الرسمية"], 2);
addQuestion("ما هو الحد الأقصى للسحب بوثيقة SFPO1؟", ["1 مليون سنتيم", "5 ملايين سنتيم", "10 ملايين سنتيم", "2 مليون سنتيم"], 3);
addQuestion("ما هو الإجراء المتخذ عندما لا يُستلم الطرد بعد 30 يومًا؟", ["إلغاؤه", "إرجاعه للمرسل", "إتلافه", "تحويله للإدارة"], 1);



  let currentQuestion = 0;
        let answers = Array(questions.length).fill(null);
        let timers = Array(questions.length).fill(120); // 120 ثانية
        let timerInterval;

        function showQuestion() {
            clearInterval(timerInterval);

            const q = questions[currentQuestion];
            document.getElementById("question-text").textContent = `السؤال ${currentQuestion + 1}: ${q.q}`;
            const choicesDiv = document.getElementById("choices");
            choicesDiv.innerHTML = "";

            q.choices.forEach((choice, index) => {
                const label = document.createElement("label");
                const input = document.createElement("input");
                input.type = "radio";
                input.name = "choice";
                input.value = index;
                // تعطيل الخيارات إذا انتهى الوقت أو تم اختيار إجابة
                input.disabled = (timers[currentQuestion] <= 0 || answers[currentQuestion] !== null);
                if (answers[currentQuestion] === index) input.checked = true;

                input.addEventListener("change", () => {
                    if (answers[currentQuestion] === null) { // السماح بالاختيار فقط إذا لم تتم الإجابة بالفعل
                        answers[currentQuestion] = index;
                        disableChoices(); // تعطيل الخيارات بعد الاختيار
                    }
                });

                label.appendChild(input);
                label.appendChild(document.createTextNode(" " + choice));
                choicesDiv.appendChild(label);
            });

            document.getElementById("prevBtn").style.display = currentQuestion === 0 ? "none" : "inline";

            startTimer();
        }

        function startTimer() {
            const timerElement = document.getElementById("timer");

            if (timers[currentQuestion] <= 0) {
                timerElement.textContent = "انتهى الوقت لهذا السؤال.";
                disableChoices(); // التأكد من تعطيل الخيارات إذا انتهى الوقت
                return;
            }

            updateTimerText();

            timerInterval = setInterval(() => {
                timers[currentQuestion]--;
                updateTimerText();

                if (timers[currentQuestion] <= 0) {
                    clearInterval(timerInterval);
                    timerElement.textContent = "انتهى الوقت لهذا السؤال.";
                    disableChoices();
                    // لا تنتقل تلقائيا للسؤال التالي في هذا السيناريو، اترك المستخدم ينتقل بنفسه أو ينهي الامتحان
                    // setTimeout(nextQuestion, 1000); 
                }
            }, 1000);
        }

        function updateTimerText() {
            const timerElement = document.getElementById("timer");
            const seconds = timers[currentQuestion];
            const minutes = Math.floor(seconds / 60);
            const remainingSeconds = seconds % 60;

            const minStr = minutes.toString().padStart(2, '0');
            const secStr = remainingSeconds.toString().padStart(2, '0');

            timerElement.textContent = `الوقت المتبقي: ${minStr}:${secStr}`;
        }

        function disableChoices() {
            const inputs = document.querySelectorAll("input[name='choice']");
            inputs.forEach(input => input.disabled = true);
        }

        // جعل الدالة عامة ليمكن استدعاؤها من HTML
        window.nextQuestion = function() {
            if (currentQuestion < questions.length - 1) {
                currentQuestion++;
                showQuestion();
            } else {
                finishQuiz();
            }
        };

        // جعل الدالة عامة ليمكن استدعاؤها من HTML
        window.prevQuestion = function() {
            if (currentQuestion > 0) {
                currentQuestion--;
                showQuestion();
            }
        };

        function finishQuiz() {
            clearInterval(timerInterval);
            examStarted = false; // الامتحان انتهى بنجاح، لذا لن يتم تسجيل إقصاء
            localStorage.removeItem("disqualified"); // مسح حالة الإقصاء عند الانتهاء بنجاح

            examSection.classList.add("hidden"); // إخفاء قسم الامتحان
            document.getElementById("question-text").classList.add("hidden");
            document.getElementById("choices").classList.add("hidden");
            document.getElementById("timer").classList.add("hidden");
            document.getElementById("nav-buttons").classList.add("hidden");

            let correct = 0;
            let wrong = 0;

            questions.forEach((q, i) => {
                // التحقق مما إذا تم الإجابة على السؤال
                if (answers[i] !== null) {
                    if (answers[i] === q.correct) {
                        correct++;
                    } else {
                        wrong++;
                    }
                }
            });

            let finalScore = correct - wrong;
            if (finalScore < 0) finalScore = 0; // لا يمكن أن تكون النتيجة سلبية

            const resultDiv = document.getElementById("result");
            resultDiv.classList.remove("hidden");

            let message = "";
            if (finalScore >= Math.ceil(questions.length / 2)) {
                message = `<span style="color: green;">✔️ مبروك! لقد نجحت</span>`;
            } else {
                message = `<span style="color: red;">❌ للأسف، حظ موفق في المرة القادمة</span>`;
            }

            resultDiv.innerHTML = `
                <h2>انتهى الاختبار</h2>
                <p>عدد الإجابات الصحيحة: ${correct}</p>
                <p>عدد الإجابات الخاطئة: ${wrong}</p>
                <p><strong>علامتك النهائية: ${finalScore} من ${questions.length}</strong></p>
                <p>${message}</p>
                <footer style="
                    position: fixed;
                    bottom: 0;
                    width: 85%;
                    text-align: center;
                    font-size: 12px;
                    color: #555;
                    background-color: transparent;
                    padding: 5px 0;
                    direction: rtl;
                ">
                    Created by: Haricha Younes
                </footer>
                <button onclick="showCorrectionPage()">تصحيح الاختبار</button>
            `;
        }

        // جعل الدالة عامة ليمكن استدعاؤها من HTML
        window.showCorrectionPage = function() {
            document.getElementById("result").classList.add("hidden");
            const correctionSection = document.getElementById("correction-section");
            correctionSection.innerHTML = "<h2>تصحيح الاختبار:</h2>";

            questions.forEach((q, index) => {
                const userAnswer = answers[index];
                const correctAnswer = q.correct;

                const div = document.createElement("div");
                div.style.padding = "10px";
                div.style.marginBottom = "10px";
                div.style.borderRadius = "10px";
                div.style.backgroundColor = userAnswer === correctAnswer ? "#d4edda" : "#f8d7da"; // أخضر للفصل، أحمر للخطأ

                div.innerHTML = `
                    <p><strong>سؤال ${index + 1}:</strong> ${q.q}</p>
                    <p>✔️ الإجابة الصحيحة: ${q.choices[correctAnswer]}</p>
                    <p>📝 إجابتك: ${userAnswer !== null ? q.choices[userAnswer] : "لم يجب"}</p>
                `;

                correctionSection.appendChild(div);
            });

            correctionSection.classList.remove("hidden");
        };
    });
  </script>
</body>
</html>  
