<!DOCTYPE html>

<html lang="ar" dir="rtl">

<head>

  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>مركز الامتحان - بريد الجزائر</title>
  
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
      <h3> مكلف بالزبائن</h3>
      <div class="status">
        <span>55 الأسئلة</span>
        <span class="orange">لم يبدأ بعد</span>
      </div>
      <p>امتحان مكلف بالزبائن</p>
      <button onclick="startExam()">بدء الامتحان</button>
    </div>
  </div>

  <div id="exam-section">
    <div class="question" id="question-text"></div>
    <div id="timer"></div>
    <div id="choices"></div>
    <div id="nav-buttons">
      <button id="prevBtn" onclick="prevQuestion()">السابق</button>
      <button onclick="nextQuestion()">التالي</button>
    </div>
    <div id="result" class="hidden"></div>
  </div>
  
<!-- هذا هو العنصر الذي تحتاجه لتصحيح الأسئلة -->
<div id="correction-section" class="hidden"></div>

  <script>
    // التقاط الاسم من الرابط
    function getNameFromURL() {
      const params = new URLSearchParams(window.location.search);
      return params.get("name") || "المترشح";
    }

    // عرض الاسم
    document.addEventListener("DOMContentLoaded", function () {
      const name = getNameFromURL();
      document.getElementById("username").textContent = `مرحبا ${name.replace(/\./g, ' ')}`;
    });

    // بدء الامتحان
    function startExam() {
      document.getElementById("welcome-section").style.display = "none";
      document.getElementById("exam-section").style.display = "block";
      showQuestion();
    }

    const questions = [];

    function addQuestion(q, options, correct) {
      questions.push({ q, choices: options, correct });
    }



