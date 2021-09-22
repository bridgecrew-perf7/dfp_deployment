
<div dir="rtl" >
<H1>Deployment</H1>
 
 تا به این جای کار  روی سیستم شخصی به عنوان محیط توسعه کار می‌کردیم. حال زمان آن فرا رسیده که پروژه را برای دسترسی عمومی ( دسترسی  از طریق اینترنت) نیز دیپلوی کنیم. عنوان دیپلوی موضوع گسترده ای می‌باشد، به گونه ای  که می‌توان در کتاب جداگانه‌ای مفصل به توضیح آن پرداخت. در مقایسه با دیگر فریم ورک‌ها، جنگو در این موضوع بسیار کاربردی می‌باشد. امکان دیپلوی اپلیکیشن جنگو تنها با یک کلیک رو  اکثر پلتفرم‌های هاستینگ وجود نداشته و همین دلیل نیازمند کار بیشتر سمت توسعه دهندگان می‌باشد، با این حال امکان سفارشی سازی بالایی را  در مقایسه با روش معمول در جنگو برای ما فراهم می‌آورد.
در فصل قبل با پیکره بندی کامل فایل docker-compose-prod.yml به صورت جداگانه، همچنین به روزرسانی فایل config/setting.py‌، اپلیکیشن جنگو را آماده قرار گیری در محیط پروداکشن کردیم.
در این فصل به ترتیب اقدام به بررسی نحوه انتخاب هاستینگ، اضافه کردن وب سرور  برای محیط پروداکشن و همچنین اطمینان از پیکره‌بندی فایل‌های static/media  ، قبل از استقرار فروشگاه کتاب آنلاین، خواهیم نمود.

 <h2> رایانش ابری و پلتفرم ابری </h2>

اولین سوال این است که از پلتفرم ابری استفاده کنیم یا رایانش ابری.
 پلتفرم ابری یکی از گزینه‌های هاستینگ معتبر می‌باشد، که اکثر پیکره‌بندی های اولیه آن انجام شده و امکان مقیاس پذیری را نیز برای سایت شما فراهم می‌کند. از سرویس دهنده‌های مشهور پلتفرم ابری،‌می‌توان به Heroku،PythonAnyWhere و Dokku در بین دیگر سرویس دهنده ها اشاره کرد.
 اگرچه پلتفرم ابری در مقایسه با رایانش ابری دارای هزینه بیشتری می‌با‌شد ولی این سرویس زمان زیادی از توسعه دهندگان را صرفه‌جویی می‌کند.
 در مقابل پلتفرم ابری،  رایانش ابری وجود دارد که اگر چه انعطاف پذیری بیشتری نسبت به پتلفرم ابری داشته و همچنین هزینه کمتری دارد، اما نیازمند دانش فنی بالا و همچنین اطمینان کافی از نصب و راه اندازی را دارد. از سرویس دهندگان رایانش ابری هم می‌توان به DigitalOcean، Linode، Amazon EC2 و Google Compute Engine اشاره کرد.
 
 حال باید از کدام استفاده کرد؟ توسعه دهندگان جنگو تمایل دارند از یکی از این دو سرویس را استفاده کنند: اگر آن ها اقدام به دیپلوی پایپ لاین روی سرویس رایانش ابری کرده باشند، انتخاب آن‌ها رایانش ابری خواهد بود، در غیر این صورت سراغ پلتفرم ابری خواهند رفت. 
 <div dir="rtl">
 از آنجایی که استفاده از سرویس رایانش ابری پیچیده بوده و نیازمند پیکره‌بندی‌های زیادی می‌باشد، ما در اینجا از پلتفرم ابری Heroku استفاده خواهیم کرد.
 اگر چه  پلتفرم ابری Heroku یک تکنولوژی بالغ بوده و همچنین این سرویس دهنده دارای پلن رایگان مناسب برای فروشگاه کتاب آنلاین ما می‌باشد، اما انتخاب  Heroku یک موضوع اختیاری می‌باشد.
 
  <h2> White Noise </h2>
در توسعه لوکال، جنگو بر اساس staticfiles app اقدام به جمع آوری و ارائه فایل های استاتیک در سراسر پروژه می‌کرد،  اگر چه راه حل آسانی می‌باشد ولی کاربردی و امن نیست.
برای محیط پروداکشن باید collectstatic اجرا شود، تا تمامی فایل‌های استاتیکی که توسط STATIC_ROOT مشخص شده اند را در یک دایرکتوری مشخص کامپایل کند.
با به روزرسانی STATICFILES_STORAGE می‌توان آن ها را در یک سرور واحد در کنار اپلیکیشن جنگو  یا یک سرورجداگانه و یا همچنین سرویس های ابری مانند CDN، ارائه داد.
در این پروژه ما فایل‌های سرور خود را به کمک WhiteNoise ارائه خواهیم داد، که این پکیج کاملا با سرویس Heroku سازگار بوده و سرعت بسیار بیشتری نسبت به جنگو در حالت عادی به ما ارائه می‌دهد.
در محله اول اقدام به نصب پکیج whitenoise در داکر و همچنین اقدام به توقف کانتینر خواهیم کرد:
 </div>
  <div dir="ltr">
 <hr>
$ docker-compose exec web pipenv install whitenoise==5.1.0 <br>
$ docker-compose down <br>
 <hr>
 </div>
