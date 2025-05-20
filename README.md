# AmootSMS
## سامانه پیامک آموت
پیامک آموت (پیشرفته‌ترین سامانه پیامکی ایران) یک سامانه دارای وب سرویس کامل برای ارسال و دریافت پیامک و مدیریت کامل خدمات دیگر است که براحتی میتوانید از آن استفاده کنید.

آدرس وب سایت
https://www.amootsms.com

آدرس پرتال
https://portal.amootsms.com

آدرس مستندات وب سرویس 
https://portal.amootsms.com/documentation

## وب سرویس پیامک آموت - REST (REST)
آدرس وب سرویس https://portal.amootsms.com/rest

### متد وضعیت حساب کاربری (AccountStatus)
از طریق این متد می توانید تمامی اطلاعات حساب کاربری خود از قبیل اعتبار باقیمانده و موجود در پنل ، تعرفه های ارسال و لیست خطوط را دریافت نمایید .
#### پارامترهای ورودی AccountStatus
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|Token|String *|توکن ثبت شده در سامانه پیامک آموت|
#### خروجی AccountStatus (Object)
```html
AccountName => نام حساب کاربری
RemaindCredit => باقیمانده اعتبار به ریال

BaseSMS_PersianPrice => تعرفه هر صفحه پیامک پایه فارسی
BaseSMS_EnglishPrice => تعرفه هر صفحه پیامک پایه انگلیسی

ServiceSMS_PersianPrice => تعرفه هر صفحه پیامک خدماتی فارسی
ServiceSMS_EnglishPrice => تعرفه هر صفحه پیامک خدماتی انگلیسی

AdsSMS_PersianPrice => تعرفه هر صفحه پیامک تبلیغاتی فارسی
AdsSMS_EnglishPrice => تعرفه هر صفحه پیامک تبلیغاتی انگلیسی

Avanak_SecondPrice => تعرفه هر ثانیه پیام صوتی آوانک

ListLineNumbers => {
شماره خطوط,
}
```
#### نمونه کد AccountStatus (C#)
```csharp
string Token = "MyToken";
using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

    var data = new System.Collections.Specialized.NameValueCollection()
    {

    };

    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/AccountStatus", data);

    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes);//خروجی
}
```
#### نمونه کد AccountStatus (PHP)
```php
$url = "https://portal.amootsms.com/rest/AccountStatus";

$url = $url."?"."Token=".urlencode("MyToken");
$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;


```
### متد ارسال پیامک (SendSimple)
از طریق این متد می توانید با وارد کردن متن  و شماره  ، پیامک های خود را به شماره یا شماره های انتخابی ارسال نمایید.
#### پارامترهای ورودی SendSimple
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|Token|String *|توکن ثبت شده در سامانه پیامک آموت|
|SendDateTime|Object *|زمان ارسال پیامک|
|SMSMessageText|String *|متن پیامک|
|LineNumber|String *|شماره خط|
|Mobiles|Object *|لیست موبایل های دریافت کنندگان پیامک|
#### خروجی SendSimple (Object)
```html
result.Data شامل آرایه ای از یک کلاس با مقادیر
 {
Status = وضعیت ,
MessageID = کد پیامک (کد شماره کمپین) ,
Mobile = شماره موبایل
}
```
#### نمونه کد SendSimple (C#)
```csharp
string Token = "MyToken";
DateTime SendDateTime = DateTime.Now;
string SMSMessageText = "پیامک تستی من";
string LineNumber = "public";
string[] Mobiles = new string[]
{
    "9120000000",
    "9150000000",
};


using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

    var data = new System.Collections.Specialized.NameValueCollection()
    {
        { "SendDateTime", SendDateTime.ToString() },
        { "SMSMessageText", SMSMessageText  },
        { "LineNumber", LineNumber },
        { "Mobiles", string.Join(",",Mobiles)  },
    };

    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/SendSimple", data);

    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes);//خروجی
}
```
#### نمونه کد SendSimple (PHP)
```php
$url = "https://portal.amootsms.com/rest/SendSimple";

$url = $url."?"."Token=".urlencode("MyToken");
$nowIran = new DateTime('now', new DateTimeZone('IRAN'));
$url = $url."&"."SendDateTime=".urlencode($nowIran->format('c'));

$url = $url."&"."SMSMessageText=".urlencode("پیامک تستی من");
$url = $url."&"."LineNumber=public";

$url = $url."&"."Mobiles=9120000000,9150000000";

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;

```
#### نمونه کد SendSimple (NodeJS)
```nodejs
///////////// Method GET =>

const https = require('https');

var url = 'https://portal.amootsms.com/rest/SendSimple';
url += '?Token='+encodeURIComponent('MyToken');
url += '&SendDateTime=2020-01-01 12:00:00';
url += '&SMSMessageText='+encodeURIComponent('تست');
url += '&LineNumber=public';
url += '&Mobiles=09159999999,09129999999';


https.get(url, (resp) => {
    let data = '';

    // A chunk of data has been received.
    resp.on('data', (chunk) => {
        data += chunk;
    });

    // The whole response has been received. Print out the result.
    resp.on('end', () => {
        console.log(JSON.parse(data).explanation);
    });

}).on("error", (err) => {
    console.log("Error: " + err.message);
});


/////////// Method POST =>


const https = require('https');

var postData = JSON.stringify({
    'Token': 'MyToken',
    'SendDateTime': '2020-01-01 12:00:00',
    'SMSMessageText': 'تست',
    'LineNumber': 'public',
    'Mobiles': '09159999999,09129999999'
});

var options = {
    hostname: 'portal.amootsms.com',
    port: 443,
    path: '/rest/SendSimple',
    method: 'POST',
    headers: {
        'Content-Type': 'application/x-www-form-urlencoded',
        'Content-Length': postData.length
    }
};

var req = https.request(options, (resp) => {
    console.log('statusCode:', resp.statusCode);
    console.log('headers:', resp.headers);

    let data = '';

    // A chunk of data has been received.
    resp.on('data', (chunk) => {
        data += chunk;
    });

    // The whole response has been received. Print out the result.
    resp.on('end', () => {
        console.log(JSON.parse(data).explanation);
    });
});

req.on('error', (e) => {
    console.error(e);
});

req.write(postData);
req.end();
```
### متد ارسال سریع کد اعتبارسنجی (SendQuickOTP)
از طریق این متد می توانید پیامک با رمز یکبار مصرف را به صورت فوری به شماره انتخابی خود ارسال نمایید ، جهت استفاده از این متد باید یک الگو با متغیر code در پنل کاربری ایجاد نمایید .

