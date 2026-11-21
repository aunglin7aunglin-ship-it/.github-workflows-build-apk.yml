# မြေတိုင်းတာ | Field Survey

Android ကွင်းမှတ်တမ်း App ဖြစ်ပါသည်။ GPS၊ ဓာတ်ပုံ၊ စာရင်းနှင့် မြေကွက်ဖိုင်များကို App ထဲတွင် offline သိမ်းပါသည်။ GPS တိကျမှုသည် ဖုန်းပေါ် မူတည်ပါသည်။ Map သည် ထည့်သွင်းထားသော WGS84 KML/GeoJSON Polygon/MultiPolygon နှင့် GPS points ကိုသာ ပြသပြီး နောက်ခံလမ်းမြေပုံ မပါပါ။ ဧရိယာသည် နယ်နိမိတ်ဖိုင်အပေါ် အခြေခံသည့် ခန့်မှန်းတန်ဖိုးဖြစ်ပါသည်။ Form (၁-၁) PDF သည် တရားဝင်မဟုတ်သော မူကြမ်းဖြစ်ပါသည်။

## Build

GitHub → Actions → **Build Android APK** → Build run → **Artifacts → survey-app-apk** ကို ဒေါင်းလုဒ်လုပ်ပါ။ ZIP ထဲရှိ `app-debug.apk` သည် Android 5.1+ (API 22+) နှင့် အပ်ဒိတ်လုပ်ထားသော Android System WebView လိုပါသည်။ Debug အတွက်သာ ဖြစ်ပြီး Play Store release မဟုတ်ပါ။ GitHub-hosted runner က အသစ်ထုတ်သော debug signing key ကြောင့် နောက် Build တစ်ခုက လက်ရှိ App ပေါ် update မတင်နိုင်ပါက **JSON Backup အရင်ထုတ်ပြီးမှ** app အဟောင်း ဖြုတ်ပါ။

PC တွင် Node.js 22၊ Java 17 နှင့် Android SDK API 34 (Build Tools 34.0.0) ကို ထည့်ပါ။ Project root တွင် `npm ci`, `npm test`, `npm run build`, `npx cap sync android` ကို အစဉ်လိုက် run ပါ။ ပြီးလျှင် `npx cap open android` သို့မဟုတ် `cd android` ပြီး `./gradlew assembleDebug` (Windows တွင် `gradlew.bat assembleDebug`) ကို သုံးပါ။

## Backup နှင့် Sync

- **CSV** သည် Excel အတွက် ဖြစ်ပြီး ဓာတ်ပုံ မပါပါ။ **JSON Backup** တွင် ဓာတ်ပုံ၊ စာရင်းနှင့် မြေကွက်ဖိုင်များပါဝင်ပြီး App ထဲသို့ ပြန်သွင်းနိုင်ပါသည်။ App ကို ဖြုတ်မီ သို့မဟုတ် ဖုန်းပြောင်းမီ JSON Backup ထုတ်ထားပါ။
- **Google Sheets**: သင်ထိန်းချုပ်သည့် Spreadsheet → Extensions → Apps Script တွင် `sheets/Code.gs` ကို ကူးထည့်ပါ။ Project Settings → Script properties တွင် `SURVEY_SPREADSHEET_ID` နှင့် အနည်းဆုံး 24 လုံးရှိ ကျပန်း `SURVEY_SYNC_KEY` သတ်မှတ်ပါ။ Spreadsheet ID သည် Sheet URL ရှိ `/d/` နှင့် နောက် `/` ကြားရှိတန်ဖိုး ဖြစ်ပါသည်။ Deploy → New deployment → Web app → Execute as **Me** ကို ရွေးပါ။ Android App မှ Google login မလုပ်ဘဲ သုံးရန် **Anyone** access လိုပါသည်။ Workspace policy က ဟန့်တားပါက သင်အတည်ပြုထားသော HTTPS server သုံးပါ။ `/exec` URL နှင့် တူညီသော Sync key ကို App တွင် ဖြည့်ပါ။ Secret ကို repository ထဲ မတင်ပါနှင့်။
- **သီးခြား server**: HTTPS endpoint သည် `POST {schema:1,key,records:[...]}` ကိုလက်ခံ၊ key ကိုစစ်ပြီး `{ok:true,accepted:<count>}` ပြန်ပေးရပါမည်။ Sync သည် ဖုန်းမှသာ တစ်ဖက်သတ်ပို့ပြီး Record ID ဖြင့် ထပ်နေသောစာရင်းများကို update လုပ်ပါသည်။ ဖုန်းထဲက ဖျက်ခြင်းသည် Sheet မှ ဖျက်ခြင်းမဟုတ်ပါ။ GPS နှင့် ဦးပိုင်အချက်အလက်ကို ယုံကြည်ရသော endpoint သို့သာ ပို့ပါ။