تا انجام تغییرات در تنظیمات اپلیکیشن جنگو، ما اقدام به ساخت دوباره کانتینر از ایمیج اپلیکیشن نخواهیم کرد.
تا زمانی که که از داکر استفاده می‌کنیم، امکان تغییر به whitenoise به صورت لوکالی همانند محیط پروداکشن را داریم. در حالی که  می‌توان با ست کردن آرگومان nostatic-- این کار را انجام داد،‌ولی در عمل این کار خسته کننده خواهد بود. رویکرد بهتر این می‌باشد که در پیکره بندی INSTALLED_APPS  دستور whitenoise.runserver_nostatic را قبل از django.contrib.staticfiles  اضافه کنیم.
  ما همچنین برای استفاده از whitenoise پیکره بندی ‌های  آن را در بخش MIDDLEWARE دقیقا پایین خط SecurityMiddleware  و  بخش STATICFILES_STORAGE اضافه می ‌کنیم.
 <div dir ="ltr">
 <hr>
 config/settings.py <br>
INSTALLED_APPS = [ <br>
'django.contrib.admin',<br>
'django.contrib.auth',<br>
'django.contrib.contenttypes',<br>
'django.contrib.sessions',<br>
'django.contrib.messages',<br>
'whitenoise.runserver_nostatic', # new <br>
'django.contrib.staticfiles', <br>
'django.contrib.sites', <br>
... <br>
] <br>
MIDDLEWARE = [ <br>
'django.middleware.cache.UpdateCacheMiddleware', <br>
'django.middleware.security.SecurityMiddleware', <br>
'whitenoise.middleware.WhiteNoiseMiddleware', # new <br>
... <br>
] <br>
STATICFILES_STORAGE = <br>
'whitenoise.storage.CompressedManifestStaticFilesStorage' # new <br>
  <hr>
   </div>
  <div dir="rtl" >
 
  با انجام تغییرات، ما می‌توانیم پروژه را در حالت توسعه به صورت لوکال اجرا کنیم.
  </div>
<div dir="ltr">
 <hr>
 
$ docker-compose up -d --build <br>
 
 <hr>
  </div>
پکیج whitenoise دارای ویژگی‌های خاص برای ارائه محتوا ‌ به صورت فشرده  و همچنین دارای هدرهایی برای کشینگ محتوا داشته، که تغییر نخواهند کرد. اما الان یک بار دیگه دستور collectstatic را مجدد اجرا می‌کنیم:

<div dir="ltr" >
 <hr>
 
$ docker-compose exec web python manage.py collectstatic <br>
 
 <hr>
 </div>
بعد از اجرای دستور بالا یک اخطار در مورد بازنویسی فایل ‌های موجود نمایش داده خواهد شد، که مشکلی از این بابت وجود ندارد. yes را  تایپ کرده و روی دکمه Enter کلیک کنید تا ادامه یابد.
 <h2> Media Files  </h2>  
   متاسفانه <a href="http://whitenoise.evans.io/en/stable/django.html#serving-media-files">white-noise </a> عملکرد خوبی در مواجه با فایل‌های چندرسانه‌ای آپلود شده توسط کاربران ندارد. کاورهای کتاب ما توسط جنگو ادمین اضافه شده است ولی عمکلرد آن شبیه به فایل های آپلود شده توسط کاربران می‌باشد. در نتیجه، در حالی که در توسعه لوکال به صورت دلخواه ظاهر می‌شوند، ولی در تنظیمات محیط پروداکشن  ظاهر نمی‌شوند.
رویکرد پیشنهادی استفاده از پکیج محبوب django-storage در کنار یک CDN اختصاصی مانند S3 می‌باشد.  این رویکرد نیازمند پیکره‌بندی‌های اضافی که توضیحات مربوط به آن، خارج از محدوده کتاب می‌باشد.  
 
  <h2> Gunicorn  </h2>  
  وقتی ما در فصل سوم دستور startproject را اجرا کردیم، فایل wsgi.py با پیکره بندی‌ های پیش فرض WGSI (Web Server Gateway Interface) ساخته شد. این مشخصات برای ارتباط بین وب اپ ساخته شده (مانند پروژه فروشگاه کتاب آنلاین) با وب سرور استفاده می‌گردد.
برای محیط پروداکشن رایج است که از Gunicorn یا  uWSGI به جایWGSI پیش‌فرض استفاده کنیم، هر دو این‌ها باعث بهبود عمکلرد می‌گردند ولی به دلیل سادگی در پیاده سازی انتخاب ما Gunicorn  می‌باشد.

مرحله اول نصب پکیج Gunircorn در پروژه و سپس متوقف کردن کانتینرها می‌باشد:
   
 <div dir="ltr" >
  
  <hr>
  $ docker-compose exec web pipenv install gunicorn==20.0.4 <br>
$ docker-compose down
<hr>
  
 </div>
 
 از آنجا که ما از داکر استفاده می‌کنیم، محیط لوکال ما می‌توانید نقش محیط پروداکشن را به خوبی ایفا کند،‌ بنابراین ما هر دو فایل docker-compose.yml و docker-compose-prod.yml را برای استفاده از Gunicorn به جای وب سرورقبلی، به روز رسانی می‌کنیم.

  <div dir="ltr" >
  <b> docker-compose.yml </b>
  <hr>
   # command: python /code/manage.py runserver 0.0.0.0:8000 <br>
    command: gunicorn config.wsgi -b 0.0.0.0:8000 # new <br>
   <hr>
   
   <b> docker-prod-compose.yml </b>
  <hr>
   # command: python /code/manage.py runserver 0.0.0.0:8000 <br>
    command: gunicorn config.wsgi -b 0.0.0.0:8000 # new <br>
   <hr>

 </div>
 حال مجدد کانتینر را اجرا می‌کنیم تا دوباره ایمیج جدید به همراه پکیج Gunicorn و همچنین متغیرهای محلی ساخته شود.
 
 
  
</div>