addQuestion("ما هو الرمز الذي يُستخدم لتحويل الأموال إلى حساب CCP؟", ["RIB", "RIP", "IBAN", "SWIFT"], 1);
addQuestion("متى تأسس بريد الجزائر؟", ["1962", "1975", "1990", "2000"], 0);
addQuestion("ما هو الحد الأقصى للمبلغ المسموح بسحبه من الموزع الآلي في الجزائر؟", ["10000 دج", "20000 دج", "50000 دج", "100000 دج"], 1);
addQuestion("في حال ضياع البطاقة الذهبية، يجب على الزبون:", ["الاتصال بالشرطة", "الاتصال بالبنك", "الاتصال بمركز الزبائن لبريد الجزائر", "الذهاب إلى مكتب الضرائب"], 2);
addQuestion("ما هي الوثيقة الأساسية التي يجب تقديمها عند سحب الأموال من الحساب؟", ["رخصة السياقة", "بطاقة التعريف الوطنية", "جواز السفر فقط", "دفتر العائلة"], 1);
addQuestion("ما هو الغرض الرئيسي من وثيقة \"SFPO1\"؟", ["تفعيل خدمات الإنترنت", "طلب بطاقة ذهبية", "تقديم شكوى", "سحب أو تحويل أموال وعمليات أخرى"], 3);
addQuestion("عند إدخال الرقم السري ثلاث مرات خاطئة، يتم:", ["فتح الحساب", "حذف الحساب", "تجميد البطاقة", "مضاعفة الرصيد"], 2);
addQuestion("كيف يمكن تتبع الطرود البريدية؟", ["عن طريق الهاتف فقط", "عن طريق الموقع الإلكتروني لبريد الجزائر", "غير ممكن", "يجب زيارة مركز الفرز"], 1);
addQuestion("مدة صلاحية البطاقة الذهبية هي:", ["سنة واحدة", "سنتان", "3 سنوات", "5 سنوات"], 1);
addQuestion("ما هي الخدمة التي تسمح بالسحب بدون بطاقة؟", ["Hawalatic", "Cardless", "Flexy", "Edahabia"], 1);
addQuestion("عند التقاعد، يتم صرف المعاش عبر:", ["شيك فقط", "حوالة بريدية", "تحويل بريدي مباشر", "دفع نقدي"], 2);
addQuestion("يُستخدم الرمز السري PIN في:", ["تشغيل الهاتف", "سحب الأموال من الموزع", "الدخول إلى الحاسوب", "فتح الأبواب الآلية"], 1);
addQuestion("ما هو الفرق بين حوالة عادية وحوالة فورية؟", ["لا فرق", "العادية أسرع", "الفورية أسرع", "الفورية أرخص"], 2);
addQuestion("ما هي أقصى مدة لبقاء طرد بريدي في مكتب البريد قبل إرجاعه؟", ["7 أيام", "15 يومًا", "30 يومًا", "60 يومًا"], 2);
addQuestion("ما هي الوظيفة الأساسية لجهاز \"PIN PAD\" في مكاتب البريد؟", ["إصدار فواتير", "شحن هواتف", "طباعة كشوفات", "قراءة بيانات البطاقة الذهبية لخدمات البريد"], 3);
addQuestion("هل يمكن إرسال الأموال عبر البطاقة الذهبية؟", ["نعم", "لا", "فقط دوليًا", "فقط بين أفراد العائلة"], 0);
addQuestion("ما هو نوع البطاقة التي تتيح الدفع الإلكتروني عبر الإنترنت؟", ["بطاقة الشفاء", "البطاقة الذهبية", "بطاقة الهوية", "بطاقة التقاعد"], 1);
addQuestion("كيف يتم تغيير الرقم السري للبطاقة الذهبية؟", ["عبر التطبيق", "عبر البريد الإلكتروني", "عبر الموزع الآلي", "غير ممكن"], 2);
addQuestion("عند تسليم طرد يحتوي على مواد خطرة، يجب على العامل:", ["قبوله مباشرة", "رفضه فورًا", "فتحه والتحقق", "سؤاله عن محتواه فقط"], 1);
addQuestion("ما هو الإجراء عند فقدان وثيقة هوية زبون في البريد؟", ["تمزيقها", "الاتصال بالشرطة", "الاحتفاظ بها والتبليغ", "إعادتها فورًا للزبون"], 2);
addQuestion("ما هو تطبيق بريد الجزائر الرسمي؟", ["BaridNet", "Edahabia", "BaridiMob", "AlgeriePost"], 2);
addQuestion("عند حدوث خلل في الموزع، على العامل:", ["إصلاحه", "الاتصال بالمصلحة التقنية", "تركه معلقًا", "نزع الكهرباء"], 1);
addQuestion("مدة صلاحية وصل الإيداع؟", ["3 أيام", "7 أيام", "15 يومًا", "لا تنتهي"], 2);
addQuestion("ما هي الجهة المخولة بالمراقبة البريدية؟", ["وزارة الدفاع", "الشرطة", "مفتشية البريد", "مكتب الشؤون الخارجية"], 2);
addQuestion("كم يبلغ عدد أرقام الحساب البريدي الجاري؟", ["10", "12", "20", "25"], 1);
addQuestion("ما هو رمز المؤسسة البريدية في التحويلات؟", ["001", "003", "007", "101"], 0);
addQuestion("من هو المسؤول الأول عن الأموال في مكتب البريد؟", ["الحارس", "ساعي البريد", "قابض البريد", "المفتش"], 2);
addQuestion("في حال نشوب حريق داخل المكتب، أول إجراء هو:", ["الركض", "فتح النوافذ", "إطلاق الإنذار", "إخفاء الأموال"], 2);", "بريد مضمون", "بريد عادي", "بريد ناقص"], 1);توقيع العميل
addQuestion("ما هي الوظيفة الأساسية لجهاز \"PIN PAD\" في مكاتب البريد؟", ["إصدار فواتير", "شحن هواتف", "طباعة كشوفات", "قراءة بيانات البطاقة الذهبية لخدمات البريد"], 3);
addQuestion("ما هو تعريف \"OTP\"؟", ["رقم سري ثابت", "كلمة مرور دائمة", "رمز لتغيير معلومات", "رمز رقمي لمرة واحدة لأمان المعاملات عبر الإنترنت"], 3);
addQuestion("ما هو الغرض من وثيقة \"CH25\"؟", ["طلب وظيفة", "إرسال طرد", "طلب دفتر صكوك", "توثيق وتغيير التوقيع للحساب والصك البريدي"], 3);    
addQuestion("كم يبلغ عدد محاولات إدخال الرقم السري في الموزع قبل حجز البطاقة؟", ["2", "3", "4", "5"], 1);
addQuestion("هل يمكن فتح حساب بريدي بدون وثيقة هوية؟", ["نعم", "لا", "أحيانًا", "للأجانب فقط"], 1);
addQuestion("ما هي الرسوم السنوية للبطاقة الذهبية؟", ["مجانية", "350 دج", "500 دج", "1000 دج"], 1);
addQuestion("عند تلقي شكوى، أول إجراء هو:", ["رفضها", "تقديم استمارة شكوى", "تسجيلها شفويًا", "تحويلها لرئيس المركز"], 1);
addQuestion("عند إرسال طرد خارج الوطن، يجب تقديم:", ["جواز سفر", "بطاقة هوية", "بيان جمركي", "دفتر العائلة"], 2);
addQuestion("في أي سنة تم إطلاق البطاقة الذهبية؟", ["2014", "2016", "2017", "2019"], 1);
addQuestion("ما هي طريقة إرسال الأموال داخليًا دون حساب؟", ["شيك", "حوالة بريدية", "بطاقة ذهبية", "تحويل بنكي"], 1);
addQuestion("يمنع إرسال المواد التالية عبر البريد:", ["الملابس", "الأدوية", "الأسلحة", "الكتب"], 2);
addQuestion("عند رفض الزبون دفع رسوم الخدمة، يتم:", ["إلغاؤها", "تحويله للشرطة", "شرحها وتوضيحها له", "إرجاع الطرد فورًا"], 2);
addQuestion("ما هو الحد الأقصى للوزن المسموح به في الطرود العادية؟", ["10 كلغ", "20 كلغ", "30 كلغ", "50 كلغ"], 2);
addQuestion("هل يمكن تفويض شخص لسحب الأموال من حساب آخر؟", ["نعم بوثيقة تفويض", "لا", "فقط من الأقارب", "بالهاتف فقط"], 0);
addQuestion("عند إدخال بطاقة غير صالحة في الموزع، تظهر رسالة:", ["جاري التحميل", "خطأ في البطاقة", "الرجاء الانتظار", "تم السحب"], 1);
addQuestion("ما هي الجهة التي تراقب جودة الخدمات؟", ["مفتشية العمل", "المديرية العامة", "مكتب الشكاوي", "كل ما سبق"], 3);
addQuestion("ما هي الجهة المسؤولة عن إصدار الطوابع البريدية؟", ["الجمارك", "بريد الجزائر", "وزارة المالية", "المكتبة الوطنية"], 1);
addQuestion("ما هي الوثائق المطلوبة لفتح حساب بريدي جاري؟", ["نسخة من شهادة الميلاد", "الوثيقة CH1 + نسخة من بطاقة الهوية", "بطاقة إقامة", "شهادة عمل"], 1);
addQuestion("ما المقصود بالاتصال الإداري؟", ["مكالمات شخصية", "مراسلات بين الأعوان", "نقل المعلومات داخل المؤسسة", "البريد الإلكتروني فقط"], 2);
addQuestion("هل يمكن استخدام البطاقة الذهبية لسحب راتب التقاعد؟", ["لا", "نعم", "فقط في المكاتب", "فقط للموظفين"], 1);
addQuestion("كم مرة في السنة يتم تجديد البطاقة الذهبية؟", ["مرة", "مرتين", "كل سنتين", "كل 3 سنوات"], 2);
addQuestion("عند وجود خطأ في اسم المستفيد على الطرد، يجب:", ["تسليمه", "رفضه", "تمزيقه", "سحبه"], 1);
addQuestion("عند إرسال حوالة بريدية بمبلغ 10,000 دج، وعمولة 1.5%، فما قيمة العمولة؟", ["100 دج", "125 دج", "150 دج", "200 دج"], 2);
addQuestion("أي من العمليات التالية تتطلب التحقق المزدوج؟", ["سحب من ATM", "الدفع عبر الإنترنت", "الاطلاع على الرصيد", "شحن الهاتف"], 1);
addQuestion("في حالة اختلاف بيانات المستلم في الطرد، فإن العامل البريدي يجب أن:", ["يسلم الطرد", "يعيد الطرد", "يحتفظ بالطرد", "يرسل نسخة للمصلحة"], 1);
addQuestion("ما هو الإجراء المتخذ في حالة عدم تسلم الطرد بعد مرور 30 يومًا؟", ["يُلغى", "يُعاد للمرسل", "يُتلف", "يُرسل إلى إدارة المركز"], 1);
    

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
    input.disabled = (timers[currentQuestion] <= 0 || answers[currentQuestion] !== null);
    if (answers[currentQuestion] === index) input.checked = true;

    input.addEventListener("change", () => {
      if (answers[currentQuestion] === null) {
        answers[currentQuestion] = index;
        disableChoices();
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
    return;
  }

  timerElement.textContent = `الوقت المتبقي: ${timers[currentQuestion]} ثانية`;

  timerInterval = setInterval(() => {
    timers[currentQuestion]--;
    timerElement.textContent = `الوقت المتبقي: ${timers[currentQuestion]} ثانية`;

    if (timers[currentQuestion] <= 0) {
      clearInterval(timerInterval);
      timerElement.textContent = "انتهى الوقت لهذا السؤال.";
      disableChoices();
      setTimeout(nextQuestion, 1000);
    }
  }, 1000);
}

function disableChoices() {
  const inputs = document.querySelectorAll("input[name='choice']");
  inputs.forEach(input => input.disabled = true);
}

function nextQuestion() {
  if (currentQuestion < questions.length - 1) {
    currentQuestion++;
    showQuestion();
  } else {
    finishQuiz();
  }
}

function prevQuestion() {
  if (currentQuestion > 0 && answers[currentQuestion - 1] === null) {
    currentQuestion--;
    showQuestion();
  }
}

function finishQuiz() {
  clearInterval(timerInterval);
  document.getElementById("question-text").classList.add("hidden");
  document.getElementById("choices").classList.add("hidden");
  document.getElementById("timer").classList.add("hidden");
  document.getElementById("nav-buttons").classList.add("hidden");

  let correct = 0;
  let wrong = 0;

  questions.forEach((q, i) => {
    if (answers[i] !== null && timers[i] > 0) {
      if (answers[i] === q.correct) {
        correct++;
      } else {
        wrong++;
      }
    }
  });

  let finalScore = correct - wrong;
  if (finalScore < 0) finalScore = 0;

  const resultDiv = document.getElementById("result");
  resultDiv.classList.remove("hidden");

  let message = "";
  if (finalScore >= Math.ceil(questions.length / 2)) {
    message = `<span style=\"color: green;\">✔️ مبروك! لقد نجحت</span>`;
  } else {
    message = `<span style=\"color: red;\">❌ للأسف، حظ موفق في المرة القادمة</span>`;
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
  font-size: 5px;
  color: #555;
  background-color: transparent;
  padding: 5px 0;
  direction: rtl;
">
Created by: Haricha Younes 
</footer>

    <button onclick=\"showCorrectionPage()\">تصحيح الاختبار</button>
  `;
}

function showCorrectionPage() {
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
    div.style.backgroundColor = userAnswer === correctAnswer ? "#d4edda" : "#f8d7da";

    div.innerHTML = `
      <p><strong>سؤال ${index + 1}:</strong> ${q.q}</p>
      <p>✔️ الإجابة الصحيحة: ${q.choices[correctAnswer]}</p>
      <p>📝 إجابتك: ${userAnswer !== null ? q.choices[userAnswer] : "لم يجب"}</p>
    `;

    correctionSection.appendChild(div);
  });

  correctionSection.classList.remove("hidden");
}
<!-- باقي محتوى صفحة التصحيح -->


</script>
</body>
</html>
    
