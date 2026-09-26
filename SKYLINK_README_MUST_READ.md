# SkyLink VPN — ဒီ project ကို ဘာတွေပြောင်းလဲထားလဲ

Original: [2dust/v2rayNG](https://github.com/2dust/v2rayNG) (GPLv3 license)

## ✅ ပြင်ပြီးသား အရာများ

1. **App name**: `v2rayNG` → **`SkyLink VPN`** (strings.xml, notification channel, quick-settings tile)
2. **Application ID**: `com.v2ray.ang` → **`com.skylink.vpn`** (Play Store/Google ကမြင်ရမယ့် ID)
   - Internal Kotlin package name (`com.v2ray.ang`) ကိုတော့ **တမင်မပြောင်းထားပါ** — ဒါက build error
     အန္တရာယ်ကို လျှော့ချဖို့ဖြစ်ပါတယ်။ Play Store ပေါ်မှာ user မမြင်ရတဲ့ internal code path ဖြစ်လို့
     brand ပေါ်မှာ ဘာမှမထိခိုက်ပါဘူး။
3. **Icon/Logo**: အပြာရောင် gradient background + လျှပ်စီး (bolt) icon — launcher icon
   (adaptive + legacy + round, density အားလုံး)၊ notification icon၊ Play Store hi-res icon (512×512)
   အားလုံး ပြောင်းပြီးပါပြီ
4. **Output APK filename / Gradle project name**: `SkyLinkVPN_...apk`

## 🖼️ Logo v2: Shield + Mountain + Lock (dark navy + blue)

User ပေးထားတဲ့ reference ပုံနဲ့ ကိုက်ညီအောင် launcher icon/adaptive icon/notification icon/
Play Store icon အားလုံးကို shield-mountain-swoosh-lock design အသစ်နဲ့ ပြန်ထုတ်ထားပါပြီ။

## ✅ Android Studio လုံးဝမလိုအောင် — GitHub Actions ကနေ APK ကို အလိုအလျောက် build

Repo ထဲမှာ `.github/workflows/build-debug-apk.yml` ဆိုပြီး workflow အသစ်တစ်ခု ထည့်ပေးထားပါတယ်။
ဒါက GitHub ရဲ့ server ပေါ်မှာ Xray-core AAR ဒေါင်းလုဒ်ဆွဲတာ၊ hev-socks5-tunnel native library
compile လုပ်တာ အားလုံးကို **အလိုအလျောက်** လုပ်ပေးပြီး APK ကို ထုတ်ပေးပါတယ် — Android Studio
ဖွင့်စရာ၊ NDK/Go ကိုယ်တိုင် install လုပ်စရာ **လုံးဝမလိုပါဘူး**။ Signing keystore secret လည်း
မလိုပါဘူး (debug APK ထုတ်ပေးတာမို့).

### GitHub ပေါ် တင်နည်း (step-by-step)

1. https://github.com/new သွားပြီး repo အသစ်တစ်ခု create လုပ်ပါ (public/private ရွေးလို့ရပါတယ်)
   — **README/gitignore/license ဘာမှ tick မထားပါနဲ့** (empty repo ဖြစ်ရပါမယ်)
2. ဒီ project ကို git repo အနေနဲ့ ကျွန်တော် ပြင်ဆင်ပြီးသားပါ (`git init` + commit လုပ်ပြီးပါပြီ)။
   Terminal ဖွင့်ပြီး project folder ထဲမှာ **ဒီ command တွေအတိုင်း အစဉ်လိုက် run ပါ**
   (internet ရှိတဲ့ ကွန်ပျူတာပေါ်မှာ run ရပါမယ် — Android Studio/NDK/Go ဘာမှ install
   လုပ်စရာမလိုပါဘူး, git ရှိရုံပါ):
   ```bash
   # 1) Xray-core နဲ့ VPN-tunnel native library ရဲ့ source (submodule) ကို ချိတ်ဆက်ပါ
   git submodule add https://github.com/2dust/AndroidLibXrayLite.git AndroidLibXrayLite
   git submodule add https://github.com/heiher/hev-socks5-tunnel.git hev-socks5-tunnel
   git commit -m "Attach submodules"

   # 2) GitHub repo ကို ချိတ်ပြီး push
   git remote add origin https://github.com/<သင့်-username>/<သင့်-repo-name>.git
   git push -u origin main
   ```
   **ဘာကြောင့် ဒီ step လိုအပ်လဲ**: ဒီ project က AndroidLibXrayLite နဲ့ hev-socks5-tunnel
   ဆိုတဲ့ open-source library ၂ခုပေါ်မှာ မှီခိုနေပါတယ် (Xray VPN protocol core ကို
   အလုပ်လုပ်စေတဲ့ engine ပါ)။ ကျွန်တော့် environment မှာ internet မရှိလို့ အဲဒီ ၂ခုကို
   ကိုယ်စား ဆွဲမပေးနိုင်ပါဘူး — ဒါကြောင့် အပေါ်က command ၂ကြောင်းကို run ပေးရပါမယ်
   (တစ်ခါတည်း၊ ၁ မိနစ်ပဲ ကြာပါတယ်)။ ဒါပြီးရင် GitHub Actions က ကျန်တာအားလုံး
   (compile, build, APK ထုတ်တာ) ကို အလိုအလျောက် လုပ်ပေးပါလိမ့်မယ်။
3. Push ပြီးရင် GitHub repo ရဲ့ **"Actions"** tab ကို သွားကြည့်ပါ — workflow အလိုအလျောက် run
   စပါလိမ့်မယ် (၁၀-၁၅ မိနစ်ခန့် ကြာနိုင်ပါတယ်)
4. Run ပြီးသွားရင် အဲဒီ run ရဲ့ page အောက်ဆုံးက **"Artifacts"** section ထဲမှာ
   `SkyLinkVPN-debug-apk` ဆိုတဲ့ zip ကို download ဆွဲလို့ရပါပြီ — ဖွင့်ရင် APK ပါလာပါမယ်

### Play Store အတွက် signed release APK လိုရင်

`.github/workflows/build.yml` ကတော့ Play Store ပို့ဖို့ signed release APK build ပေးတဲ့
workflow ဖြစ်ပါတယ် — ဒါပေမဲ့ keystore file (`APP_KEYSTORE_BASE64` စတဲ့ GitHub Secrets)
ကို သင့်ဘက်က အရင် generate လုပ်ပြီး repo Settings → Secrets ထဲ ထည့်ပေးရပါမယ်။
အခုအဆင့်မှာတော့ **အသုံးမပြုပါနဲ့** — အလုပ်မလုပ်ဘဲ error ပြနိုင်ပါတယ် (secrets မရှိသေးလို့)။
Testing/install လုပ်ဖို့ဆိုရင် အပေါ်က `build-debug-apk.yml` ကိုပဲ သုံးပါ။

## ⚖️ GPLv3 License — သိထားရမယ့်အချက်

ဒီ code ဟာ **GNU GPLv3** license အောက်မှာရှိပါတယ်။ ဆိုလိုတာက:
- Rebrand လုပ်လို့ရပါတယ်၊ app name/icon ပြောင်းလို့ရပါတယ် — ဒါပေမဲ့
- **Source code ကို public ထုတ်ပြီး GPLv3 အောက်မှာပဲ ဆက်ထားရပါမယ်** — App ကို install/APK အနေနဲ့
  ဖြန့်ဝေမယ်ဆိုရင် သင့် modified source code ကိုပါ တောင်းရင် ပေးရပါမယ် (public GitHub repo ဖြစ်ရင် အလိုအလျောက် ပြည့်မီပါတယ်)
- Original license (`LICENSE` file) ကို ဖျက်လို့မရပါဘူး — copyright notice ကိုလည်း ထိန်းထားရပါမယ်
- LICENSE file ကို ဒီ repo ထဲမှာ ဆက်ထားပေးထားပါတယ် — ဖျက်မထားပါနဲ့

GitHub ပေါ် push တင်ရင် README.md ထဲမှာ "based on 2dust/v2rayNG (GPLv3)" ဆိုပြီး credit
ထည့်ထားပေးဖို့ အကြံပြုပါတယ်။

## GitHub ပေါ် push တင်နည်း

```bash
cd v2rayNG-master
git init
git add .
git commit -m "Initial commit: SkyLink VPN (based on v2rayNG)"
git branch -M main
git remote add origin https://github.com/<သင့်-username>/SkyLinkVPN.git
git push -u origin main
```
