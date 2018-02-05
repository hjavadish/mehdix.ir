---
title: بیلد سریع‌تر وبسایت
uuid: 8ec3f320-a7ce-487c-a9c0-8d3043812dff
tags: بیلد روبی کشینگ جکیل پروفایلینگ
---
وبسایت من در حال حاضر حاوی ۵۶ یادداشت است. لپ‌تاپ Core i5 من حدود ۱۲ ثانیه زمان نیاز دارد تا وبسات را بیلد کند. با انجام تغییراتی این زمان را به حدود ۳ ثانیه کاهش دادم.

قبلا ‏[نوشته][نوشته‌قبلی] بودم که با تغییراتی به کمک پروفایلر بیلد را سریع‌تر کرده‌ام. اینبار تغییرات زیربنایی‌تری دادم که بیلد از آن هم سریع‌تر بشود. چرا که بیلد وبسایتم با همه دنگ و فنگش به حدود ۱۲ ثانیه رسیده بود و مطمئنم اگر در حال خواندن این متن هستید تصدیق خواهید کرد که چنین سرعتی اصلا قابل قبول نیست!

## چه چیزی کند بود؟
دو چیز:
1. [پلاگینی][پلاگین‌قدیمی] که برای تولید تگ‌ها و تگ‌کلاود استفاده می‌کردم.
2. دستورات `include` درون قالب.

## فورک پلاگین و حل مشکل کندی آن
دفعه گذشته که روی سایت کار کردم به کمک پروفایلر متوجه شدم که این پلاگین خیلی کند است. از جایی که کارش را به خوبی انجام می‌دهد و پلاگین بهتری هم پیدا نکردم تصمیم گرفتم بهبودش بدهم. دو فکر به ذهنم رسید. یکی اینکه الگوریتم استفاده شده در پلاگین را تغییر بدهم و دیگر اینکه سعی کنم دستورات تکراری را ذخیره کنم.

از جایی که من یک برنامه‌نویس مبتدی روبی هستم و با مقداری جستجو به راه حلی برای بهبود فوری نرسیدم، بهبود کد و الگوریتم پلاگین کار پر دردسری به نظرم رسید. البته چیزهایی دستگیرم شد. مثلا [این خط][خط‌کد] بسیار کند اجرا می‌شد و در متن فارسی اصلا کاربردی ندارد (بجز جایگزین کردن خط فاصله با خط تیره. روش بعدی بهتر بود.

با مقدار بیشتری کند و کاو متوجه شدم وقت‌گیرترین عملیاتی که این پلاگین انجام می‌دهد تولید تگ‌کلاود است. سایت من طوری است که در صفحه‌ای که برای هر تگ تولید می‌شود این تگ‌کلاود یکبار کپی می‌شود. متوجه شدم که این عملیات بارها تکرار می‌شود و نتیجه هر بار همان است. فقط کافی بود که نتیجه را ذخیره و دوباره استفاده کنم. همینکار را هم [کردم][تگ‌کلاود]:

    @@tag_cloud = nil

    def tag_cloud(site)
      @@tag_cloud ||= active_tag_data.map { |tag, set|
          tag_link(tag, tag_url(tag), :class => "set-#{set}")
        }.join(' ')
    end

یک متغیر استاتیک رد ماژولی روبی داخل پلاگین تعریف کردم تا فقط بار اول تگ‌کلاود را حساب کند و دفعات بعدی همین را بازگرداند. همین تغییر حدود هفت تا هشت ثانیه روی دستگاه من در زمان بیلد صرفه‌جویی کرد.

### انتشار به صورت پلاگین مجزا
من دوست داشتم این تغییر را به پلاگین اصلی اضافه کنم. متاسفانه به [سوالی] که پرسیده بودم کسی جواب نداد، از طرفی پول ریکوئست‌های مرج نشده و سوالات بی‌پاسخ پیامش روشن بود، کسی وقت رسیدگی به این پروژه را ندارد. بنابراین من پروژه را فورک کردم و تحت نام [`jekyll-tagging-lite`](https://github.com/mehdisadeghi/jekyll-tagging-lite) منتشر کردم و از همین جم در تم وبسایت استفاده کردم.

## پرهیز از `include`های تکراری
هنوز هم جا داشت که بیلد سریع‌تر شود. خوشبختانه برای این قسمت قبلا کسی یک پلاگین نوشته است. بسیاری از `include`های استفاده شده در قالب تکراری هستند. پلاگینی بنام [`jekyll-include-cache`](https://github.com/benbalter/jekyll-include-cache) را به تم اضافه کردم و `inlcude`هایی که بعد از اولین بیلد داده‌شان تکراری است را با کمک این پلاگین کش کردم. مثلا فوتر صفحه همه‌جا تکراری است بنابراین فقط کافیست یکبار بیلد شود:

{% raw %}
    {%- include_cached footer.html -%}
{% endraw %}

## از این هم سریع‌تر می‌شود؟
حتما می‌شود. در آینده باز هم سعی خواهم کرد بیلد را سریع‌تر کنم. آنجا که منابع و زمان محدود می‌شود چیزهای بسیاری برای یاد گرفتن هست. چه بسا زبان مورد استفاده را عوض بکنم، الگوریتم‌ها را تغییر بدهم یا تا می‌توانم بی‌رحمانه دستورالعمل‌های عبث را حذف کنم تا سرعت را افزایش بدهم. دوست دارم وبسایت در چشم برهم زدنی بلید بشود. حداقل زیر یک ثانیه. الان رسیده است به ۳.۴۸۱ ثانیه.

این قصه سر دراز دارد. باز هم در این باره خواهم نوشت.



[نوشته‌قبلی]: merciless-simplification.html
[پلاگین‌قدیمی]: https://github.com/pattex/jekyll-tagging
[خط‌کد]: https://github.com/pattex/jekyll-tagging/blob/master/lib/jekyll/tagging.rb#L15
[تگ‌کلاود]: https://github.com/mehdisadeghi/jekyll-tagging-lite/blob/master/lib/jekyll-tagging-lite.rb#L121
[سوالی]:https://github.com/pattex/jekyll-tagging/issues/71