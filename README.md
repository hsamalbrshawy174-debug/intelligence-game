# intelligence-game
لعبه تقوي الذكاء عباره عن مجموعه من الاسئله تضم معلومات  
<!doctype html>
<html lang="ar" dir="rtl" class="h-full">
 <head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>لعبة الأسئلة</title>
  <script src="https://cdn.tailwindcss.com/3.4.17"></script>
  <script src="https://cdn.jsdelivr.net/npm/lucide@0.263.0/dist/umd/lucide.min.js"></script>
  <script src="/_sdk/element_sdk.js"></script>
  <link href="https://fonts.googleapis.com/css2?family=Tajawal:wght@400;500;700;800;900&amp;display=swap" rel="stylesheet">
  <style>
    * { font-family: 'Tajawal', sans-serif; }

    @keyframes slideUp {
      from { opacity: 0; transform: translateY(40px); }
      to { opacity: 1; transform: translateY(0); }
    }
    @keyframes fadeIn {
      from { opacity: 0; }
      to { opacity: 1; }
    }
    @keyframes pulse-glow {
      0%, 100% { box-shadow: 0 0 20px rgba(255, 193, 7, 0.3); }
      50% { box-shadow: 0 0 40px rgba(255, 193, 7, 0.6); }
    }
    @keyframes bounce-in {
      0% { transform: scale(0.3); opacity: 0; }
      50% { transform: scale(1.05); }
      70% { transform: scale(0.95); }
      100% { transform: scale(1); opacity: 1; }
    }
    @keyframes confetti-fall {
      0% { transform: translateY(-100%) rotate(0deg); opacity: 1; }
      100% { transform: translateY(800%) rotate(720deg); opacity: 0; }
    }
    @keyframes shake {
      0%, 100% { transform: translateX(0); }
      20% { transform: translateX(-8px); }
      40% { transform: translateX(8px); }
      60% { transform: translateX(-5px); }
      80% { transform: translateX(5px); }
    }
    @keyframes star-spin {
      from { transform: rotate(0deg) scale(0); opacity: 0; }
      to { transform: rotate(360deg) scale(1); opacity: 1; }
    }
    .animate-slide-up { animation: slideUp 0.6s ease-out forwards; }
    .animate-fade-in { animation: fadeIn 0.4s ease-out forwards; }
    .animate-bounce-in { animation: bounce-in 0.5s ease-out forwards; }
    .animate-shake { animation: shake 0.5s ease-out; }
    .animate-star { animation: star-spin 0.6s ease-out forwards; }
    .pulse-glow { animation: pulse-glow 2s ease-in-out infinite; }

    .confetti-piece {
      position: fixed;
      width: 10px;
      height: 10px;
      top: -10px;
      animation: confetti-fall 3s ease-in forwards;
      z-index: 100;
      pointer-events: none;
    }

    .question-card {
      background: linear-gradient(135deg, #1a1a2e 0%, #16213e 50%, #0f3460 100%);
      border: 2px solid rgba(255, 193, 7, 0.3);
    }

    .progress-fill {
      transition: width 0.5s ease-out;
    }

    .option-btn {
      transition: all 0.2s ease;
    }
    .option-btn:hover {
      transform: translateY(-2px);
    }

    .bg-pattern {
      background-color: #0a0a1a;
      background-image:
        radial-gradient(ellipse at 20% 50%, rgba(15, 52, 96, 0.4) 0%, transparent 50%),
        radial-gradient(ellipse at 80% 20%, rgba(233, 69, 96, 0.15) 0%, transparent 50%),
        radial-gradient(ellipse at 50% 80%, rgba(255, 193, 7, 0.1) 0%, transparent 50%);
    }
  </style>
  <style>body { box-sizing: border-box; }</style>
  <script src="/_sdk/data_sdk.js" type="text/javascript"></script>
 </head>
 <body class="h-full bg-pattern text-white overflow-auto">
  <main id="app" class="h-full w-full flex flex-col items-center justify-center p-4">
   <!-- Start Screen -->
   <section id="start-screen" class="text-center animate-slide-up max-w-2xl w-full">
    <div class="mb-8">
     <div class="w-24 h-24 mx-auto mb-6 rounded-2xl bg-gradient-to-br from-amber-400 to-amber-600 flex items-center justify-center pulse-glow">
      <span class="text-5xl">🧠</span>
     </div>
     <h1 id="main-title" class="text-4xl font-900 mb-3 bg-gradient-to-l from-amber-300 to-amber-500 bg-clip-text text-transparent">لعبة الأسئلة</h1>
     <p class="text-gray-400 text-lg">استمتع بـ ١٠٠ مستوى متزايد الصعوبة!</p>
     <div class="flex gap-3 justify-center mt-6">
      <button id="download-btn" onclick="toggleModal('download-modal')" class="flex items-center gap-2 bg-gradient-to-l from-cyan-500 to-cyan-600 text-white font-bold px-6 py-3 rounded-xl hover:from-cyan-400 hover:to-cyan-500 transition-all shadow-lg shadow-cyan-500/30"> <i data-lucide="download" class="w-5 h-5"></i> تحميل </button> <button id="share-btn" onclick="toggleModal('share-modal')" class="flex items-center gap-2 bg-gradient-to-l from-purple-500 to-purple-600 text-white font-bold px-6 py-3 rounded-xl hover:from-purple-400 hover:to-purple-500 transition-all shadow-lg shadow-purple-500/30"> <i data-lucide="share-2" class="w-5 h-5"></i> شارك </button>
     </div><!-- Download Modal -->
     <div id="download-modal" class="hidden fixed inset-0 bg-black/60 flex items-center justify-center z-50 p-4">
      <div class="bg-gradient-to-b from-slate-900 to-slate-950 rounded-3xl p-8 max-w-md w-full border-2 border-cyan-500/30 animate-bounce-in">
       <div class="flex items-center gap-3 mb-6"><i data-lucide="download" class="w-8 h-8 text-cyan-400"></i>
        <h3 class="text-2xl font-bold">طرق التحميل</h3>
       </div>
       <div class="space-y-3"><button onclick="downloadGame(); toggleModal('download-modal')" class="w-full flex items-center gap-3 bg-white/10 hover:bg-white/20 border border-white/30 rounded-xl p-4 transition-all"> <i data-lucide="file-download" class="w-6 h-6 text-cyan-400"></i>
         <div class="text-right">
          <p class="font-bold">💻 ملف HTML مباشر</p>
          <p class="text-sm text-gray-400">احفظ الملف على جهازك</p>
         </div></button> <button class="w-full flex items-center gap-3 bg-white/10 hover:bg-white/20 border border-white/30 rounded-xl p-4 transition-all"> <i data-lucide="smartphone" class="w-6 h-6 text-emerald-400"></i>
         <div class="text-right">
          <p class="font-bold">📱 على الهاتف</p>
          <p class="text-sm text-gray-400">حمّل من متجر التطبيقات قريباً</p>
         </div></button> <button class="w-full flex items-center gap-3 bg-white/10 hover:bg-white/20 border border-white/30 rounded-xl p-4 transition-all"> <i data-lucide="pc" class="w-6 h-6 text-blue-400"></i>
         <div class="text-right">
          <p class="font-bold">🖥️ متجر ويندوز</p>
          <p class="text-sm text-gray-400">قريباً على Microsoft Store</p>
         </div></button>
       </div><button onclick="toggleModal('download-modal')" class="w-full mt-6 bg-white/10 border border-white/20 hover:bg-white/20 text-white font-bold py-3 rounded-xl transition-all">إغلاق</button>
      </div>
     </div><!-- Share Modal -->
     <div id="share-modal" class="hidden fixed inset-0 bg-black/60 flex items-center justify-center z-50 p-4">
      <div class="bg-gradient-to-b from-slate-900 to-slate-950 rounded-3xl p-8 max-w-md w-full border-2 border-purple-500/30 animate-bounce-in">
       <div class="flex items-center gap-3 mb-6"><i data-lucide="share-2" class="w-8 h-8 text-purple-400"></i>
        <h3 class="text-2xl font-bold">شارك اللعبة</h3>
       </div>
       <div class="space-y-3"><button onclick="shareOnWhatsApp(); toggleModal('share-modal')" class="w-full flex items-center gap-3 bg-gradient-to-l from-green-500 to-green-600 hover:from-green-400 hover:to-green-500 rounded-xl p-4 transition-all shadow-lg shadow-green-500/30"> <i data-lucide="message-circle" class="w-6 h-6"></i>
         <div class="text-right">
          <p class="font-bold text-white">💬 WhatsApp</p>
          <p class="text-sm text-white/80">شارك مع أصدقائك</p>
         </div></button> <button onclick="shareOnTwitter(); toggleModal('share-modal')" class="w-full flex items-center gap-3 bg-gradient-to-l from-blue-500 to-blue-600 hover:from-blue-400 hover:to-blue-500 rounded-xl p-4 transition-all shadow-lg shadow-blue-500/30"> <i data-lucide="share" class="w-6 h-6"></i>
         <div class="text-right">
          <p class="font-bold text-white">𝕏 تويتر</p>
          <p class="text-sm text-white/80">غرّد عن اللعبة</p>
         </div></button> <button onclick="shareOnFacebook(); toggleModal('share-modal')" class="w-full flex items-center gap-3 bg-gradient-to-l from-blue-700 to-blue-800 hover:from-blue-600 hover:to-blue-700 rounded-xl p-4 transition-all shadow-lg shadow-blue-700/30"> <i data-lucide="send" class="w-6 h-6"></i>
         <div class="text-right">
          <p class="font-bold text-white">f فيسبوك</p>
          <p class="text-sm text-white/80">شارك على صفحتك</p>
         </div></button> <button onclick="copyShareLink(); toggleModal('share-modal')" class="w-full flex items-center gap-3 bg-white/10 hover:bg-white/20 border border-white/30 rounded-xl p-4 transition-all"> <i data-lucide="copy" class="w-6 h-6 text-amber-400"></i>
         <div class="text-right">
          <p class="font-bold">📋 انسخ الرابط</p>
          <p class="text-sm text-gray-400">انسخ رابط المشاركة</p>
         </div></button>
       </div><button onclick="toggleModal('share-modal')" class="w-full mt-6 bg-white/10 border border-white/20 hover:bg-white/20 text-white font-bold py-3 rounded-xl transition-all">إغلاق</button>
      </div>
     </div>
    </div>
    <div id="level-selector" class="grid grid-cols-4 gap-2 mb-8 max-h-96 overflow-y-auto p-4 bg-white/5 rounded-2xl border border-white/10"><!-- Levels will be generated here -->
    </div>
   </section><!-- Question Screen -->
   <section id="question-screen" class="hidden max-w-xl w-full">
    <header class="mb-6">
     <div class="flex items-center justify-between mb-3"><span id="level-title" class="text-amber-400 font-bold text-lg">المستوى ١ / ١٠٠</span> <span id="live-score" class="flex items-center gap-2 text-emerald-400 font-bold text-lg"> <i data-lucide="star" class="w-5 h-5"></i> <span>٠</span> </span>
     </div>
     <div class="w-full h-3 bg-white/10 rounded-full overflow-hidden">
      <div id="progress-bar" class="progress-fill h-full bg-gradient-to-l from-amber-400 to-amber-600 rounded-full" style="width: 0%"></div>
     </div>
    </header>
    <div id="question-card" class="question-card rounded-3xl p-8 mb-6">
     <h2 id="question-text" class="text-2xl font-bold text-center leading-relaxed"></h2>
    </div>
    <div id="answer-area" class="space-y-4">
     <label for="answer-input" class="sr-only">إجابتك</label>
     <div class="relative">
      <input id="answer-input" type="text" placeholder="اكتب إجابتك هنا..." class="w-full bg-white/10 border-2 border-white/20 rounded-2xl px-6 py-4 text-xl text-center text-white placeholder-gray-500 focus:outline-none focus:border-amber-400 transition-colors" onkeydown="if(event.key==='Enter') checkAnswer()">
     </div><button id="submit-btn" onclick="checkAnswer()" class="option-btn w-full bg-gradient-to-l from-amber-500 to-amber-600 text-black font-bold text-xl py-4 rounded-2xl hover:from-amber-400 hover:to-amber-500 shadow-lg shadow-amber-500/30"> تحقق </button>
    </div><!-- Feedback -->
    <div id="feedback" class="hidden mt-6 rounded-2xl p-6 text-center">
     <div id="feedback-icon" class="text-5xl mb-3"></div>
     <p id="feedback-text" class="text-xl font-bold mb-1"></p>
     <p id="feedback-answer" class="text-gray-300"></p><button id="next-btn" onclick="nextQuestion()" class="mt-4 bg-white/10 border border-white/20 text-white font-bold px-8 py-3 rounded-xl hover:bg-white/20 transition-colors"> السؤال التالي ← </button>
    </div>
   </section><!-- Result Screen -->
   <section id="result-screen" class="hidden text-center max-w-lg w-full animate-bounce-in">
    <div id="result-emoji" class="text-8xl mb-6"></div>
    <h2 id="result-title" class="text-3xl font-900 mb-2"></h2>
    <p id="result-subtitle" class="text-gray-400 text-lg mb-8"></p>
    <div id="score-display" class="question-card rounded-3xl p-8 mb-6">
     <div class="text-6xl font-900 bg-gradient-to-l from-amber-300 to-amber-500 bg-clip-text text-transparent mb-2">
      <span id="level-score">٠</span> / <span id="level-questions">٤</span>
     </div>
     <p class="text-gray-400 text-lg">نقاط هذا المستوى</p>
    </div>
    <div class="question-card rounded-3xl p-8 mb-8">
     <p class="text-gray-400 mb-2">إجمالي النقاط</p>
     <div class="text-4xl font-900 bg-gradient-to-l from-emerald-300 to-emerald-500 bg-clip-text text-transparent">
      <span id="total-score">٠</span>
     </div>
    </div>
    <div class="flex gap-4"><button id="next-level-btn" onclick="nextLevel()" class="option-btn flex-1 bg-gradient-to-l from-amber-500 to-amber-600 text-black font-bold text-lg px-8 py-4 rounded-2xl hover:from-amber-400 hover:to-amber-500 shadow-lg shadow-amber-500/30"> <span class="flex items-center gap-2 justify-center"> المستوى التالي <i data-lucide="arrow-left" class="w-5 h-5"></i> </span> </button> <button onclick="goToLevelSelect()" class="option-btn flex-1 bg-white/10 border border-white/20 text-white font-bold text-lg px-8 py-4 rounded-2xl hover:bg-white/20 transition-colors"> <span class="flex items-center gap-2 justify-center"> المستويات <i data-lucide="grid" class="w-5 h-5"></i> </span> </button>
    </div>
   </section>
  </main>
  <footer class="fixed bottom-0 left-0 right-0 text-center py-3 bg-black/40 backdrop-blur text-gray-400 text-sm">
   <p>© جميع الحقوق محفوظة لـ <span class="text-amber-400 font-semibold">حبيبه محمد عشم</span></p>
  </footer>
  <script src="/_sdk/data_sdk.js"></script>
  <script>
    // Comprehensive question database organized by difficulty
    const questionBank = {
      easy: [
        { q: "ما هو أكبر حيوان على الأرض؟", a: "الفيل" },
        { q: "كم عدد أيام السنة الميلادية؟", a: "365" },
        { q: "ما لون العلم المصري؟", a: "أحمر أبيض أسود" },
        { q: "ما هو أكبر محيط في العالم؟", a: "المحيط الهادئ" },
        { q: "ما هو أكبر كوكب في المجموعة الشمسية؟", a: "المشتري" },
        { q: "ما لون السماء في النهار؟", a: "أزرق" },
        { q: "كم عدد قارات العالم؟", a: "7" },
        { q: "أين تقع دولة مصر؟", a: "أفريقيا" },
        { q: "أين يعيش الكنغر؟", a: "أستراليا" },
        { q: "من هو أول رئيس أمريكي؟", a: "جورج واشنطن" },
        { q: "ما هو أكبر حيوان مائي على الأرض؟", a: "الحوت الأزرق" },
        { q: "كم عدد أيام الأسبوع؟", a: "7" },
        { q: "ما هي أكبر دولة من حيث المساحة؟", a: "روسيا" },
        { q: "كم عدد ألوان قوس قزح؟", a: "7" },
        { q: "أين توجد الأهرامات المصرية؟", a: "الجيزة" },
        { q: "ما هو أقرب كوكب من الشمس؟", a: "عطارد" },
        { q: "كم عدد أيام شهر ديسمبر؟", a: "31" },
        { q: "من هو مخترع الكهرباء؟", a: "توماس إديسون" },
        { q: "ما هي أطول أنهار العالم؟", a: "النيل" },
        { q: "كم عدد أصابع اليد الواحدة؟", a: "5" }
      ],
      medium: [
        { q: "ما هو أصغر دولة في العالم؟", a: "الفاتيكان" },
        { q: "كم عدد أيام شهر فبراير في السنة الكبيسة؟", a: "29" },
        { q: "أين يقع برج بيزا المائل؟", a: "إيطاليا" },
        { q: "ما هو العنصر الأخف وزناً في الجدول الدوري؟", a: "الهيدروجين" },
        { q: "كم عدد دول الاتحاد الأوروبي؟", a: "27" },
        { q: "من اخترع الهاتف؟", a: "جرهام بيل" },
        { q: "كم عدد فقرات العمود الفقري للإنسان؟", a: "33" },
        { q: "ما هي عاصمة أستراليا؟", a: "كانبرا" },
        { q: "كم عدد أقمار المريخ؟", a: "2" },
        { q: "ما هو أعلى جبل في إفريقيا؟", a: "كليمنجارو" },
        { q: "من ألف قاموس المحيط؟", a: "الفيروز آبادي" },
        { q: "ما هي عاصمة اليابان؟", a: "طوكيو" },
        { q: "كم عدد أضلاع المكعب؟", a: "12" },
        { q: "ما هي أصغر دولة عربية من حيث المساحة؟", a: "قطر" },
        { q: "من هو مؤسس الدولة الإسلامية؟", a: "محمد" },
        { q: "كم عدد سور الصين العظيم؟", a: "واحد" },
        { q: "ما هي أطول سلسلة جبال في العالم؟", a: "جبال الأنديز" },
        { q: "من هو مؤسس الخوارزمي؟", a: "محمد بن موسى الخوارزمي" },
        { q: "كم عدد أركان الإسلام؟", a: "5" },
        { q: "ما هي أكبر دولة عربية من حيث السكان؟", a: "مصر" }
      ],
      hard: [
        { q: "ما هو أثقل عنصر في الجدول الدوري الطبيعي؟", a: "اليورانيوم" },
        { q: "كم عدد عظام الأذن الداخلية للإنسان؟", a: "3" },
        { q: "ما هي سرعة الضوء تقريباً؟", a: "300000" },
        { q: "من اكتشف قانون الجاذبية؟", a: "نيوتن" },
        { q: "كم عدد عضلات الجسم البشري؟", a: "606" },
        { q: "ما هو أعمق محيط في العالم؟", a: "هادئ" },
        { q: "كم عدد أسنان الحوت الأزرق؟", a: "0" },
        { q: "ما هو الغاز الأكثر وفرة في الجو؟", a: "النيتروجين" },
        { q: "كم عدد عظام الهيكل العظمي للبالغ؟", a: "206" },
        { q: "ما هي أكبر صحراء في العالم؟", a: "الصحراء" },
        { q: "من هو العالم الذي اخترع التلسكوب؟", a: "جاليليو" },
        { q: "كم عدد حروف اللغة العربية؟", a: "28" },
        { q: "ما هو أعمق محيط في العالم؟", a: "المحيط الهادئ" },
        { q: "كم عدد أيام السنة الميلادية في السنة الكبيسة؟", a: "366" },
        { q: "ما هو أكبر بركان في العالم؟", a: "مونا لوا" },
        { q: "كم عدد ألوان العلم الأولمبي؟", a: "5" },
        { q: "ما هي أكبر بحيرة في العالم؟", a: "بحر قزوين" },
        { q: "من هو مخترع الطباعة؟", a: "جوهانس جوتنبرج" },
        { q: "كم عدد أيام السنة الشمسية؟", a: "365" },
        { q: "ما هو أطول نهر في آسيا؟", a: "نهر اليانغتسي" }
      ],
      veryHard: [
        { q: "كم عدد عضلات لسان الإنسان؟", a: "16" },
        { q: "ما هي درجة حرارة سطح الشمس بالمئات؟", a: "5778" },
        { q: "كم عدد أيام السنة الشمسية الدقيقة؟", a: "365.2425" },
        { q: "ما هو أكبر كهف في العالم؟", a: "سون دونج" },
        { q: "كم عدد عروق الدم في جسم الإنسان؟", a: "60000" },
        { q: "ما هو أقدم دستور مكتوب في العالم؟", a: "شريعة حمورابي" },
        { q: "كم عدد نقاط الدماغ البشري تقريباً؟", a: "86000000000" },
        { q: "ما هي درجة غليان الماء؟", a: "100" },
        { q: "كم عدد كواكب المجموعة الشمسية؟", a: "8" },
        { q: "ما هو أكثر عنصر وفرة في القشرة الأرضية؟", a: "الأكسجين" },
        { q: "كم عدد عظام الجمجمة؟", a: "22" },
        { q: "ما هي أكبر جزيرة في العالم؟", a: "جرينلاند" },
        { q: "كم عدد أيام دورة القمر؟", a: "29.5" },
        { q: "ما هو أسرع حيوان برا؟", a: "الفهد" },
        { q: "كم عدد ألوان الطيف المرئي؟", a: "7" },
        { q: "ما هي أصغر وحدة في الحياة؟", a: "الخلية" },
        { q: "كم عدد أنواع الأنسجة البيولوجية الأساسية؟", a: "4" },
        { q: "ما هو أكثر العناصر الكيميائية وفرة في الكون؟", a: "الهيدروجين" },
        { q: "كم عدد أوتار الجيتار؟", a: "6" },
        { q: "ما هي أكثر لغة منطوقة في العالم؟", a: "الإنجليزية" }
      ]
    };

    // Data SDK initialization
    let levelProgress = {}; // Track completed levels
    
    const dataHandler = {
      onDataChanged(data) {
        if (data && Array.isArray(data)) {
          levelProgress = {};
          data.forEach(record => {
            levelProgress[record.level] = record;
          });
          updateLevelButtons();
        }
      }
    };

    (async () => {
      const initResult = await window.dataSdk.init(dataHandler);
      if (!initResult.isOk) {
        console.error('Failed to initialize data SDK');
      }
    })();

    // Generate all 100 levels with progressive difficulty
    const levels = {};
    const generateAllLevels = () => {
      let questionIndex = 0;
      const allQuestions = [
        ...questionBank.easy,
        ...questionBank.medium,
        ...questionBank.hard,
        ...questionBank.veryHard
      ];

      for (let level = 1; level <= 100; level++) {
        let difficulty;
        if (level <= 25) difficulty = questionBank.easy;
        else if (level <= 50) difficulty = questionBank.medium;
        else if (level <= 75) difficulty = questionBank.hard;
        else difficulty = questionBank.veryHard;

        const questionsForLevel = [];
        const numQuestions = Math.min(4 + Math.floor((level - 1) / 20), 6);

        for (let i = 0; i < numQuestions; i++) {
          const idx = (questionIndex + i) % difficulty.length;
          questionsForLevel.push(difficulty[idx]);
        }
        questionIndex += numQuestions;

        levels[level] = questionsForLevel;
      }
    };

    generateAllLevels();

    let currentLevel = 1;
    let currentIndex = 0;
    let score = 0;
    let levelScore = 0;
    let shuffled = [];
    let results = [];

    const toArabicNum = (n