نکته : شما باید الگوی پیامک خود را در پنل کاربری پیامک ----> گزینه ی امکانات ---> گزینه ی پیامکهای با الگو ثبت  و گزینه otp را فعال نمایید ، پس از تایید توسط اپراتور مرکز مخابرات و فعال شدن ، میتوانید ارسال داشته باشید.
#### پارامترهای ورودی SendQuickOTP
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|Token|String *|توکن ثبت شده در سامانه پیامک آموت|
|Mobile|String *|موبایل دریافت کننده پیامک|
|CodeLength|Short *|طول کد (حداقل 4 رقم و حداکثر 8 رقم)|
|OptionalCode|String *|رمز یکبار مصرف سفارشی خود اگر خالی باشد رمز یکبار مصرف سمت سرور تولید می شود|
#### خروجی SendQuickOTP (Object)
```html
result.Data شامل آرایه ای از یک کلاس با مقادیر
{
Status = وضعیت ,
MessageID = کد پیامک (کد شماره کمپین) ,
Mobile = شماره موبایل
Code = رمز یکبار مصرف
}
```
#### نمونه کد SendQuickOTP (C#)
```csharp
string Token = "MyToken";
string Mobile = "9120000000";
short CodeLength = 4;
string OptionalCode = "";


using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

    var data = new System.Collections.Specialized.NameValueCollection()
    {
        { "Mobile", Mobile  },
        { "CodeLength", CodeLength.ToString() },
        { "OptionalCode", OptionalCode.ToString() },
    };

    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/SendQuickOTP", data);

    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes);//خروجی
}
```
#### نمونه کد SendQuickOTP (PHP)
```php
$url = "https://portal.amootsms.com/rest/SendQuickOTP";

$url = $url."?"."Token=".urlencode("MyToken");
$url = $url."&"."Mobile=9120000000";
$url = $url."&"."CodeLength=4";
$url = $url."&"."OptionalCode=";

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
### متد ارسال از طریق الگو (پترن) (SendWithPattern)
در صورتی که الگوی پیامک خود را در پنل کاربری ثبت نموده باشید با استفاده از این متد می توانید الگوی خود را به صورت فوری و به شماره انتخابی ارسال نمایید.

نکته : شما باید الگوی پیامک خود را در پنل کاربری پیامک ---->  گزینه ی امکانات ---> گزینه ی پیامکهای با الگو ثبت نمایید و پس تایید توسط اپراتور مرکز مخابرات و فعال شدن ، میتوانید الگوی ثبت شده را با استفاده از این متد ارسال نمایید.
#### پارامترهای ورودی SendWithPattern
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|Token|String *|توکن ثبت شده در سامانه پیامک آموت|
|Mobile|String *|موبایل دریافت کننده پیامک|
|PatternCodeID|Int *|کد الگوی پیامک|
|PatternValues|String[] *|مقادیر الگوهای در متن پیامک بایستی به ترتیب همان الگوها در نظر گرفته شود|
#### نمونه کد SendWithPattern (C#)
```csharp
string Token = "MyToken";
string Mobile = "9120000000";
int PatternCodeID = 1;
string[] PatternValues = new string[] { "پارامتر 1", "پارامتر 2" };


using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

    var data = new System.Collections.Specialized.NameValueCollection()
    {
        { "Mobile", Mobile },
        { "PatternCodeID", PatternCodeID.ToString() },
        { "PatternValues", string.Join(",",PatternValues)  },
    };

    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/SendWithPattern", data);

    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes);//خروجی
}
```
#### نمونه کد SendWithPattern (PHP)
```php
$url = "https://portal.amootsms.com/rest/SendWithPattern";

$url = $url."?"."Token=".urlencode("MyToken");
$url = $url."&"."Mobile=9120000000";
$url = $url."&"."PatternCodeID=1";
$url = $url."&"."PatternValues=p1,p2";

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
### متد ارسال از طریق الگو (پترن) با خط اختصاصی (SendWithPatternOWN)
در صورتی که الگوی پیامک خود را در پنل کاربری ثبت نموده باشید با استفاده از این متد می توانید الگوی خود را به صورت فوری و با استفاده از خط اختصاصی خود به شماره انتخابی ارسال نمایید.

نکته : شما باید الگوی پیامک خود را در پنل کاربری پیامک ---->  گزینه ی امکانات ---> گزینه ی پیامکهای با الگو ثبت نمایید و پس تایید توسط اپراتور مرکز مخابرات و فعال شدن ، میتوانید الگوی ثبت شده را با استفاده از این متد ارسال نمایید.
#### پارامترهای ورودی SendWithPatternOWN
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|Token|String *|توکن ثبت شده در سامانه پیامک آموت|
|LineNumber|String *|شماره خط|
|Mobile|String *|موبایل دریافت کننده پیامک|
|PatternCodeID|Int *|کد الگوی پیامک|
|PatternValues|String[] *|مقادیر الگوهای در متن پیامک بایستی به ترتیب همان الگوها در نظر گرفته شود|
#### نمونه کد SendWithPatternOWN (C#)
```csharp
string Token = "MyToken";
string Mobile = "9120000000";
string LineNumber = "public";
int PatternCodeID = 1;
string[] PatternValues = new string[] { "پارامتر 1", "پارامتر 2" };


using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

    var data = new System.Collections.Specialized.NameValueCollection()
    {
        { "Mobile", Mobile },
        { "LineNumber", LineNumber },
        { "PatternCodeID", PatternCodeID.ToString() },
        { "PatternValues", string.Join(",",PatternValues)  },
    };

    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/SendWithPatternOWN", data);

    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes);//خروجی
}
```
#### نمونه کد SendWithPatternOWN (PHP)
```php
$url = "https://portal.amootsms.com/rest/SendWithPatternOWN";

$url = $url."?"."Token=".urlencode("MyToken");
$url = $url."&"."LineNumber=public";
$url = $url."&"."Mobile=9120000000";
$url = $url."&"."PatternCodeID=1";
$url = $url."&"."PatternValues=p1,p2";

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
### متد ارسال نظیر به نظیر (SendPeerToPeer)

#### پارامترهای ورودی SendPeerToPeer
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|Token|String *|توکن ثبت شده در سامانه پیامک آموت|
|SendDateTime|DateTime *|زمان ارسال پیامک|
|LineNumber|String *|شماره خط|
|Input|Object[] *|آرایه ای از یک کلاس با دوفیلد متن به نامهای Number و MessageText هست. که به ازای هر ردیف شماره و متن خاص خود را دارا هستند.|
#### نمونه کد SendPeerToPeer (C#)
```csharp
string Token = "MyToken";
DateTime SendDateTime = DateTime.Now;
string LineNumber = "public";

using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);
    client.Headers.Add(System.Net.HttpRequestHeader.ContentType, "application/json; charset=utf-8");
    client.Encoding = Encoding.UTF8;

    var data = new
    {
        SendDateTime = SendDateTime,
        LineNumber = LineNumber,
        Input = new object[] {
         new { Number = "0915xxxxxxx", MessageText = "متن 1" },
         new { Number = "0912xxxxxxx", MessageText = "متن 2" },
             }
    };

    string jsonInput = JsonConvert.SerializeObject(data);

    string json = client.UploadString("https://portal.amootsms.com/rest/SendPeerToPeer", jsonInput);//خروجی
}
```
### متد محاسبه هزینه ارسال الگو (CalculatePatternMessagePrice)
از طریق این متد می توانید با وارد کردن اطلاعات مربوط به الگو ، متن پیامک و شماره های دریافت کنندگان ، هزینه ارسال با الگوی خود را محاسبه نمایید.
#### پارامترهای ورودی CalculatePatternMessagePrice
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|Token|String *|توکن ثبت شده در سامانه پیامک آموت|
|Mobile|String *|موبایل دریافت کننده پیامک|
|PatternCodeID|Int *|کد الگوی پیامک|
|PatternValues|String[] *|مقادیر الگوهای در متن پیامک بایستی به ترتیب همان الگوها در نظر گرفته شود|
#### نمونه کد CalculatePatternMessagePrice (C#)
```csharp
string Token = "MyToken";
string Mobile = "9120000000";
int PatternCodeID = 1;
string[] PatternValues = new string[] { "پارامتر 1", "پارامتر 2" };


using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

    var data = new System.Collections.Specialized.NameValueCollection()
    {
        { "Mobile", Mobile },
        { "PatternCodeID", PatternCodeID.ToString() },
        { "PatternValues", string.Join(",",PatternValues)  },
    };

    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/CalculatePatternMessagePrice", data);

    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes);//خروجی
}
```
#### نمونه کد CalculatePatternMessagePrice (PHP)
```php
$url = "https://portal.amootsms.com/rest/CalculatePatternMessagePrice";

