<h1 dir='rtl'>پرداخت درون برنامه‌ای کیپوپِی</h1>

<h2 dir='rtl'>رویه کلی پرداخت (غیر فنی)</h2>
<p dir='rtl'>
  <b>۱.</b> کتابخانه می‌بایست قبل از فراخوانی دستور پرداخت/انتقال وجه، با استفاده از اطلاعات مورد نیاز، پیکربندی شود. چنانچه در زمان فراخوانی دستور پرداخت/انتقال وجه، الزامات مورد نیاز کتابخانه برآورده نشده باشد، خطای مناسبی به برنامه‌نویس تحویل خواهد شد.
</p>
<p dir='rtl'>
  <b>۲.</b> نرم‌افزار (بنا به شرایط مورد نظر برنامه‌نویس) درخواست پرداخت/انتقال وجه مبلغ مورد نظر را به کتابخانه می‌دهد.
</p>
<p dir='rtl'>
  <b>۳.</b> کتابخانه با استفاده از اطلاعاتی که در مرحله پیکربندی دریافت نموده است، درخواست پرداخت/انتقال وجه را شبیه‌سازی کرده و کاربر را از نرم‌افزار، به مرورگر سافاری منتقل می‌کند؛ تا روند پرداخت/انتقال وجه توسط کاربر ادامه یابد.
</p>
<p dir='rtl'>
  <b>۴.</b> کاربر، در مرورگر، پس از مشاهده اطلاعات پرداخت (مانند مبلغ، دریافت‌کننده و …) اقدام به تصمیم‌گیری جهت ادامه پرداخت، یا انصراف از پرداخت خواهد کرد.
</p>
<p dir='rtl'>
  <b>۵.</b> در صورتی که کاربر اقدام به پرداخت نماید، (پس از انجام امور مربوطه) در نهایت صفحه‌ای به کاربر نمایش داده خواهد شد که علاوه‌بر پیام‌های مناسب (براساس موفقیت یا بروز خطا)، شامل دکمه‌ای برای بازگشت به نرم‌افزار نیز خواهد بود.
</p>
<p dir='rtl'>
  <b>۶.</b> با زدن دکمه بازگشت به نرم‌افزار، اطلاعات حاصل از این تراکنش نیز به نرم‌افزار تحویل داده خواهد شد؛ و برنامه‌نویس براساس اطلاعات دریافتی، اقدامات لازم و مورد نظر خود را ادامه خواهد داد.
</p>
<p dir='rtl'>
  <b>۷.</b> بعدشم، برنامه‌نویس، نرم‌افزار، کاربر، و خود ما، همگی خوش و خوشحال به زندگی‌مون ادامه خواهیم داد
.</p>

<h2 dir='rtl'>روند نصب و پیکربندی</h2>
<p dir='rtl'>
  برای نصب و استفاده از این ابزار، رویه مرسوم در برنامه‌نویسی iOS یا همان استفاده از Cocoapod (یکی از ابزارهای مدیریت وابستگی‌های پروژه) را مورد استفاده قرار دادیم.
</p>
<p dir='rtl'>
  <b>۱.</b> برنامه‌نویسان می‌توانند با استفاده از اضافه‌کردن عبارت <code dir='ltr'>pod 'KIPOIAPKit'</code> به فایل <code dir='ltr'>Podfile</code> پروژه، و نصب وابستگی‌ها با استفاده از دستور <code dir='ltr'>pod install</code>، این کتابخانه را به پروژه اضافه نمایند.
</p>
<p dir='rtl'>
  <b>۲.</b> روند انتقال نتیجه تراکنش‌ها به نرم‌افزارها، با استفاده از Deep Linking انجام خواهد شد. برای اینکار، شناسه نرم‌افزار یا همان Bundle Identifier بعنوان URLScheme مورد استفاده قرار خواهد گرفت؛ تا بدین صورت از غیرتکراری بودن URLScheme اطمینان حاصل شود. برای پیکربندی Deep Linking کد زیر را به فایل <code dir='ltr'>info.plist</code> اضافه می‌کنیم:
</p>

<pre>
&lt;key&gt;CFBundleURLTypes&lt;/key&gt;
&lt;array&gt;
   &lt;dict&gt;
      &lt;key&gt;CFBundleURLName&lt;/key&gt;
      &lt;string&gt;{{APP_BUNDLE_ID}}&lt;/string&gt;
      &lt;key&gt;CFBundleURLSchemes&lt;/key&gt;
      &lt;array&gt;
         &lt;string&gt;{{APP_BUNDLE_ID}}&lt;/string&gt;
      &lt;/array&gt;
   &lt;/dict&gt;
&lt;/array&gt;
</pre>

