# 📚 دليل استخدام الـ API - فريق الويب والموبيل

## 🔗 الـ Base URL:
```
https://study-assistant-api-xxxxx.onrender.com
```

---

## 📍 الـ Endpoints المتاحة:

### 1️⃣ **Health Check** (للتحقق من حالة الخادم)
```
GET /health
```

**الرد:**
```json
{
  "status": "ok",
  "mistral_initialized": true,
  "timestamp": "2026-02-20T05:41:44.797196"
}
```

---

### 2️⃣ **استخراج النصوص من الملفات**
```
POST /extract
```

**المتطلبات:**
- `file` (multipart/form-data) - الملف (PDF, Word, PowerPoint, صورة)

**الرد:**
```json
{
  "filename": "document.pdf",
  "text": "محتوى النص المستخرج...",
  "metadata": {...},
  "word_count": 250,
  "character_count": 1500,
  "chunks_count": 5
}
```

**مثال JavaScript:**
```javascript
const formData = new FormData();
formData.append('file', fileInput.files[0]);

const response = await fetch(
  'https://study-assistant-api-xxxxx.onrender.com/extract',
  { method: 'POST', body: formData }
);
const data = await response.json();
console.log(data.text);
```

**مثال Python:**
```python
import requests

with open('document.pdf', 'rb') as f:
    files = {'file': f}
    response = requests.post(
        'https://study-assistant-api-xxxxx.onrender.com/extract',
        files=files
    )
    print(response.json()['text'])
```

---

### 3️⃣ **توليد الملخص**
```
POST /summary
```

**المتطلبات:**
```
text: str (النص المراد تلخيصه)
language: str (en أو ar)
```

**الرد:**
```json
{
  "summary": "ملخص النص...",
  "language": "en",
  "generated_at": "2026-02-20T05:41:44"
}
```

**مثال React:**
```javascript
const generateSummary = async (text) => {
  const formData = new FormData();
  formData.append('text', text);
  formData.append('language', 'en');
  
  const response = await fetch(
    'https://study-assistant-api-xxxxx.onrender.com/summary',
    { method: 'POST', body: formData }
  );
  return response.json();
};
```

---

### 4️⃣ **توليد الأسئلة (Quiz)**
```
POST /quiz
```

**المتطلبات:**
```
text: str (النص)
num_questions: int (عدد الأسئلة، افتراضي: 10)
language: str (en أو ar)
```

**الرد:**
```json
{
  "questions": [
    {
      "question": "السؤال؟",
      "options": ["خيار 1", "خيار 2", "خيار 3", "خيار 4"],
      "correct_answer": 1,
      "explanation": "الشرح..."
    }
  ],
  "error": null
}
```

**مثال Flutter/Dart:**
```dart
Future<List> generateQuiz(String text, int numQuestions) async {
  final response = await http.post(
    Uri.parse('https://study-assistant-api-xxxxx.onrender.com/quiz'),
    body: {
      'text': text,
      'num_questions': numQuestions.toString(),
      'language': 'en',
    },
  );
  
  if (response.statusCode == 200) {
    return jsonDecode(response.body)['questions'];
  }
  throw Exception('Failed to load quiz');
}
```

---

### 5️⃣ **توليد الخريطة الذهنية**
```
POST /mindmap
```

**المتطلبات:**
```
text: str (النص)
language: str (en أو ar)
```

**الرد:**
```json
{
  "mindmap": "محتوى الخريطة الذهنية بصيغة مرئية...",
  "language": "en",
  "generated_at": "2026-02-20T05:41:44"
}
```

---

### 6️⃣ **بنك الأسئلة الشامل**
```
POST /question-bank
```

**المتطلبات:**
```
text: str (النص)
num_questions: int (عدد الأسئلة)
language: str (en أو ar)
```

**الرد:**
```json
{
  "questions": [
    {
      "question": "السؤال؟",
      "answer": "الإجابة...",
      "difficulty": "easy|medium|hard"
    }
  ],
  "total_count": 20,
  "generated_at": "2026-02-20T05:41:44"
}
```

---

### 7️⃣ **توليد جميع الميزات دفعة واحدة**
```
POST /generate-all
```

**المتطلبات:**
```
file: File (الملف المراد معالجته)
language: str (en أو ar)
```