$url = $url."?"."Token=".urlencode("MyToken");
$url = $url."&"."Mobile=9120000000";
$url = $url."&"."PatternCodeID=1";
$url = $url."&"."PatternValues=p1,p2";

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
### متد دریافت  دلیوری پیامک ها (GetDeliveries)
از طریق این متد می توانید وضعیت دلیوری (گزارش تحویل) پیامک های ارسال شده خود را دریافت نمایید.
#### پارامترهای ورودی GetDeliveries
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|Token|String *|توکن ثبت شده در سامانه پیامک آموت|
|ListMessageID|Object *|لیست کد پیامک (کد شماره کمپین) [با کد کمپین اشتباه نشود]|
#### نمونه کد GetDeliveries (C#)
```csharp
string Token = "MyToken";
long[] ListMessageID = new long[]{
    10001,
    10002,
};


using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

    var data = new System.Collections.Specialized.NameValueCollection()
    {
        { "ListMessageID", string.Join(",",ListMessageID)  },
    };

    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/GetDeliveries", data);

    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes);//خروجی
}
```
#### نمونه کد GetDeliveries (PHP)
```php
$url = "https://portal.amootsms.com/rest/GetDeliveries";

$url = $url."?"."Token=".urlencode("MyToken");
$url = $url."&"."ListMessageID=10001,10002";

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
### متد دریافت دلیوری پیامک (GetDelivery)
از طریق این متد می توانید وضعیت دلیوری (گزارش تحویل) پیامک ارسال شده خود را دریافت نمایید.
#### پارامترهای ورودی GetDelivery
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|Token|String *|توکن ثبت شده در سامانه پیامک آموت|
|MessageID|Long *|کد پیامک (کد شماره کمپین) [با کد کمپین اشتباه نشود]|
#### نمونه کد GetDelivery (C#)
```csharp
string Token = "MyToken";
long MessageID = 10001;


using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

    var data = new System.Collections.Specialized.NameValueCollection()
    {
        { "MessageID", MessageID.ToString() },
    };

    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/GetDelivery", data);

    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes);//خروجی
}
```
#### نمونه کد GetDelivery (PHP)
```php
$url = "https://portal.amootsms.com/rest/GetDelivery";

$url = $url."?"."Token=".urlencode("MyToken");
$url = $url."&"."MessageID=10001";

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
### متد دریافت  دلیوری پیامک ها بر اساس شماره کمپین (کد بالک) (GetDeliveriesByCampaignID)
از طریق این متد می توانید وضعیت دلیوری (گزارش تحویل) پیامک های ارسال شده خود را بر اساس شماره کمپین (کد بالک) دریافت نمایید.
#### پارامترهای ورودی GetDeliveriesByCampaignID
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|Token|String *|توکن ثبت شده در سامانه پیامک آموت|
|CampaignID|Object *|کد کمپین پیامک (کد بالک)|
#### نمونه کد GetDeliveriesByCampaignID (C#)
```csharp
string Token = "MyToken";
long CampaignID = 10001,;


using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

    var data = new System.Collections.Specialized.NameValueCollection()
    {
        { "CampaignID", CampaignID.ToString()  },
    };

    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/GetDeliveriesByCampaignID", data);

    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes);//خروجی
}
```
#### نمونه کد GetDeliveriesByCampaignID (PHP)
```php
$url = "https://portal.amootsms.com/rest/GetDeliveriesByCampaignID";

$url = $url."?"."Token=".urlencode("MyToken");
$url = $url."&"."CampaignID=10001";

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
### متد ایجاد مخاطب (ContactCreate)
از طریق این متد می توانید با وارد کردن اطلاعات مخاطب ، آن شخص را در دفترچه تلفن  پنل کاربری خود ذخیره نمائید.
#### پارامترهای ورودی ContactCreate
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|Token|String *|توکن ثبت شده در سامانه پیامک آموت|
|ContactGroupID|Long *|کد گروه مخاطب (غیرفعال میباشد - 0 بگذارید) برای برچسب از پارامتر Labels استفاده کنید|
|Active|Bool *|فعال|
|Mobile|String *|شماره همراه مخاطب|
|FName|String *|نام|
|LName|String *|نام خانوادگی|
|GenderType|Bool *|False = خانم , True = آقا|
|CompanyTitle|String *|عنوان شرکت|
|JobTitle|String *|عنوان سمت/شغل|
|Email|String *|ایمیل|
|CityName|String *|نام شهر|
|AddressText|String *|آدرس|
|BornDate|Object *|تاریخ تولد|
|AnniversaryDate|Object *|تاریخ سالگرد|
|CustomText1|String *|فیلد سفارشی 1|
|CustomText2|String *|فیلد سفارشی 2|
|CustomText3|String *|فیلد سفارشی 3|
|CustomText4|String *|فیلد سفارشی 4|
|CustomText5|String *|فیلد سفارشی 5|
|CustomText6|String *|فیلد سفارشی 6|
|CustomDate1|Object *|تاریخ سفارشی 1|
|CustomDate2|Object *|تاریخ سفارشی 2|
|CustomDate3|Object *|تاریخ سفارشی 3|
|Labels|String[] *|لیست عناوین برچسب|
#### نمونه کد ContactCreate (C#)
```csharp
string Token = "MyToken";
//غیر فعال میباشد
//از پارامتر
// Labels 
//استفاده کنید
long ContactGroupID = 0;

bool Active = true;
string Mobile = "09120000000";
string FName = "";
string LName = "";
bool GenderType = true;
string CompanyTitle = "";
string JobTitle = "";
string Email = "";
string CityName = "";
string AddressText = "";
DateTime? BornDate = null;
DateTime? AnniversaryDate = null;
string CustomText1 = "";
string CustomText2 = "";
string CustomText3 = "";
string CustomText4 = "";
string CustomText5 = "";
string CustomText6 = "";
DateTime? CustomDate1 = null;
DateTime? CustomDate2 = null;
DateTime? CustomDate3 = null;
string[] Labels = new string[2]{
"گروه 1",
"گروه 2"
};


using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

    var data = new System.Collections.Specialized.NameValueCollection()
    {
        { "Mobile", Mobile  },
        { "FName", FName  },
        { "LName", LName  },
        { "GenderType", GenderType.ToString()  },
        { "CompanyTitle", CompanyTitle  },
        { "JobTitle", JobTitle  },
        { "Email", Email  },
        { "CityName", CityName  },
        { "AddressText", AddressText  },
        { "BornDate", BornDate.ToString()  },
        { "AnniversaryDate", AnniversaryDate.ToString()  },
        { "CustomText1", CustomText1  },
        { "CustomText2", CustomText2  },
        { "CustomText3", CustomText3  },
        { "CustomText4", CustomText4  },
        { "CustomText5", CustomText5  },
        { "CustomText6", CustomText6  },
        { "CustomDate1", CustomDate1.ToString()  },
        { "CustomDate2", CustomDate2.ToString()  },
        { "CustomDate3", CustomDate3.ToString()  },
        { "Labels", string.Join(",",Labels)  },
    };

    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/ContactCreate", data);

    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes);//خروجی
}
```
#### نمونه کد ContactCreate (PHP)
```php
$url = "https://portal.amootsms.com/rest/ContactCreate";

$url = $url."?"."Token=".urlencode("MyToken");
//غیر فعال میباشد
//از پارامتر
// Labels
//استفاده کنید
$url = $url."&"."ContactGroupID=0";