<p dir='rtl'>
  <b>۳.</b> مرحله بعدی پیکربندی، پیکربندی کتابخانه درون نرم‌افزار خواهد بود:
</p>
<p dir='rtl'>
  <b>۳. ۱.</b> برای استفاده از اطلاعات منتقل‌شده از مرورگر به نرم‌افزار، می‌بایست از متد <code dir='ltr'>Check(_ url:)</code> استفاده نماییم. این متد را در قسمت مربوطه در فایل <code dir='ltr'>AppDelegate</code> قرار خواهیم داد:
</p>

<pre><code>func application(_ application: UIApplication, open url: URL, sourceApplication: String?, annotation: Any) -&gt; Bool {
    return KipoIAP.Check(url)
}
</code></pre>

<p dir='rtl'>
  چنانچه ساختار URL دریافتی این متد، صحیح (براساس ساختار و متغیرهای مورد اتنظار کتابخانه) باشد، براساس اطلاعات دریافتی، یکی از دو حالت زیر اتفاق خواهد افتاد:
</p>
<p dir='rtl'>
  <b>۳. ۱. ۱.</b> چنانچه روند پرداخت بدون بروز خطا به اتمام رسیده باشد، و توکن پرداخت تولید شده باشد، متد زیر فراخوانی خواهد شد:
</p>

<code>KipoIAP.delegate?.kipoPayment(paymentToken:)</code></p>

<p dir='rtl'>
  <b>۳. ۱. ۲.</b> چنانچه در روند پرداخت خطایی بروز نماید، متد زیر بهمراه پیام خطای رخ داده، فراخوانی خواهد شد:
</p>

<code>KipoIAP.delegate?.kipoPayment(errorMessage:)</code></p>

<p dir='rtl'>
  <b>۳. ۲.</b> قدم بعدی، استفاده از متد <code dir='ltr'>Setup(merchantKey:)</code> خواهد بود. ورودی این متد، از نوع <code dir='ltr'>String</code> بوده، و برنامه‌نویس می‌بایست (پیش از فراخوانی درخواست پرداخت/انتقال وجه) شماره همراهی را که با آن در سامانه کیپو ثبت نموده است را بعنوان پارامتر این متد تحویل دهد.
</p>

<h2 dir='rtl'>فراخوانی درخواست پرداخت/انتقال وجه</h2>

<p dir='rtl'>
  <b>۱.</b> برای فراخوانی درخواست پرداخت، برنامه‌نویس ابتدا می‌بایست مشخصه   را مقداردهی نماید. این کار مشابه مقداردهی <code dir='ltr'>delegate</code> و یا <code dir='ltr'>dataSource</code> المان <code dir='ltr'>UITableView</code> انجام خواهد شد. همانگونه که برای استفاده از <code dir='ltr'>UITableView</code>، المانی بعنوان <code dir='ltr'>delegate</code> در نظر گرفته می‌شود، و متدهای اجباری <code dir='ltr'>UITableViewDelegate</code> پیاده‌سازی می‌شوند، در اینجا هم باید مشخصه <code dir='ltr'>delegate</code> مربوط به <code dir='ltr'>KipoIAP</code> مقداردهی شده و متدهای آن نیز پیاده‌سازی شوند؛ با این کار پروتکل <code dir='ltr'>KipoIAPDelegate</code> پیاده‌سازی خواهد شد. این پروتکل دارای سه متد اجباریست:
</p>
<ul dir='rtl'>
  <li>متد <code dir='ltr'>kipoCannotPerform(error:)</code> : زمانیکه انجام فراخوانی پرداخت امکان‌پذیر نباشد، بهمراه خطای مربوطه، در اختیار برنامه‌نویس خواهد بود.</li>
  <li>متد <code dir='ltr'>kipoPayment(paymentToken:)</code> : پس از انجام موفقیت‌آمیز پرداخت، بهمراه توکن پرداخت، در اختیار برنامه‌نویس خواهد بود.</li>
  <li>متد <code dir='ltr'>kipoPayment(errorMessage:)</code> : در صورت بروز خطا در روند پرداخت، بهمراه پیام خطای رخ داده، در اختیار برنامه‌نویس خواهد بود.</li>
</ul>
<p dir='rtl'>
  پیکربندی <code dir='ltr'>delegate</code> نیز با استفاده از متد <code dir='ltr'>Setup(delegate:)</code> صورت می‌پذیرد.
</p>

<p dir='rtl'>
  <b>۲.</b> فراخوانی درخواست پرداخت/انتقال وجه نیز با استفاده از متد <code dir='ltr'>Pay(amount:)</code> انجام می‌شود که تنها یک ورودی، از نوع عددی (<code dir='ltr'>Int</code>) دارد، که مشخص‌کننده مبلغ پرداختی به ریال خواهد بود.
</p>