**الرد:**
```json
{
  "filename": "document.pdf",
  "text_hash": "abc123...",
  "features": {
    "en": {
      "summary": "...",
      "quiz": {...},
      "mindmap": "...",
      "question_bank": {...}
    },
    "ar": {...}
  },
  "generated_at": "2026-02-20T05:41:44"
}
```

---

### 8️⃣ **المحادثة حول المستند (Chat)**
```
POST /chat
```

**المتطلبات:**
```
filename: str (اسم الملف)
user_message: str (السؤال)
context: str (محتوى الملف)
language: str (en أو ar)
```

**الرد:**
```json
{
  "response": "إجابة الذكاء الاصطناعي...",
  "filename": "document.pdf",
  "generated_at": "2026-02-20T05:41:44"
}
```

**مثال TypeScript:**
```typescript
async function askQuestion(filename: string, question: string, context: string) {
  const response = await fetch(
    'https://study-assistant-api-xxxxx.onrender.com/chat',
    {
      method: 'POST',
      body: new FormData({
        filename,
        user_message: question,
        context,
        language: 'en'
      })
    }
  );
  return response.json();
}
```

---

### 9️⃣ **Batch Chat (محادثات متعددة)**
```
POST /batch-chat
```

**المتطلبات:**
```
filename: str (اسم الملف)
context: str (محتوى الملف)
messages: List[{role, content}] (قائمة الرسائل)
language: str (en أو ar)
```

**الرد:**
```json
{
  "messages": [
    {
      "role": "user",
      "content": "السؤال الأول"
    },
    {
      "role": "assistant",
      "content": "الإجابة..."
    }
  ],
  "generated_at": "2026-02-20T05:41:44"
}
```

---

## 🔐 معلومات الأمان:

✅ **HTTPS مفعّل تلقائياً**
✅ **CORS مفعّل** (يمكن الاستدعاء من أي نطاق)
✅ **API Key محفوظ بأمان** في متغيرات البيئة
✅ **لا توجد بيانات شخصية مخزنة**

---

## ⚙️ إعدادات Limits:

```
max_file_size: 50 MB
max_text_length: 100,000 حرف
timeout: 60 ثانية
rate_limit: بدون تحديد (الآن)
```

---

## 🆘 معالجة الأخطاء:

**رموز الأخطاء الشائعة:**

| الرمز | المعنى | الحل |
|---|---|---|
| 400 | طلب غير صحيح | تحقق من المتطلبات |
| 422 | بيانات غير صالحة | تحقق من format البيانات |
| 500 | خطأ في الخادم | جرب لاحقاً |
| 503 | الخادم غير متاح | انتظر قليلاً |

**مثال معالجة الأخطاء:**
```javascript
try {
  const response = await fetch(api_url, options);
  if (!response.ok) {
    const error = await response.json();
    console.error('Error:', error.detail);
  }
  return response.json();
} catch (error) {
  console.error('Network error:', error);
}
```

---

## 📊 أمثلة استخدام عملية:

### **Web App (React)**
```javascript
import axios from 'axios';

const api = axios.create({
  baseURL: 'https://study-assistant-api-xxxxx.onrender.com'
});

// استخراج نصوص
const extractText = async (file) => {
  const formData = new FormData();
  formData.append('file', file);
  return api.post('/extract', formData);
};

// توليد ملخص
const getSummary = async (text, lang = 'en') => {
  const formData = new FormData();
  formData.append('text', text);
  formData.append('language', lang);
  return api.post('/summary', formData);
};
```

### **Mobile App (Flutter)**
```dart
import 'package:http/http.dart' as http;

class StudyAssistantAPI {
  final String baseUrl = 'https://study-assistant-api-xxxxx.onrender.com';
  
  Future<Map> generateQuiz(String text, int numQuestions) async {
    final response = await http.post(
      Uri.parse('$baseUrl/quiz'),
      body: {
        'text': text,
        'num_questions': numQuestions.toString(),
        'language': 'en',
      },
    );
    
    if (response.statusCode == 200) {
      return jsonDecode(response.body);
    }
    throw Exception('Failed to generate quiz');
  }
}
```

---

## 📞 الدعم والاستفسارات:

للأسئلة والاستفسارات:
- تواصل مع فريق الـ Backend Development
- اطلب من الـ API Developer إضافة ميزة جديدة

---

**آخر تحديث: 2026-02-20**