$url = $url."&"."Active=true";
$url = $url."&"."Mobile=09120000000";
$url = $url."&"."FName=";
$url = $url."&"."LName=";
$url = $url."&"."GenderType=";
$url = $url."&"."CompanyTitle=";
$url = $url."&"."JobTitle=";
$url = $url."&"."Email=";
$url = $url."&"."CityName=";
$url = $url."&"."AddressText=";
$url = $url."&"."BornDate=";
$url = $url."&"."AnniversaryDate=";
$url = $url."&"."CustomText1=";
$url = $url."&"."CustomText2=";
$url = $url."&"."CustomText3=";
$url = $url."&"."CustomText4=";
$url = $url."&"."CustomText5=";
$url = $url."&"."CustomText6=";
$url = $url."&"."CustomDate1=";
$url = $url."&"."CustomDate2=";
$url = $url."&"."CustomDate3=";
$url = $url."&"."Labels=گروه 1,گروه 2";

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
### متد حذف مخاطب (ContactDelete)
از طریق این متد می توانید یک مخاطب را از دفترچه تلفن پنل کاربری خود حذف نمائید.
#### پارامترهای ورودی ContactDelete
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|Token|String *|توکن ثبت شده در سامانه پیامک آموت|
|ContactID|Long *|کد مخاطب|
#### نمونه کد ContactDelete (C#)
```csharp
string Token = "MyToken";
long ContactID = 0;


using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

    var data = new System.Collections.Specialized.NameValueCollection()
    {
        { "ContactID", ContactID.ToString()  },
    };

    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/ContactDelete", data);

    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes);//خروجی
}
```
#### نمونه کد ContactDelete (PHP)
```php
$url = "https://portal.amootsms.com/rest/ContactDelete";

$url = $url."?"."Token=".urlencode("MyToken");
$url = $url."&"."ContactID=0";

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
### متد ویرایش مخاطب (ContactEdit)
از طریق این متد می توانید با وارد کردن اطلاعات مخاطب ، آن شخص را در دفترچه تلفن  پنل کاربری ویرایش و ذخیره نمائید.
#### پارامترهای ورودی ContactEdit
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|Token|String *|توکن ثبت شده در سامانه پیامک آموت|
|ContactID|Long *|کد مخاطب جهت ویرایش|
|ContactGroupID|Long *|کد گروه مخاطب|
|Active|Bool *|فعال|
|Mobile|String *|شماره همراه مخاطب|
|FName|String *|نام|
|LName|String *|نام خانوادگی|
|GenderType|Bool *|False = خانم , True = آقا|
|CompanyTitle|String *|عنوان شرکت|
|JobTitle|String *|عنوان سمت/شغل|
|Email|String *|ایمیل|
|CityName|String *|نام شهر|
|AddressText|String *|آدرس|
|BornDate|Object *|تاریخ تولد|
|AnniversaryDate|Object *|تاریخ سالگرد|
|CustomText1|String *|فیلد سفارشی 1|
|CustomText2|String *|فیلد سفارشی 2|
|CustomText3|String *|فیلد سفارشی 3|
|CustomText4|String *|فیلد سفارشی 4|
|CustomText5|String *|فیلد سفارشی 5|
|CustomText6|String *|فیلد سفارشی 6|
|CustomDate1|Object *|تاریخ سفارشی 1|
|CustomDate2|Object *|تاریخ سفارشی 2|
|CustomDate3|Object *|تاریخ سفارشی 3|
#### نمونه کد ContactEdit (C#)
```csharp
string Token = "MyToken";
long ContactID = 0;

long ContactGroupID = 0;
bool Active = true;
string Mobile = "09120000000";
string FName = "";
string LName = "";
bool GenderType = true;
string CompanyTitle = "";
string JobTitle = "";
string Email = "";
string CityName = "";
string AddressText = "";
DateTime? BornDate = null;
DateTime? AnniversaryDate = null;
string CustomText1 = "";
string CustomText2 = "";
string CustomText3 = "";
string CustomText4 = "";
string CustomText5 = "";
string CustomText6 = "";
DateTime? CustomDate1 = null;
DateTime? CustomDate2 = null;
DateTime? CustomDate3 = null;


using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

    var data = new System.Collections.Specialized.NameValueCollection()
    {
        { "ContactID", ContactID.ToString()  },
        { "Mobile", Mobile  },
        { "FName", FName  },
        { "LName", LName  },
        { "GenderType", GenderType.ToString()  },
        { "CompanyTitle", CompanyTitle  },
        { "JobTitle", JobTitle  },
        { "Email", Email  },
        { "CityName", CityName  },
        { "AddressText", AddressText  },
        { "BornDate", BornDate.ToString()  },
        { "AnniversaryDate", AnniversaryDate.ToString()  },
        { "CustomText1", CustomText1  },
        { "CustomText2", CustomText2  },
        { "CustomText3", CustomText3  },
        { "CustomText4", CustomText4  },
        { "CustomText5", CustomText5  },
        { "CustomText6", CustomText6  },
        { "CustomDate1", CustomDate1.ToString()  },
        { "CustomDate2", CustomDate2.ToString()  },
        { "CustomDate3", CustomDate3.ToString()  },
        { "Labels", string.Join(",",Labels)  },
    };

    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/ContactEdit", data);

    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes);//خروجی
}
```
#### نمونه کد ContactEdit (PHP)
```php
$url = "https://portal.amootsms.com/rest/ContactEdit";

$url = $url."?"."Token=".urlencode("MyToken");
$url = $url."&"."ContactID=0";

$url = $url."&"."ContactGroupID=0";
$url = $url."&"."Active=true";
$url = $url."&"."Mobile=09120000000";
$url = $url."&"."FName=";
$url = $url."&"."LName=";
$url = $url."&"."GenderType=";
$url = $url."&"."CompanyTitle=";
$url = $url."&"."JobTitle=";
$url = $url."&"."Email=";
$url = $url."&"."CityName=";
$url = $url."&"."AddressText=";
$url = $url."&"."BornDate=";
$url = $url."&"."AnniversaryDate=";
$url = $url."&"."CustomText1=";
$url = $url."&"."CustomText2=";
$url = $url."&"."CustomText3=";
$url = $url."&"."CustomText4=";
$url = $url."&"."CustomText5=";
$url = $url."&"."CustomText6=";
$url = $url."&"."CustomDate1=";
$url = $url."&"."CustomDate2=";
$url = $url."&"."CustomDate3=";

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
### متد دریافت اطلاعات مخاطب (ContactGet)
از طریق این متد می توانید اطلاعات  مربوط به مخاطب خود را از پنل  کاربری خود دریافت نمایید.
#### پارامترهای ورودی ContactGet
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|Token|String *|توکن ثبت شده در سامانه پیامک آموت|
|ContactID|Long *|کد مخاطب|
#### نمونه کد ContactGet (C#)
```csharp
string Token = "MyToken";
long ContactID = 0;


using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

    var data = new System.Collections.Specialized.NameValueCollection()
    {
        { "ContactID", ContactID.ToString()  },
    };

    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/ContactGet", data);

    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes);//خروجی
}
```
#### نمونه کد ContactGet (PHP)
```php
$url = "https://portal.amootsms.com/rest/ContactGet";

$url = $url."?"."Token=".urlencode("MyToken");
$url = $url."&"."ContactID=0";

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
### متد دریافت لیست گروههای (برچسب های) مخاطبین (ContactGroupList)
از طریق این متد می توانید لیست گروههای (برچسب های) مخاطبین موجود در دفترچه تلفن دریافت نمائید.
#### پارامترهای ورودی ContactGroupList
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|Token|String *|توکن ثبت شده در سامانه پیامک آموت|
#### نمونه کد ContactGroupList (C#)
```csharp
string Token = "MyToken";
using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

    var data = new System.Collections.Specialized.NameValueCollection()
    {

    };

    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/ContactGroupList", data);

    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes);//خروجی
}
```
#### نمونه کد ContactGroupList (PHP)
```php
$url = "https://portal.amootsms.com/rest/ContactGroupList";

$url = $url."?"."Token=".urlencode("MyToken");
$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
### متد دریافت لیست مخاطبین گروه (ContactList)
از طریق این متد می توانید با وارد کردن اطلاعات گروه (کد برچسب یا گروه مخاطب) ، مخاطبین موجود در آن گروه بندی  را دریافت نمایید .
#### پارامترهای ورودی ContactList
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|Token|String *|توکن ثبت شده در سامانه پیامک آموت|
|ContactGroupID|Long *|کد گروه مخاطبین (برچسب)  0 به معنای عدم فیلتر -1 به معنای لیست مخاطبین بدون برچسب|
#### نمونه کد ContactList (C#)
```csharp
string Token = "MyToken";
long ContactGroupID = 0;


using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

    var data = new System.Collections.Specialized.NameValueCollection()
    {

    };

    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/ContactList", data);

    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes);//خروجی
}
```
#### نمونه کد ContactList (PHP)
```php
$url = "https://portal.amootsms.com/rest/ContactList";

$url = $url."?"."Token=".urlencode("MyToken");
$url = $url."&"."ContactGroupID=0";

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
### متد ایجاد ارسال هوشمند دوره ای (CourseCreateSimple)
از طریق این متد می توانید یک برنامه ارسال دوره ای هوشمند را ایجاد نمایید ، در صورتی که پنل دارای اعتبار باشد ارسال ها به صورت خود کار انجام می گردد.
#### پارامترهای ورودی CourseCreateSimple
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|Token|String *|توکن ثبت شده در سامانه پیامک آموت|
|CourseGroupID|Long *|کد گروه برنامه ارسال دوره ای هوشمند|
|CourseType|String *|نوع برنامه هوشمند Yearly = 0, Monthly = 1, Weekly = 2, Daily = 3,|
|CourseDateTime|Object *|زمان شروع برنامه|
|Title|String *|عنوان|
|Mobile|String *|موبایل|
|SMSMessageText|String *|متن پیامک|
|LineNumber|String *|شماره خط|
#### نمونه کد CourseCreateSimple (C#)
```csharp
string Token = "MyToken";
long CourseGroupID = 0;
string CourseType = "Daily";
DateTime CourseDateTime = DateTime.Now.Date;
string Title = "";
string Mobile = "9120000000";
string SMSMessageText = "";
string LineNumber = "public";



using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

    var data = new System.Collections.Specialized.NameValueCollection()
    {
        { "CourseGroupID", CourseGroupID.ToString() },
        { "CourseType", CourseType },
        { "CourseDateTime", CourseDateTime.ToString() },

        { "Title", Title },
        { "Mobile", Mobile },
        { "SMSMessageText", SMSMessageText  },
        { "LineNumber", LineNumber },
    };

    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/CourseCreateSimple", data);

    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes);//خروجی
}
```
#### نمونه کد CourseCreateSimple (PHP)
```php
$url = "https://portal.amootsms.com/rest/CourseCreateSimple";

$url = $url."?"."Token=".urlencode("MyToken");
$url = $url."&"."CourseGroupID=0";
$url = $url."&"."CourseType=Daily";

$nowIran = new DateTime('now', new DateTimeZone('IRAN'));
$url = $url."&"."CourseDateTime=".$nowIran->format('c');

$url = $url."&"."Title=MyTitle";
$url = $url."&"."Mobile=9120000000";
$url = $url."&"."SMSMessageText=".urlencode("پیامک تستی من");
$url = $url."&"."LineNumber=public";

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
### متد ایجاد ارسال هوشمند دوره ای با گارانتی خط تبلیغاتی (CourseCreateWithBackupLine)
از طریق این متد می توانید یک برنامه ارسال دوره ای هوشمند را ایجاد نمایید ، در صورتی که  شماره دریافت کننده در لیست سیاه باشد از طریق خط تبلیغاتی (98) به ایشان ارسال انجام میگردد.
#### پارامترهای ورودی CourseCreateWithBackupLine
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|Token|String *|توکن ثبت شده در سامانه پیامک آموت|
|CourseGroupID|Long *|کد گروه برنامه ارسال دوره ای هوشمند|
|CourseType|String *|نوع برنامه هوشمند Yearly = 0, Monthly = 1, Weekly = 2, Daily = 3,|
|CourseDateTime|Object *|زمان شروع برنامه|
|Title|String *|عنوان|
|Mobile|String *|موبایل|
|SMSMessageText|String *|متن پیامک|
|LineNumber|String *|شماره خط|
|BackupLineNumber|String *|شماره خط پشتیبان|
#### نمونه کد CourseCreateWithBackupLine (C#)
```csharp
string Token = "MyToken";
long CourseGroupID = 0;
SMS.CourseType CourseType = SMS.CourseType.Daily;
DateTime CourseDateTime = DateTime.Now.Date;
string Title = "";
string Mobile = "9120000000";
string SMSMessageText = "پیامک تستی من";
string LineNumber = "public";
string BackupLineNumber = "98";


using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

    var data = new System.Collections.Specialized.NameValueCollection()
    {
        { "CourseGroupID", CourseGroupID.ToString() },
        { "CourseType", CourseType },
        { "CourseDateTime", CourseDateTime.ToString() },

        { "Title", Title },
        { "Mobile", Mobile },
        { "SMSMessageText", SMSMessageText  },
        { "LineNumber", LineNumber },
        { "BackupLineNumber", BackupLineNumber },
    };

    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/CourseCreateWithBackupLine", data);

    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes);//خروجی
}
```
#### نمونه کد CourseCreateWithBackupLine (PHP)
```php
$url = "https://portal.amootsms.com/rest/CourseCreateWithBackupLine";

$url = $url."?"."Token=".urlencode("MyToken");
$url = $url."&"."CourseGroupID=0";
$url = $url."&"."CourseType=Daily";

$nowIran = new DateTime('now', new DateTimeZone('IRAN'));
$url = $url."&"."CourseDateTime=".$nowIran->format('c');

$url = $url."&"."Title=MyTitle";
$url = $url."&"."Mobile=9120000000";
$url = $url."&"."SMSMessageText=".urlencode("پیامک تستی من");
$url = $url."&"."LineNumber=public";
$url = $url."&"."BackupLineNumber=98";

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
### متد دریافت لیست برنامه های ارسال هوشمند دوره ای (CourseList)
از طریق این متد می توانید لیست برنامه های ارسال دوره ای هوشمند را دریافت نمایید.
#### پارامترهای ورودی CourseList
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|Token|String *|توکن ثبت شده در سامانه پیامک آموت|
#### نمونه کد CourseList (C#)
```csharp
string Token = "MyToken";
using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

    var data = new System.Collections.Specialized.NameValueCollection()
    {

    };

    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/CourseList", data);

    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes);//خروجی
}
```
#### نمونه کد CourseList (PHP)
```php
$url = "https://portal.amootsms.com/rest/CourseList";

$url = $url."?"."Token=".urlencode("MyToken");
$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
### متد فعال کردن برنامه ارسال هوشمند دوره ای (CourseSetActive)
از طریق این متد می توانید یک برنامه ارسال دوره ای هوشمند را فعال یا غیرفعال نمایید.
#### پارامترهای ورودی CourseSetActive
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|Token|String *|توکن ثبت شده در سامانه پیامک آموت|
|CourseID|Long *|کد برنامه ارسال دوره ای هوشمند|
|Active|Bool *|فعال بودن|
#### نمونه کد CourseSetActive (C#)
```csharp
string Token = "MyToken";
long CourseID = 0;
bool Active = true;


using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

    var data = new System.Collections.Specialized.NameValueCollection()
    {
        { "CourseID", CourseID.ToString()  },
        { "Active", Active.ToString()  },
    };

    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/CourseSetActive", data);

    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes);//خروجی
}
```
#### نمونه کد CourseSetActive (PHP)
```php
$url = "https://portal.amootsms.com/rest/CourseSetActive";

$url = $url."?"."Token=".urlencode("MyToken");
$url = $url."&"."CourseID=0";
$url = $url."&"."Active=true";

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
### متد دریافت لیست بانک اطلاعاتی (GetBankTree)
از طریق این متد می توانید لیست درختی بانک اطلاعاتی شماره همراه را دریافت نمائید.
#### پارامترهای ورودی GetBankTree
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|Token|String *|توکن ثبت شده در سامانه پیامک آموت|
#### نمونه کد GetBankTree (C#)
```csharp
string Token = "MyToken";
using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

    var data = new System.Collections.Specialized.NameValueCollection()
    {

    };

    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/GetBankTree", data);

    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes);//خروجی
}
```
#### نمونه کد GetBankTree (PHP)
```php
$url = "https://portal.amootsms.com/rest/GetBankTree";

$url = $url."?"."Token=".urlencode("MyToken");
$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
### متد دریافت پیامک ارسالی (GetMessage)
از طریق این متد می توانید  پیامکی که ارسال نمودید را دریافت نمایید.
#### پارامترهای ورودی GetMessage
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|Token|String *|توکن ثبت شده در سامانه پیامک آموت|
|MessageID|Long *|کد پیام|
#### نمونه کد GetMessage (C#)
```csharp
string Token = "MyToken";
long MessageID = 1000;


using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

    var data = new System.Collections.Specialized.NameValueCollection()
    {
        { "MessageID", MessageID.ToString()  },
    };

    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/GetMessage", data);

    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes);//خروجی
}
```
#### نمونه کد GetMessage (PHP)
```php
$url = "https://portal.amootsms.com/rest/GetMessage";

$url = $url."?"."Token=".urlencode("MyToken");
$url = $url."&"."MessageID=1000";

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
### متد دریافت پیامک های دریافتی (RecieveMessages)
از طریق این متد می توانید پیامک های ارسالی خود را  در بازه زمانی مشخص دریافت نمایید.
#### پارامترهای ورودی RecieveMessages
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|Token|String *|توکن ثبت شده در سامانه پیامک آموت|
|FromDateTime|Object *|از تاریخ/زمان|
|ToDateTime|Object *|تا تاریخ/زمان|
|LineNumber|String |شماره خط (این فیلد اختیاری میباشد و درصورت مقدار براساس آن شماره خط فیلتر میشود)|
#### نمونه کد RecieveMessages (C#)
```csharp
string Token = "MyToken";
DateTime FromDateTime = DateTime.Now.AddMonths(-1);
DateTime ToDateTime = DateTime.Now;
string LineNumber="";


using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

    var data = new System.Collections.Specialized.NameValueCollection()
    {
        { "FromDateTime", FromDateTime.ToString() },
        { "ToDateTime", ToDateTime.ToString() },
        { "LineNumber", LineNumber },
    };

    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/RecieveMessages", data);

    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes);//خروجی
}
```
#### نمونه کد RecieveMessages (PHP)
```php
$url = "https://portal.amootsms.com/rest/RecieveMessages";

$url = $url."?"."Token=".urlencode("MyToken");
$FromDateTime= new DateTime('2020-01-01');
$url = $url."&"."FromDateTime=".$FromDateTimeIran->format('c');

$ToDateTime= new DateTime('2020-02-01');
$url = $url."&"."ToDateTime=".$ToDateTime->format('c');

$LineNumber = "";
$url = $url."&"."LineNumber=".$LineNumber;

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
### متد دریافت پیامک های جدید دریافتی (RecieveNewMessages)
از طریق این متد می توانید پیامک های جدید دریافتی خود را دریافت نمایید. 

نکته : در هر بار اجرای این متد فقط یکبار پیامک ها را به شما نشان می دهد.
#### پارامترهای ورودی RecieveNewMessages
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|Token|String *|توکن ثبت شده در سامانه پیامک آموت|
|Count|Int *|تعداد پیامهای دریافتی|
#### نمونه کد RecieveNewMessages (C#)
```csharp
string Token = "MyToken";
int Count = 10;


using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

    var data = new System.Collections.Specialized.NameValueCollection()
    {
        { "Count", Count.ToString() },
    };

    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/RecieveNewMessages", data);

    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes);//خروجی
}
```
#### نمونه کد RecieveNewMessages (PHP)
```php
$url = "https://portal.amootsms.com/rest/RecieveNewMessages";

$url = $url."?"."Token=".urlencode("MyToken");
$url = $url."&"."Count=100";

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
### متد افزودن آدرس جهت دریافت دلیوری (RelayMessageDeliveryCreate)
از طریق این متد می تواند یک آدرس جدید به لیست آدرسهای "انتقال دلیوری پیامهای ارسالی" اضافه کند.
#### پارامترهای ورودی RelayMessageDeliveryCreate
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|Token|String *|توکن ثبت شده در سامانه پیامک آموت|
|URLAddress|String *|آدرس URL|
#### نمونه کد RelayMessageDeliveryCreate (C#)
```csharp
string Token = "MyToken";
string URLAddress = "http://Mysite.com/RelayMessageDelivery";

using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

    var data = new System.Collections.Specialized.NameValueCollection()
    {
        { "URLAddress", URLAddress },
    };

    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/RelayMessageDeliveryCreate", data);

    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes);//خروجی
}
```
#### نمونه کد RelayMessageDeliveryCreate (PHP)
```php
$url = "https://portal.amootsms.com/rest/RelayMessageDeliveryCreate";

$url = $url."?"."Token=".urlencode("MyToken");
$url = $url."&"."URLAddress=".urlencode( "http://Mysite.com/RelayMessageDelivery");

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
### متد دریافت لیست آدرس های دریافت دلیوری (RelayMessageDeliveryList)
از طریق این متد می توانید لیست آدرسهای "انتقال دلیوری پیامهای ارسالی" را دریافت نمایید.
#### پارامترهای ورودی RelayMessageDeliveryList
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|Token|String *|توکن ثبت شده در سامانه پیامک آموت|
#### نمونه کد RelayMessageDeliveryList (C#)
```csharp
string Token = "MyToken";
using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

    var data = new System.Collections.Specialized.NameValueCollection()
    {

    };

    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/RelayMessageDeliveryList", data);

    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes);//خروجی
}
```
#### نمونه کد RelayMessageDeliveryList (PHP)
```php
$url = "https://portal.amootsms.com/rest/RelayMessageDeliveryList";

$url = $url."?"."Token=".urlencode("MyToken");
$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
### متد فعال کردن آدرس انتقال دلیوری (RelayMessageDeliverySetActive)
از طریق این متد می توانید یک آدرس "انتقال دلیوری پیامهای ارسالی" را فعال یا غیرفعال نمایید.
#### پارامترهای ورودی RelayMessageDeliverySetActive
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|Token|String *|توکن ثبت شده در سامانه پیامک آموت|
|RelayMessageDeliveryID|Long *|کد آدرس "انتقال دلیوری پیامهای ارسالی"|
|Active|Bool *|فعال بودن|
#### نمونه کد RelayMessageDeliverySetActive (C#)
```csharp
string Token = "MyToken";
long RelayMessageDeliveryID = 0;
bool Active = true;


using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

    var data = new System.Collections.Specialized.NameValueCollection()
    {
        { "RelayMessageDeliveryID", RelayMessageDeliveryID.ToString() },
        { "Active", Active.ToString() },
    };

    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/RelayMessageDeliverySetActive", data);

    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes);//خروجی
}
```
#### نمونه کد RelayMessageDeliverySetActive (PHP)
```php
$url = "https://portal.amootsms.com/rest/RelayMessageDeliverySetActive";

$url = $url."?"."Token=".urlencode("MyToken");
$url = $url."&"."RelayMessageDeliveryID=0";
$url = $url."&"."Active=true";

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
### متد افزودن آدرس انتقال پیامهای دریافتی (RelayRecieveMessageCreate)
از طریق این متد می توانید یک آدرس جدید به لیست آدرسهای "انتقال پیامهای دریافتی" اضافه می کند.
#### پارامترهای ورودی RelayRecieveMessageCreate
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|Token|String *|توکن ثبت شده در سامانه پیامک آموت|
|URLAddress|String *|آدرس URL|
#### نمونه کد RelayRecieveMessageCreate (C#)
```csharp
string Token = "MyToken";
string URLAddress = "http://Mysite.com/RelayRecieveMessage";


using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

    var data = new System.Collections.Specialized.NameValueCollection()
    {
        { "URLAddress", URLAddress },
    };

    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/RelayRecieveMessageCreate", data);

    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes);//خروجی
}
```
#### نمونه کد RelayRecieveMessageCreate (PHP)
```php
$url = "https://portal.amootsms.com/rest/RelayRecieveMessageCreate";

$url = $url."?"."Token=".urlencode("MyToken");
$url = $url."&"."URLAddress=".urlencode( "http://Mysite.com/RelayRecieveMessage");

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
### متد دریافت آدرس های انتقال پیامهای دریافتی (RelayRecieveMessageList)
از طریق این متد می توانید لیست آدرسهای "انتقال پیامهای دریافتی" را دریافت نمایید.
#### پارامترهای ورودی RelayRecieveMessageList
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|Token|String *|توکن ثبت شده در سامانه پیامک آموت|
#### نمونه کد RelayRecieveMessageList (C#)
```csharp
string Token = "MyToken";
using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

    var data = new System.Collections.Specialized.NameValueCollection()
    {

    };

    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/RelayRecieveMessageList", data);

    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes);//خروجی
}
```
#### نمونه کد RelayRecieveMessageList (PHP)
```php
$url = "https://portal.amootsms.com/rest/RelayRecieveMessageList";

$url = $url."?"."Token=".urlencode("MyToken");
$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
### متد فعال کردن آدرس انتقال پیامهای دریافتی (RelayRecieveMessageSetActive)
از طریق این متد می توانید یک آدرس "انتقال پیامهای دریافتی" را فعال یا غیرفعال نمایید.
#### پارامترهای ورودی RelayRecieveMessageSetActive
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|Token|String *|توکن ثبت شده در سامانه پیامک آموت|
|RelayRecieveMessageID|Long *|کد آدرس "انتقال پیامهای دریافتی"|
|Active|Bool *|فعال بودن|
#### نمونه کد RelayRecieveMessageSetActive (C#)
```csharp
string Token = "MyToken";
long RelayRecieveMessageID = 0;
bool Active = true;


using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

    var data = new System.Collections.Specialized.NameValueCollection()
    {
        { "RelayRecieveMessageID", RelayRecieveMessageID.ToString() },
        { "Active", Active.ToString() },
    };

    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/RelayRecieveMessageSetActive", data);

    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes);//خروجی
}
```
#### نمونه کد RelayRecieveMessageSetActive (PHP)
```php
$url = "https://portal.amootsms.com/rest/RelayRecieveMessageSetActive";

$url = $url."?"."Token=".urlencode("MyToken");
$url = $url."&"."RelayRecieveMessageID=0";
$url = $url."&"."Active=true";

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
### متد ارسال از طریق بانک اطلاعاتی  (SendBank)
از طریق این متد می توانید پیامک های خود را با استفاده از بانک های اطلاعاتی موجود در سامانه ارسال نمایید.
#### پارامترهای ورودی SendBank
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|Token|String *|توکن ثبت شده در سامانه پیامک آموت|
|SendDateTime|Object *|زمان ارسال پیامک|
|SMSMessageText|String *|متن پیامک|
|LineNumber|String *|شماره خط|
|BankID|Long *|کد بانک اطلاعاتی|
|OrderType|String *|ترتیب انتخاب Sequential = 0, Random = 1,|
|FromRow|Int *|از ردیف|
|Count|Int *|تعداد|
#### نمونه کد SendBank (C#)
```csharp
string Token = "MyToken";
DateTime SendDateTime = DateTime.Now;
string SMSMessageText = "پیامک تستی من";
string LineNumber = "public";

long BankID = 0;
bool IsRandom = false;
int FromRow = 0;
int Count = 100;


using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

    var data = new System.Collections.Specialized.NameValueCollection()
    {
        { "SendDateTime", SendDateTime.ToString() },
        { "SMSMessageText", SMSMessageText  },
        { "LineNumber", LineNumber },

        { "BankID", BankID.ToString() },
        { "IsRandom", IsRandom.ToString() },
        { "FromRow", FromRow.ToString() },
        { "Count", Count.ToString() },
    };

    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/SendBank", data);

    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes);//خروجی
}
```
#### نمونه کد SendBank (PHP)
```php
$url = "https://portal.amootsms.com/rest/SendBank";

$url = $url."?"."Token=".urlencode("MyToken");
$nowIran = new DateTime('now', new DateTimeZone('IRAN'));
$url = $url."&"."SendDateTime=".urlencode($nowIran->format('c'));

$url = $url."&"."SMSMessageText=".urlencode("پیامک تستی من");
$url = $url."&"."LineNumber=public";

$url = $url."&"."BankID=0";
$url = $url."&"."SendOrderType=Sequential";
$url = $url."&"."FromRow=0";
$url = $url."&"."Count=0";

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
### متد ارسال کد اعتبار سنجی (SendOTP)
از طریق این متد می توانید پیامک با رمز یکبار مصرف را به شماره انتخابی خود ارسال نمایید ، جهت استفاده از این متد باید نام مجوعه خود را اعلام نموده باشید.
#### پارامترهای ورودی SendOTP
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|Token|String *|توکن ثبت شده در سامانه پیامک آموت|
|SendDateTime|Object *|زمان ارسال پیامک|
|SMSMessageText|String *|متن پیامک|
|LineNumber|String *|شماره خط|
|Mobile|String *|لیست موبایل های دریافت کنندگان پیامک|
|CodeLength|Short *|طول کد (حداقل 4 رقم و حداکثر 8 رقم)|
#### نمونه کد SendOTP (C#)
```csharp
string Token = "MyToken";
DateTime SendDateTime = DateTime.Now;
string SMSMessageText = "پیامک تستی من";
string LineNumber = "public";
string Mobile = "9120000000";
short CodeLength = 4;


using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

    var data = new System.Collections.Specialized.NameValueCollection()
    {
        { "SendDateTime", SendDateTime.ToString() },
        { "SMSMessageText", SMSMessageText  },
        { "LineNumber", LineNumber },
        { "Mobile", Mobile },

        { "CodeLength", CodeLength.ToString() },
    };

    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/SendOTP", data);

    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes);//خروجی
}
```
#### نمونه کد SendOTP (PHP)
```php
$url = "https://portal.amootsms.com/rest/SendOTP";

$url = $url."?"."Token=".urlencode("MyToken");
$nowIran = new DateTime('now', new DateTimeZone('IRAN'));
$url = $url."&"."SendDateTime=".urlencode($nowIran->format('c'));

$url = $url."&"."SMSMessageText=".urlencode("پیامک تستی من");
$url = $url."&"."LineNumber=public";

$url = $url."&"."Mobile=9120000000";
$url = $url."&"."CodeLength=4";

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
### متد آمار پیامک های ارسالی (Statistics)
از طریق این متد می توانید آمار و گزارش پیامک های ارسالی خود را دریافت نمایید.
#### پارامترهای ورودی Statistics
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|Token|String *|توکن ثبت شده در سامانه پیامک آموت|
|FromDate|Object *|از تاریخ|
|ToDate|Object *|تا تاریخ|
#### نمونه کد Statistics (C#)
```csharp
string Token = "MyToken";
DateTime FromDate = DateTime.Now.AddMonths(-1);
DateTime ToDate = DateTime.Now;

using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

    var data = new System.Collections.Specialized.NameValueCollection()
    {
        { "FromDateTime", FromDateTime.ToString() },
        { "ToDateTime", ToDateTime.ToString() },
    };

    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/Statistics", data);

    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes);//خروجی
}
```
#### نمونه کد Statistics (PHP)
```php
$url = "https://portal.amootsms.com/rest/Statistics";

$url = $url."?"."Token=".urlencode("MyToken");
$url = $url."&"."FromDate=2020-02-01";
$url = $url."&"."ToDate=2020-03-01";

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
### متد ارسال از طریق بانک اطلاعاتی با گارانتی خط تبلیغاتی (SendBankWithBackupLine)
از طریق این متد می توانید پیامک های خود را با استفاده از بانک اطلاعاتی و با گارانتی خط تبلیغاتی (98) ارسال نمایید ، در صورتی که  شماره دریافت کننده در لیست سیاه باشد از طریق خط تبلیغاتی (98) به ایشان ارسال انجام میگردد.
#### پارامترهای ورودی SendBankWithBackupLine
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|Token|String *|توکن ثبت شده در سامانه پیامک آموت|
|SendDateTime|Object *|زمان ارسال پیامک|
|SMSMessageText|String *|متن پیامک|
|LineNumber|String *|شماره خط|
|BackupLineNumber|String *|شماره خط پشتیبان|
|BankID|Long *|کد بانک اطلاعاتی|
|OrderType|String *|ترتیب انتخاب Sequential = 0, Random = 1,|
|FromRow|Int *|از ردیف|
|Count|Int *|تعداد|
#### خروجی SendBankWithBackupLine (Object)
```html
result.Data شامل آرایه ای از یک کلاس با مقادیر
{
Status = وضعیت ,
MessageID = کد پیامک (کد شماره کمپین) ,
Mobile = شماره موبایل
}
```
#### نمونه کد SendBankWithBackupLine (C#)
```csharp
string Token = "MyToken";
DateTime SendDateTime = DateTime.Now;
string SMSMessageText = "پیامک تستی من";

string LineNumber = "public";
string BackupLineNumber = "98";

long BankID = 0;
SMS.SendOrderType SendOrderType = SMS.SendOrderType.Sequential;
int FromRow = 0;
int Count = 100;


using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

    var data = new System.Collections.Specialized.NameValueCollection()
    {
        { "SendDateTime", SendDateTime.ToString() },
        { "SMSMessageText", SMSMessageText  },
        { "LineNumber", LineNumber },
        { "BackupLineNumber", BackupLineNumber },

        { "BankID", BankID.ToString() },
        { "IsRandom", IsRandom.ToString() },
        { "FromRow", FromRow.ToString() },
        { "Count", Count.ToString() },
    };

    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/SendBankWithBackupLine", data);

    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes);//خروجی
}
```
#### نمونه کد SendBankWithBackupLine (PHP)
```php
$url = "https://portal.amootsms.com/rest/SendBankWithBackupLine";

$url = $url."?"."Token=".urlencode("MyToken");
$nowIran = new DateTime('now', new DateTimeZone('IRAN'));
$url = $url."&"."SendDateTime=".urlencode($nowIran->format('c'));

$url = $url."&"."SMSMessageText=".urlencode("پیامک تستی من");
$url = $url."&"."LineNumber=public";
$url = $url."&"."BackupLineNumber=98";

$url = $url."&"."BankID=0";
$url = $url."&"."SendOrderType=Sequential";
$url = $url."&"."FromRow=0";
$url = $url."&"."Count=0";

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
### متد دریافت پیامک های ارسالی (GetMessages)
از طریق این متد می توانید پیامک های ارسالی خود را  در بازه زمانی مشخص دریافت نمایید.
#### پارامترهای ورودی GetMessages
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|Token|String *|توکن ثبت شده در سامانه پیامک آموت|
|FromDateTime|Object *|از تاریخ/زمان|
|ToDateTime|Object *|تا تاریخ/زمان|
#### خروجی GetMessages (Object)
```html
result.Data شامل آرایه ای از یک کلاس با مقادیر
{
MessageID = کد پیامک (کد شماره کمپین) ,
Mobile = شماره موبایل,
LineNumber = شماره خط ,
SMSMessageText = متن پیامک دریافتی,
RegDateTime = زمان دریافت
}
```
#### نمونه کد GetMessages (C#)
```csharp
string Token = "MyToken";
DateTime FromDateTime = DateTime.Now.Date;
DateTime ToDateTime = DateTime.Now.Date;


using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

    var data = new System.Collections.Specialized.NameValueCollection()
    {
        { "FromDateTime", FromDateTime.ToString() },
        { "ToDateTime", ToDateTime.ToString() },
    };

    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/GetMessages", data);

    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes);//خروجی
}
```
#### نمونه کد GetMessages (PHP)
```php
$url = "https://portal.amootsms.com/rest/GetMessages";

$url = $url."?"."Token=".urlencode("MyToken");
$FromDateTime= new DateTime('2020-01-01');
$url = $url."&"."FromDateTime=".$FromDateTimeIran->format('c');

$ToDateTime= new DateTime('2020-02-01');
$url = $url."&"."ToDateTime=".$ToDateTime->format('c');

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
### متد محاسبه هزینه پیامک (CalculateMessagePrice)
از طریق این متد می توانید با وارد کردن متن پیامک به همراه شماره خط ارسال و شماره های دریافت کنندگان هزینه ارسال خود را محاسبه نمایید.
#### پارامترهای ورودی CalculateMessagePrice
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|Token|String *|توکن ثبت شده در سامانه پیامک آموت|
|SMSMessageText|String *|متن پیامک|
|LineNumber|String *|شماره خط|
|Mobiles|Object *|لیست موبایل های دریافت کنندگان پیامک|
#### نمونه کد CalculateMessagePrice (C#)
```csharp
string Token = "MyToken";
string SMSMessageText = "پیامک تستی من";
string LineNumber = "public";
string[] Mobiles = new string[]
{
    "9120000000",
    "9150000000",
};


using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

    var data = new System.Collections.Specialized.NameValueCollection()
    {
        { "SMSMessageText", SMSMessageText  },
        { "LineNumber", LineNumber },
        { "Mobiles", string.Join(",",Mobiles)  },
    };

    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/CalculateMessagePrice", data);

    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes);//خروجی
}
```
#### نمونه کد CalculateMessagePrice (PHP)
```php
$url = "https://portal.amootsms.com/rest/CalculateMessagePrice";

$url = $url."?"."Token=".urlencode("MyToken");
$SMSMessageText = urlencode("پیامک تستی من");
$url = $url."&"."SMSMessageText =".$SMSMessageText ;

$url = $url."&"."LineNumber=public";
$url = $url."&"."Mobiles=9120000000,9150000000";

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
### متد ارسال با گارانتی خط تبلیغاتی (SendWithBackupLine)
از طریق این متد می توانید پیامک های خود را با گارانتی خط تبلیغاتی (98) ارسال نمایید ، در صورتی که  شماره دریافت کننده در لیست سیاه باشد از طریق خط تبلیغاتی (98) به ایشان ارسال انجام میگردد.
#### پارامترهای ورودی SendWithBackupLine
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|Token|String *|توکن ثبت شده در سامانه پیامک آموت|
|SendDateTime|Object *|زمان ارسال پیامک|
|SMSMessageText|String *|متن پیامک|
|LineNumber|String *|شماره خط|
|BackupLineNumber|String *|شماره خط پشتیبان|
|Mobiles|Object *|لیست موبایل های دریافت کنندگان پیامک|
#### نمونه کد SendWithBackupLine (C#)
```csharp
string Token = "MyToken";
DateTime SendDateTime = DateTime.Now;
string SMSMessageText = "پیامک تستی من";
string LineNumber = "public";
string BackupLineNumber = "98";
string[] Mobiles = new string[]
{
"9120000000",
"9150000000",
};


using (var client = new System.Net.WebClient())
{
    client.Headers.Add(System.Net.HttpRequestHeader.Authorization, Token);

    var data = new System.Collections.Specialized.NameValueCollection()
    {
        { "SendDateTime", SendDateTime.ToString() },
        { "SMSMessageText", SMSMessageText  },
        { "LineNumber", LineNumber },
        { "BackupLineNumber", BackupLineNumber },
        { "Mobiles", string.Join(",",Mobiles)  },
    };

    byte[] bytes = client.UploadValues("https://portal.amootsms.com/rest/SendWithBackupLine", data);

    string json = System.Text.UTF8Encoding.UTF8.GetString(bytes);//خروجی
}
```
#### نمونه کد SendWithBackupLine (PHP)
```php
$url = "https://portal.amootsms.com/rest/SendWithBackupLine";

$url = $url."?"."Token=".urlencode("MyToken");
$nowIran = new DateTime('now', new DateTimeZone('IRAN'));
$url = $url."&"."SendDateTime=".urlencode($nowIran->format('c'));

$url = $url."&"."SMSMessageText=".urlencode("پیامک تستی من");
$url = $url."&"."LineNumber=public";
$url = $url."&"."BackupLineNumber=98";

$url = $url."&"."Mobiles=9120000000,9150000000";

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
