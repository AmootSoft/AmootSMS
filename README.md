
# AmootSMS
## پیامک آموت
پیامک آموت (پیشرفته ترین سامانه پیامکی ایران) یک سامانه دارای وب سرویس کامل برای ارسال و دریافت پیامک و مدیریت کامل خدمات دیگر است که براحتی میتوانید از آن استفاده کنید.

آدرس وب سایت
https://www.amootsms.com

آدرس پرتال
https://portal.amootsms.com

آدرس مستندات وب سرویس 
https://doc.amootsms.com

## وب سرویس پیامک آموت - نگارش 2 (ASMX)
آدرس وب سرویس https://portal.amootsms.com/webservice2.asmx

### متد وضعیت حساب کاربری (AccountStatus)
از طریق این متد می توانید تمامی اطلاعات حساب کاربری خود از قبیل اعتبار باقیمانده و موجود در پنل ، تعرفه های ارسال و لیست خطوط را دریافت نمایید .
#### پارامترهای ورودی AccountStatus
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|UserName|String *|نام کاربری شما در سامانه پیامک آموت|
|Password|String *|رمز عبور شما در سامانه پیامک آموت|
#### نمونه کد AccountStatus (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";

AmootSMS.AmootSMSWebService2SoapClient client = new AmootSMS.AmootSMSWebService2SoapClient("AmootSMSWebService2Soap12");

AmootSMS.AccountStatusResult result = client.AccountStatus(UserName, Password);

if (result.Status == AmootSMS.Status.Success)
{
    //خروجی
}
```
#### نمونه کد AccountStatus (PHP)
```php
$url = "https://portal.amootsms.com/webservice2.asmx/AccountStatus_REST";

$url = $url."?"."UserName=MyUserName";
$url = $url."&"."Password=MyPassword";

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;


```
#### نمونه کد AccountStatus (PHP)
```php
ini_set("soap.wsdl_cache_enabled", "0");

$sms_client = new SoapClient('https://portal.amootsms.com/webservice2.asmx?wsdl', array('encoding'=>'UTF-8'));

$parameters['UserName'] = "MyUserName";
$parameters['Password'] = "MyPassword";

$result = $sms_client->AccountStatus($parameters)->AccountStatusResult;
echo $result;//خروجی


```
### متد ارسال پیامک (SendSimple)
از طریق این متد می توانید با وارد کردن متن  و شماره  ، پیامک های خود را به شماره یا شماره های انتخابی ارسال نمایید.
#### پارامترهای ورودی SendSimple
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|UserName|String *|نام کاربری شما در سامانه پیامک آموت|
|Password|String *|رمز عبور شما در سامانه پیامک آموت|
|SendDateTime|Object *|زمان ارسال پیامک|
|SMSMessageText|String *|متن پیامک|
|LineNumber|String *|شماره خط|
|Mobiles|Object *|لیست موبایل های دریافت کنندگان پیامک|
#### نمونه کد SendSimple (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";
DateTime SendDateTime = DateTime.Now;
string SMSMessageText = "پیامک تستی من";
string LineNumber = "public";
string[] Mobiles = new string[]
{
    "9120000000",
    "9150000000",
};

AmootSMS.AmootSMSWebService2SoapClient client = new AmootSMS.AmootSMSWebService2SoapClient("AmootSMSWebService2Soap12");

AmootSMS.SendResult result = client.SendSimple(UserName, Password, SendDateTime, SMSMessageText, LineNumber, Mobiles);

if (result.Status == AmootSMS.Status.Success)
{
    //خروجی
}
```
#### نمونه کد SendSimple (PHP)
```php
$url = "https://portal.amootsms.com/webservice2.asmx/SendSimple_REST";

$url = $url."?"."UserName=MyUserName";
$url = $url."&"."Password=MyPassword";

$nowIran = new DateTime('now', new DateTimeZone('IRAN'));
$url = $url."&"."SendDateTime=".$nowIran->format('c');

$url = $url."&"."SMSMessageText=".urlencode("پیامک تستی من");
$url = $url."&"."LineNumber=public";

$url = $url."&"."Mobiles=9120000000,9150000000";

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;

```
#### نمونه کد SendSimple (PHP)
```php
ini_set("soap.wsdl_cache_enabled", "0");

$sms_client = new SoapClient('https://portal.amootsms.com/webservice2.asmx?wsdl', array('encoding'=>'UTF-8'));

$parameters['UserName'] = "MyUserName";
$parameters['Password'] = "MyPassword";

$nowIran = new DateTime('now', new DateTimeZone('IRAN'));
$parameters['SendDateTime'] = $nowIran->format('c');

$parameters['SMSMessageText'] = "پیامک تستی من";
$parameters['LineNumber'] = "public";
$parameters['Mobiles'] =array("9120000000", "9150000000");

$result = $sms_client->SendSimple($parameters)->SendSimpleResult;
echo $result;
```
#### نمونه کد SendSimple (NodeJS)
```nodejs
///////////// Method GET =>

const https = require('https');

var url = 'https://portal.amootsms.com/webservice2.asmx/SendSimple_REST';
url += '?UserName=MyUserName';
url += '&Password=MyPassword';
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
    'UserName': 'MyUserName',
    'Password': 'MyPassword',
    'SendDateTime': '2020-01-01 12:00:00',
    'SMSMessageText': 'تست',
    'LineNumber': 'public',
    'Mobiles': '09159999999,09129999999'
});

var options = {
    hostname: 'portal.amootsms.com',
    port: 443,
    path: '/webservice2.asmx/SendSimple_REST',
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
از طریق این متد می توانید پیامک با رمز یکبار مصرف را به صورت فوری به شماره انتخابی خود ارسال نمایید ، جهت استفاده از این متد باید نام مجوعه خود را اعلام نموده باشید.
#### پارامترهای ورودی SendQuickOTP
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|UserName|String *|نام کاربری شما در سامانه پیامک آموت|
|Password|String *|رمز عبور شما در سامانه پیامک آموت|
|Mobile|String *|موبایل دریافت کننده پیامک|
|CodeLength|Short *|طول کد (حداقل 4 رقم و حداکثر 8 رقم)|
|OptionalCode|String *|رمز یکبار مصرف سفارشی خود اگر خالی باشد رمز یکبار مصرف سمت سرور تولید می شود|
#### نمونه کد SendQuickOTP (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";
string Mobile = "9120000000";
short CodeLength = 4;
string OptionalCode = "";

AmootSMS.AmootSMSWebService2SoapClient client = new AmootSMS.AmootSMSWebService2SoapClient("AmootSMSWebService2Soap12");

AmootSMS.SendOTPResult result = client.SendQuickOTP(UserName, Password, Mobile, CodeLength, OptionalCode);

if (result.Status == AmootSMS.Status.Success)
{
    //خروجی
}
```
#### نمونه کد SendQuickOTP (PHP)
```php
$url = "https://portal.amootsms.com/webservice2.asmx/SendQuickOTP_REST";

$url = $url."?"."UserName=MyUserName";
$url = $url."&"."Password=MyPassword";
$url = $url."&"."Mobile=9120000000";
$url = $url."&"."CodeLength=4";
$url = $url."&"."OptionalCode=";

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
#### نمونه کد SendQuickOTP (PHP)
```php
ini_set("soap.wsdl_cache_enabled", "0");

$sms_client = new SoapClient('https://portal.amootsms.com/webservice2.asmx?wsdl', array('encoding'=>'UTF-8'));

$parameters['UserName'] = "MyUserName";
$parameters['Password'] = "MyPassword";
$parameters['Mobile'] = "9120000000";
$parameters['CodeLength'] = 4;
$parameters['OptionalCode'] = "";

$result = $sms_client->SendQuickOTP($parameters)->SendQuickOTPResult;
echo $result;
```
### متد ارسال از طریق الگو (پترن) (SendWithPattern)
در صورتی که الگوی پیامک خود را در پنل کاربری ثبت نموده باشید با استفاده از این متد می توانید الگوی خود را به صورت فوری و به شماره انتخابی ارسال نمایید.

نکته : شما باید الگوی پیامک خود را در پنل کاربری پیامک ---->  گزینه ی امکانات ---> گزینه ی پیامکهای با الگو ثبت نمایید و پس تایید توسط اپراتور مرکز مخابرات و فعال شدن ، میتوانید الگوی ثبت شده را با استفاده از این متد ارسال نمایید.
#### پارامترهای ورودی SendWithPattern
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|UserName|String *|نام کاربری شما در سامانه پیامک آموت|
|Password|String *|رمز عبور شما در سامانه پیامک آموت|
|Mobile|String *|موبایل دریافت کننده پیامک|
|PatternCodeID|Int *|کد الگوی پیامک|
|PatternValues|String[] *|مقادیر الگوهای در متن پیامک بایستی به ترتیب همان الگوها در نظر گرفته شود|
#### نمونه کد SendWithPattern (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";
string Mobile = "9120000000";
int PatternCodeID = 1;
string[] PatternValues = new string[] { "پارامتر 1", "پارامتر 2" };

AmootSMS.AmootSMSWebService2SoapClient client = new AmootSMS.AmootSMSWebService2SoapClient("AmootSMSWebService2Soap12");

AmootSMS.SendResult result = client.SendWithPattern(UserName, Password, Mobile,  PatternCodeID, PatternValues);

if (result.Status == AmootSMS.Status.Success)
{
    //خروجی
}
```
#### نمونه کد SendWithPattern (PHP)
```php
$url = "https://portal.amootsms.com/webservice2.asmx/SendWithPattern_REST";

$url = $url."?"."UserName=MyUserName";
$url = $url."&"."Password=MyPassword";

$url = $url."&"."Mobile=9120000000";
$url = $url."&"."PatternCodeID=1";
$url = $url."&"."PatternValues=p1,p2";

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
#### نمونه کد SendWithPattern (PHP)
```php
ini_set("soap.wsdl_cache_enabled", "0");

$sms_client = new SoapClient('https://portal.amootsms.com/webservice2.asmx?wsdl', array('encoding'=>'UTF-8'));

$parameters['UserName'] = "MyUserName";
$parameters['Password'] = "MyPassword";
$parameters['Mobile'] = "9120000000";
$parameters['PatternCodeID'] = 1;
$parameters['PatternValues'] =array("پارامتر 1", "پارامتر 2");

$result = $sms_client->SendWithPattern($parameters)->SendWithPatternResult;
echo $result;
```
### متد محاسبه هزینه ارسال الگو (CalculatePatternMessagePrice)
از طریق این متد می توانید با وارد کردن اطلاعات مربوط به الگو ، متن پیامک و شماره های دریافت کنندگان ، هزینه ارسال با الگوی خود را محاسبه نمایید.
#### پارامترهای ورودی CalculatePatternMessagePrice
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|UserName|String *|نام کاربری شما در سامانه پیامک آموت|
|Password|String *|رمز عبور شما در سامانه پیامک آموت|
|Mobile|String *|موبایل دریافت کننده پیامک|
|PatternCodeID|Int *|کد الگوی پیامک|
|PatternValues|String[] *|مقادیر الگوهای در متن پیامک بایستی به ترتیب همان الگوها در نظر گرفته شود|
#### نمونه کد CalculatePatternMessagePrice (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";
string Mobile = "9120000000";
int PatternCodeID = 1;
string[] PatternValues = new string[] { "پارامتر 1", "پارامتر 2" };

AmootSMS.AmootSMSWebService2SoapClient client = new AmootSMS.AmootSMSWebService2SoapClient("AmootSMSWebService2Soap12");

int result = client.CalculatePatternMessagePrice(UserName, Password, Mobile, PatternCodeID, PatternValues);

if (result > 0)
{
    //خروجی
}
```
#### نمونه کد CalculatePatternMessagePrice (PHP)
```php
$url = "https://portal.amootsms.com/webservice2.asmx/CalculatePatternMessagePrice_REST";

$url = $url."?"."UserName=MyUserName";
$url = $url."&"."Password=MyPassword";

$url = $url."&"."Mobile=9120000000";
$url = $url."&"."PatternCodeID=1";
$url = $url."&"."PatternValues=p1,p2";

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
#### نمونه کد CalculatePatternMessagePrice (PHP)
```php
ini_set("soap.wsdl_cache_enabled", "0");

$sms_client = new SoapClient('https://portal.amootsms.com/webservice2.asmx?wsdl', array('encoding'=>'UTF-8'));

$parameters['UserName'] = "MyUserName";
$parameters['Password'] = "MyPassword";
$parameters['Mobile'] = "9120000000";
$parameters['PatternCodeID'] = 1;
$parameters['PatternValues'] =array("پارامتر 1", "پارامتر 2");

$result = $sms_client->CalculatePatternMessagePrice($parameters)->CalculatePatternMessagePriceResult;
echo $result;
```
### متد ایجاد مخاطب (ContactCreate)
از طریق این متد می توانید با وارد کردن اطلاعات مخاطب ، آن شخص را در دفترچه تلفن  پنل کاربری خود ذخیره نمائید.
#### پارامترهای ورودی ContactCreate
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|UserName|String *|نام کاربری شما در سامانه پیامک آموت|
|Password|String *|رمز عبور شما در سامانه پیامک آموت|
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
string UserName = "MyUserName";
string Password = "MyPassword";

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

AmootSMS.AmootSMSWebService2SoapClient client = new AmootSMS.AmootSMSWebService2SoapClient("AmootSMSWebService2Soap12");

AmootSMS.ContactCreateEditResult result = client.ContactCreate(UserName, Password, 
        ContactGroupID,
        Active,
        Mobile,
        FName,
        LName,
        GenderType,
        CompanyTitle,
        JobTitle,
        Email,
        CityName,
        AddressText,
        BornDate,
        AnniversaryDate,
        CustomText1,
        CustomText2,
        CustomText3,
        CustomText4,
        CustomText5,
        CustomText6,
        CustomDate1,
        CustomDate2,
        CustomDate3,
        Labels );

if (result.Status == AmootSMS.Status.Success)
{
    //خروجی
}
```
#### نمونه کد ContactCreate (PHP)
```php
$url = "https://portal.amootsms.com/webservice2.asmx/ContactCreate_REST";

$url = $url."?"."UserName=MyUserName";
$url = $url."&"."Password=MyPassword";

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
#### نمونه کد ContactCreate (PHP)
```php
ini_set("soap.wsdl_cache_enabled", "0");

$sms_client = new SoapClient('https://portal.amootsms.com/webservice2.asmx?wsdl', array('encoding'=>'UTF-8'));

$parameters['UserName'] = "MyUserName";
$parameters['Password'] = "MyPassword";

//غیر فعال میباشد
//از پارامتر
// Labels
//استفاده کنید
$parameters['ContactGroupID'] = 0;

$parameters['Active'] = true;
$parameters['Mobile'] = '09120000000';
$parameters['FName'] = null;
$parameters['LName'] = null;
$parameters['GenderType'] = null;
$parameters['CompanyTitle'] = null;
$parameters['JobTitle'] = null;
$parameters['Email'] = null;
$parameters['CityName'] = null;
$parameters['AddressText'] = null;
$parameters['BornDate'] = null;
$parameters['AnniversaryDate'] = null;
$parameters['CustomText1'] = null;
$parameters['CustomText2'] = null;
$parameters['CustomText3'] = null;
$parameters['CustomText4'] = null;
$parameters['CustomText5'] = null;
$parameters['CustomText6'] = null;
$parameters['CustomDate1'] = null;
$parameters['CustomDate2'] = null;
$parameters['CustomDate3'] = null;
$parameters['Labels'] = array("گروه1","گروه2");

$result = $sms_client->ContactCreate($parameters)->ContactCreateResult;
echo $result;
```
### متد حذف مخاطب (ContactDelete)
از طریق این متد می توانید یک مخاطب را از دفترچه تلفن پنل کاربری خود حذف نمائید.
#### پارامترهای ورودی ContactDelete
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|UserName|String *|نام کاربری شما در سامانه پیامک آموت|
|Password|String *|رمز عبور شما در سامانه پیامک آموت|
|ContactID|Long *|کد مخاطب|
#### نمونه کد ContactDelete (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";

long ContactID = 0;

AmootSMS.AmootSMSWebService2SoapClient client = new AmootSMS.AmootSMSWebService2SoapClient("AmootSMSWebService2Soap12");

AmootSMS.ContactDeleteResult result = client.ContactDelete(UserName, Password, ContactID);

if (result.Status == AmootSMS.Status.Success)
{
    //خروجی
}
```
#### نمونه کد ContactDelete (PHP)
```php
$url = "https://portal.amootsms.com/webservice2.asmx/ContactDelete_REST";

$url = $url."?"."UserName=MyUserName";
$url = $url."&"."Password=MyPassword";

$url = $url."&"."ContactID=0";

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
#### نمونه کد ContactDelete (PHP)
```php
ini_set("soap.wsdl_cache_enabled", "0");

$sms_client = new SoapClient('https://portal.amootsms.com/webservice2.asmx?wsdl', array('encoding'=>'UTF-8'));

$parameters['UserName'] = "MyUserName";
$parameters['Password'] = "MyPassword";

$parameters['ContactID'] = 0;

$result = $sms_client->ContactDelete($parameters)->ContactDeleteResult;
echo $result;
```
### متد ارسال با گارانتی آوانک (SendWithAvanak)
از طریق این متد می توانید پیامک های خود را با گارانتی پیام صوتی آوانک ارسال نمایید، در صورتی که  شماره دریافت کننده در لیست سیاه باشد ارسال پیام صوتی به آن شماره انجام میگردد .
#### پارامترهای ورودی SendWithAvanak
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|UserName|String *|نام کاربری شما در سامانه پیامک آموت|
|Password|String *|رمز عبور شما در سامانه پیامک آموت|
|SendDateTime|Object *|زمان ارسال پیامک|
|SMSMessageText|String *|متن پیامک|
|AvanakMessageText|String *|متن پیام صوتی آوانک|
|SpeakerType|String *|نوع گوینده پیام صوتی female = 1, male = 2|
|LineNumber|String *|شماره خط|
|Mobiles|Object *|لیست موبایل های دریافت کنندگان پیامک|
#### نمونه کد SendWithAvanak (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";
DateTime SendDateTime = DateTime.Now;
string SMSMessageText = "پیامک تستی من";
string AvanakMessageText = "پیام صوتی تستی من";
string LineNumber = "public";
AmootSMS.AvanakSpeakerType AvanakSpeakerType = AmootSMS.AvanakSpeakerType.female;
string[] Mobiles = new string[]
{
    "9120000000",
    "9150000000",
};

AmootSMS.AmootSMSWebService2SoapClient client = new AmootSMS.AmootSMSWebService2SoapClient("AmootSMSWebService2Soap12");

AmootSMS.SendResult result = client.SendWithAvanak(UserName, Password, SendDateTime, SMSMessageText, AvanakMessageText, AvanakSpeakerType, LineNumber, Mobiles);

if (result.Status == AmootSMS.Status.Success)
{
    //خروجی
}
```
#### نمونه کد SendWithAvanak (PHP)
```php
$url = "https://portal.amootsms.com/webservice2.asmx/SendWithAvanak_REST";

$url = $url."?"."UserName=MyUserName";
$url = $url."&"."Password=MyPassword";

$nowIran = new DateTime('now', new DateTimeZone('IRAN'));
$url = $url."&"."SendDateTime=".$nowIran->format('c');

$url = $url."&"."SMSMessageText=".urlencode("پیامک تستی من");
$url = $url."&"."AvanakMessageText=".urlencode("پیام صوتی تستی من");
$url = $url."&"."AvanakSpeakerType=female";
$url = $url."&"."LineNumber=public";

$url = $url."&"."Mobiles=9120000000,9150000000";

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
#### نمونه کد SendWithAvanak (PHP)
```php
ini_set("soap.wsdl_cache_enabled", "0");

$sms_client = new SoapClient('https://portal.amootsms.com/webservice2.asmx?wsdl', array('encoding'=>'UTF-8'));

$parameters['UserName'] = "MyUserName";
$parameters['Password'] = "MyPassword";

$nowIran = new DateTime('now', new DateTimeZone('IRAN'));
$parameters['SendDateTime'] = $nowIran->format('c');

$parameters['SMSMessageText'] = "پیامک تستی من";
$parameters['AvanakMessageText'] = "پیام صوتی تستی من";
$parameters['AvanakSpeakerType'] = "female";
$parameters['LineNumber'] = "public";
$parameters['Mobiles'] =array("9120000000", "9150000000");

$result = $sms_client->SendWithAvanak($parameters)->SendWithAvanakResult;
echo $result;
```
### متد ویرایش مخاطب (ContactEdit)
از طریق این متد می توانید با وارد کردن اطلاعات مخاطب ، آن شخص را در دفترچه تلفن  پنل کاربری ویرایش و ذخیره نمائید.
#### پارامترهای ورودی ContactEdit
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|UserName|String *|نام کاربری شما در سامانه پیامک آموت|
|Password|String *|رمز عبور شما در سامانه پیامک آموت|
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
string UserName = "MyUserName";
string Password = "MyPassword";

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

AmootSMS.AmootSMSWebService2SoapClient client = new AmootSMS.AmootSMSWebService2SoapClient("AmootSMSWebService2Soap12");

AmootSMS.ContactCreateEditResult result = client.ContactEdit(UserName, Password,

        ContactID,

        ContactGroupID,
        Active,
        Mobile,
        FName,
        LName,
        GenderType,
        CompanyTitle,
        JobTitle,
        Email,
        CityName,
        AddressText,
        BornDate,
        AnniversaryDate,
        CustomText1,
        CustomText2,
        CustomText3,
        CustomText4,
        CustomText5,
        CustomText6,
        CustomDate1,
        CustomDate2,
        CustomDate3);

if (result.Status == AmootSMS.Status.Success)
{
    //خروجی
}
```
#### نمونه کد ContactEdit (PHP)
```php
$url = "https://portal.amootsms.com/webservice2.asmx/ContactEdit_REST";

$url = $url."?"."UserName=MyUserName";
$url = $url."&"."Password=MyPassword";

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
#### نمونه کد ContactEdit (PHP)
```php
ini_set("soap.wsdl_cache_enabled", "0");

$sms_client = new SoapClient('https://portal.amootsms.com/webservice2.asmx?wsdl', array('encoding'=>'UTF-8'));

$parameters['UserName'] = "MyUserName";
$parameters['Password'] = "MyPassword";

$parameters['ContactID'] = 0;

$parameters['ContactGroupID'] = 0;
$parameters['Active'] = true;
$parameters['Mobile'] = '09120000000';
$parameters['FName'] = null;
$parameters['LName'] = null;
$parameters['GenderType'] = null;
$parameters['CompanyTitle'] = null;
$parameters['JobTitle'] = null;
$parameters['Email'] = null;
$parameters['CityName'] = null;
$parameters['AddressText'] = null;
$parameters['BornDate'] = null;
$parameters['AnniversaryDate'] = null;
$parameters['CustomText1'] = null;
$parameters['CustomText2'] = null;
$parameters['CustomText3'] = null;
$parameters['CustomText4'] = null;
$parameters['CustomText5'] = null;
$parameters['CustomText6'] = null;
$parameters['CustomDate1'] = null;
$parameters['CustomDate2'] = null;
$parameters['CustomDate3'] = null;

$result = $sms_client->ContactEdit($parameters)->ContactEditResult;
echo $result;
```
### متد دریافت اطلاعات مخاطب (ContactGet)
از طریق این متد می توانید اطلاعات  مربوط به مخاطب خود را از پنل  کاربری خود دریافت نمایید.
#### پارامترهای ورودی ContactGet
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|UserName|String *|نام کاربری شما در سامانه پیامک آموت|
|Password|String *|رمز عبور شما در سامانه پیامک آموت|
|ContactID|Long *|کد مخاطب|
#### نمونه کد ContactGet (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";

long ContactID = 0;

AmootSMS.AmootSMSWebService2SoapClient client = new AmootSMS.AmootSMSWebService2SoapClient("AmootSMSWebService2Soap12");

AmootSMS.GetContactResult result = client.ContactGet(UserName, Password, ContactID);

if (result.Status == AmootSMS.Status.Success)
{
    //خروجی
}
```
#### نمونه کد ContactGet (PHP)
```php
$url = "https://portal.amootsms.com/webservice2.asmx/ContactGet_REST";

$url = $url."?"."UserName=MyUserName";
$url = $url."&"."Password=MyPassword";

$url = $url."&"."ContactID=0";

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
#### نمونه کد ContactGet (PHP)
```php
ini_set("soap.wsdl_cache_enabled", "0");

$sms_client = new SoapClient('https://portal.amootsms.com/webservice2.asmx?wsdl', array('encoding'=>'UTF-8'));

$parameters['UserName'] = "MyUserName";
$parameters['Password'] = "MyPassword";

$parameters['ContactID'] = 0;

$result = $sms_client->ContactGet($parameters)->ContactGetResult;
echo $result;
```
### متد دریافت لیست گروههای (برچسب های) مخاطبین (ContactGroupList)
از طریق این متد می توانید لیست گروههای (برچسب های) مخاطبین موجود در دفترچه تلفن دریافت نمائید.
#### پارامترهای ورودی ContactGroupList
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|UserName|String *|نام کاربری شما در سامانه پیامک آموت|
|Password|String *|رمز عبور شما در سامانه پیامک آموت|
#### نمونه کد ContactGroupList (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";

AmootSMS.AmootSMSWebService2SoapClient client = new AmootSMS.AmootSMSWebService2SoapClient("AmootSMSWebService2Soap12");

AmootSMS.AccountStatusResult result = client.ContactGroupList(UserName, Password);

if (result.Status == AmootSMS.Status.Success)
{
    //خروجی
}
```
#### نمونه کد ContactGroupList (PHP)
```php
$url = "https://portal.amootsms.com/webservice2.asmx/ContactGroupList_REST";

$url = $url."?"."UserName=MyUserName";
$url = $url."&"."Password=MyPassword";

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
#### نمونه کد ContactGroupList (PHP)
```php
ini_set("soap.wsdl_cache_enabled", "0");

$sms_client = new SoapClient('https://portal.amootsms.com/webservice2.asmx?wsdl', array('encoding'=>'UTF-8'));

$parameters['UserName'] = "MyUserName";
$parameters['Password'] = "MyPassword";

$result = $sms_client->ContactGroupList($parameters)->AccountStatusResult;
echo $result;//خروجی
```
### متد دریافت لیست مخاطبین گروه (ContactList)
از طریق این متد می توانید با وارد کردن اطلاعات گروه (کد برچسب یا گروه مخاطب) ، مخاطبین موجود در آن گروه بندی  را دریافت نمایید .
#### پارامترهای ورودی ContactList
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|UserName|String *|نام کاربری شما در سامانه پیامک آموت|
|Password|String *|رمز عبور شما در سامانه پیامک آموت|
|ContactGroupID|Long *|کد گروه مخاطبین (برچسب)|
#### نمونه کد ContactList (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";

long ContactGroupID = 0;

AmootSMS.AmootSMSWebService2SoapClient client = new AmootSMS.AmootSMSWebService2SoapClient("AmootSMSWebService2Soap12");

AmootSMS.ContactListResult result = client.ContactList(UserName, Password, ContactGroupID);

if (result.Status == AmootSMS.Status.Success)
{
    //خروجی
}
```
#### نمونه کد ContactList (PHP)
```php
$url = "https://portal.amootsms.com/webservice2.asmx/ContactList_REST";

$url = $url."?"."UserName=MyUserName";
$url = $url."&"."Password=MyPassword";

$url = $url."&"."ContactGroupID=0";

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
#### نمونه کد ContactList (PHP)
```php
ini_set("soap.wsdl_cache_enabled", "0");

$sms_client = new SoapClient('https://portal.amootsms.com/webservice2.asmx?wsdl', array('encoding'=>'UTF-8'));

$parameters['UserName'] = "MyUserName";
$parameters['Password'] = "MyPassword";

$parameters['ContactGroupID'] = 0;

$result = $sms_client->ContactList($parameters)->ContactListResult;
echo $result;
```
### متد ContactListUpdates (ContactListUpdates)
از طریق این متد می توانید برای sync کردن مخاطبین خود استفاده نمایید.
#### پارامترهای ورودی ContactListUpdates
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|UserName|String *|نام کاربری شما در سامانه پیامک آموت|
|Password|String *|رمز عبور شما در سامانه پیامک آموت|
|ContactGroupID|Long *|کد گروه مخاطبین|
#### نمونه کد ContactListUpdates (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";

long ContactGroupID = 0;

AmootSMS.AmootSMSWebService2SoapClient client = new AmootSMS.AmootSMSWebService2SoapClient("AmootSMSWebService2Soap12");

AmootSMS.ContactListUpdatesResult result = client.ContactListUpdates(UserName, Password, ContactGroupID);

if (result.Status == AmootSMS.Status.Success)
{
    //خروجی
}
```
#### نمونه کد ContactListUpdates (PHP)
```php
$url = "https://portal.amootsms.com/webservice2.asmx/ContactListUpdates_REST";

$url = $url."?"."UserName=MyUserName";
$url = $url."&"."Password=MyPassword";

$url = $url."&"."ContactGroupID=0";

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
#### نمونه کد ContactListUpdates (PHP)
```php
ini_set("soap.wsdl_cache_enabled", "0");

$sms_client = new SoapClient('https://portal.amootsms.com/webservice2.asmx?wsdl', array('encoding'=>'UTF-8'));

$parameters['UserName'] = "MyUserName";
$parameters['Password'] = "MyPassword";

$parameters['ContactGroupID'] = 0;

$result = $sms_client->ContactListUpdates($parameters)->ContactListUpdatesResult;
echo $result;

```
### متد ایجاد ارسال هوشمند دوره ای (CourseCreateSimple)
از طریق این متد می توانید یک برنامه ارسال دوره ای هوشمند را ایجاد نمایید ، در صورتی که پنل دارای اعتبار باشد ارسال ها به صورت خود کار انجام می گردد.
#### پارامترهای ورودی CourseCreateSimple
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|UserName|String *|نام کاربری شما در سامانه پیامک آموت|
|Password|String *|رمز عبور شما در سامانه پیامک آموت|
|CourseGroupID|Long *|کد گروه برنامه ارسال دوره ای هوشمند|
|CourseType|String *|نوع برنامه هوشمند Yearly = 0, Monthly = 1, Weekly = 2, Daily = 3,|
|CourseDateTime|Object *|زمان شروع برنامه|
|Title|String *|عنوان|
|Mobile|String *|موبایل|
|SMSMessageText|String *|متن پیامک|
|LineNumber|String *|شماره خط|
#### نمونه کد CourseCreateSimple (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";

long CourseGroupID = 0;
AmootSMS.CourseType CourseType = AmootSMS.CourseType.Daily;
DateTime CourseDateTime = DateTime.Now.Date;
string Title = "";
string Mobile = "9120000000";
string SMSMessageText = "";
string LineNumber = "public";


AmootSMS.AmootSMSWebService2SoapClient client = new AmootSMS.AmootSMSWebService2SoapClient("AmootSMSWebService2Soap12");

AmootSMS.CourseCreateResult result = client.CourseCreateSimple(UserName, Password,
    CourseGroupID,
    CourseType,
    CourseDateTime,
    Title,
    Mobile,
    SMSMessageText,
    LineNumber
    );

if (result.Status == AmootSMS.Status.Success)
{
    //خروجی
}
```
#### نمونه کد CourseCreateSimple (PHP)
```php
$url = "https://portal.amootsms.com/webservice2.asmx/CourseCreateSimple_REST";

$url = $url."?"."UserName=MyUserName";
$url = $url."&"."Password=MyPassword";

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
#### نمونه کد CourseCreateSimple (PHP)
```php
ini_set("soap.wsdl_cache_enabled", "0");

$sms_client = new SoapClient('https://portal.amootsms.com/webservice2.asmx?wsdl', array('encoding'=>'UTF-8'));

$parameters['UserName'] = "MyUserName";
$parameters['Password'] = "MyPassword";

$parameters['CourseGroupID'] = 0;
$parameters['CourseType'] = "Daily";

$nowIran = new DateTime('now', new DateTimeZone('IRAN'));
$parameters['CourseDateTime'] = $nowIran->format('c');

$parameters['Title'] = "";
$parameters['Mobile'] = "9120000000";
$parameters['SMSMessageText'] = "";
$parameters['LineNumber'] = "public";

$result = $sms_client->CourseCreateSimple($parameters)->CourseCreateSimpleResult;
echo $result;
```
### متد ایجاد ارسال هوشمند دوره ای با گارانتی آوانک (CourseCreateWithAvanak)
از طریق این متد می توانید یک برنامه ارسال دوره ای هوشمند را ایجاد نمایید ، در صورتی که  شماره دریافت کننده در لیست سیاه باشد ارسال پیام صوتی به آن شماره انجام میگردد .
#### پارامترهای ورودی CourseCreateWithAvanak
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|UserName|String *|نام کاربری شما در سامانه پیامک آموت|
|Password|String *|رمز عبور شما در سامانه پیامک آموت|
|CourseGroupID|Long *|کد گروه برنامه ارسال دوره ای هوشمند|
|CourseType|String *|نوع برنامه هوشمند Yearly = 0, Monthly = 1, Weekly = 2, Daily = 3,|
|CourseDateTime|Object *|زمان شروع برنامه|
|Title|String *|عنوان|
|Mobile|String *|موبایل|
|SMSMessageText|String *|متن پیامک|
|AvanakMessageText|String *|متن پیام صوتی آوانک|
|SpeakerType|String *|نوع گوینده پیام صوتی female = 1, male = 2|
|LineNumber|String *|شماره خط|
#### نمونه کد CourseCreateWithAvanak (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";

long CourseGroupID = 0;
AmootSMS.CourseType CourseType = AmootSMS.CourseType.Daily;
DateTime CourseDateTime = DateTime.Now.Date;
string Title = "";
string Mobile = "9120000000";
string SMSMessageText = "";
string AvanakMessageText = "پیام صوتی تستی من";
AmootSMS.AvanakSpeakerType AvanakSpeakerType = AmootSMS.AvanakSpeakerType.female;
string LineNumber = "public";



AmootSMS.AmootSMSWebService2SoapClient client = new AmootSMS.AmootSMSWebService2SoapClient("AmootSMSWebService2Soap12");

AmootSMS.CourseCreateResult result = client.CourseCreateWithAvanak(UserName, Password,
    CourseGroupID,
    CourseType,
    CourseDateTime,
    Title,
    Mobile,
    SMSMessageText,
    AvanakMessageText,
    AvanakSpeakerType,
    LineNumber
    );

if (result.Status == AmootSMS.Status.Success)
{
    //خروجی
}
```
#### نمونه کد CourseCreateWithAvanak (PHP)
```php
$url = "https://portal.amootsms.com/webservice2.asmx/CourseCreateWithAvanak_REST";

$url = $url."?"."UserName=MyUserName";
$url = $url."&"."Password=MyPassword";

$url = $url."&"."CourseGroupID=0";
$url = $url."&"."CourseType=Daily";

$nowIran = new DateTime('now', new DateTimeZone('IRAN'));
$url = $url."&"."CourseDateTime=".$nowIran->format('c');

$url = $url."&"."Title=MyTitle";
$url = $url."&"."Mobile=9120000000";
$url = $url."&"."SMSMessageText=".urlencode("پیامک تستی من");
$url = $url."&"."AvanakMessageText=".urlencode("پیام صوتی تستی من");
$url = $url."&"."AvanakSpeakerType=female";
$url = $url."&"."LineNumber=public";
$url = $url."&"."BackupLineNumber=98";

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
#### نمونه کد CourseCreateWithAvanak (PHP)
```php
ini_set("soap.wsdl_cache_enabled", "0");

$sms_client = new SoapClient('https://portal.amootsms.com/webservice2.asmx?wsdl', array('encoding'=>'UTF-8'));

$parameters['UserName'] = "MyUserName";
$parameters['Password'] = "MyPassword";

$parameters['CourseGroupID'] = 0;
$parameters['CourseType'] = "Daily";

$nowIran = new DateTime('now', new DateTimeZone('IRAN'));
$parameters['CourseDateTime'] = $nowIran->format('c');

$parameters['Title'] = "";
$parameters['Mobile'] = "9120000000";
$parameters['SMSMessageText'] = "پیامک تستی من";
$parameters['AvanakMessageText'] = "پیام صوتی تستی من";
$parameters['AvanakSpeakerType'] = "female";
$parameters['LineNumber'] = "public";

$result = $sms_client->CourseCreateWithAvanak($parameters)->CourseCreateWithAvanakResult;
echo $result;
```
### متد ایجاد ارسال هوشمند دوره ای با گارانتی خط تبلیغاتی (CourseCreateWithBackupLine)
از طریق این متد می توانید یک برنامه ارسال دوره ای هوشمند را ایجاد نمایید ، در صورتی که  شماره دریافت کننده در لیست سیاه باشد از طریق خط تبلیغاتی (98) به ایشان ارسال انجام میگردد.
#### پارامترهای ورودی CourseCreateWithBackupLine
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|UserName|String *|نام کاربری شما در سامانه پیامک آموت|
|Password|String *|رمز عبور شما در سامانه پیامک آموت|
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
string UserName = "MyUserName";
string Password = "MyPassword";

long CourseGroupID = 0;
AmootSMS.CourseType CourseType = AmootSMS.CourseType.Daily;
DateTime CourseDateTime = DateTime.Now.Date;
string Title = "";
string Mobile = "9120000000";
string SMSMessageText = "پیامک تستی من";
string LineNumber = "public";
string BackupLineNumber = "98";

AmootSMS.AmootSMSWebService2SoapClient client = new AmootSMS.AmootSMSWebService2SoapClient("AmootSMSWebService2Soap12");

AmootSMS.CourseCreateResult result = client.CourseCreateWithBackupLine(UserName, Password,
    CourseGroupID,
    CourseType,
    CourseDateTime,
    Title,
    Mobile,
    SMSMessageText,
    LineNumber,
    BackupLineNumber
    );

if (result.Status == AmootSMS.Status.Success)
{
    //خروجی
}
```
#### نمونه کد CourseCreateWithBackupLine (PHP)
```php
$url = "https://portal.amootsms.com/webservice2.asmx/CourseCreateWithBackupLine_REST";

$url = $url."?"."UserName=MyUserName";
$url = $url."&"."Password=MyPassword";

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
#### نمونه کد CourseCreateWithBackupLine (PHP)
```php
ini_set("soap.wsdl_cache_enabled", "0");

$sms_client = new SoapClient('https://portal.amootsms.com/webservice2.asmx?wsdl', array('encoding'=>'UTF-8'));

$parameters['UserName'] = "MyUserName";
$parameters['Password'] = "MyPassword";

$parameters['CourseGroupID'] = 0;
$parameters['CourseType'] = "Daily";

$nowIran = new DateTime('now', new DateTimeZone('IRAN'));
$parameters['CourseDateTime'] = $nowIran->format('c');

$parameters['Title'] = "";
$parameters['Mobile'] = "9120000000";
$parameters['SMSMessageText'] = "پیامک تستی من";
$parameters['LineNumber'] = "public";
$parameters['BackupLineNumber'] = "98";

$result = $sms_client->CourseCreateWithBackupLine($parameters)->CourseCreateWithBackupLineResult;
echo $result;
```
### متد دریافت لیست برنامه های ارسال هوشمند دوره ای (CourseList)
از طریق این متد می توانید لیست برنامه های ارسال دوره ای هوشمند را دریافت نمایید.
#### پارامترهای ورودی CourseList
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|UserName|String *||
|Password|String *||
#### نمونه کد CourseList (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";

AmootSMS.AmootSMSWebService2SoapClient client = new AmootSMS.AmootSMSWebService2SoapClient("AmootSMSWebService2Soap12");

AmootSMS.CourseListResult result = client.CourseList(UserName, Password);

if (result.Status == AmootSMS.Status.Success)
{
    //خروجی
}
```
#### نمونه کد CourseList (PHP)
```php
$url = "https://portal.amootsms.com/webservice2.asmx/CourseList_REST";

$url = $url."?"."UserName=MyUserName";
$url = $url."&"."Password=MyPassword";

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
#### نمونه کد CourseList (PHP)
```php
ini_set("soap.wsdl_cache_enabled", "0");

$sms_client = new SoapClient('https://portal.amootsms.com/webservice2.asmx?wsdl', array('encoding'=>'UTF-8'));

$parameters['UserName'] = "MyUserName";
$parameters['Password'] = "MyPassword";

$result = $sms_client->CourseList($parameters)->CourseListResult;
echo $result;
```
### متد فعال کردن برنامه ارسال هوشمند دوره ای (CourseSetActive)
از طریق این متد می توانید یک برنامه ارسال دوره ای هوشمند را فعال یا غیرفعال نمایید.
#### پارامترهای ورودی CourseSetActive
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|UserName|String *|نام کاربری شما در سامانه پیامک آموت|
|Password|String *|رمز عبور شما در سامانه پیامک آموت|
|CourseID|Long *|کد برنامه ارسال دوره ای هوشمند|
|Active|Bool *|فعال بودن|
#### نمونه کد CourseSetActive (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";

long CourseID = 0;
bool Active = true;

AmootSMS.AmootSMSWebService2SoapClient client = new AmootSMS.AmootSMSWebService2SoapClient("AmootSMSWebService2Soap12");

AmootSMS.Status result = client.CourseSetActive(UserName, Password, CourseID, Active);

if (result == AmootSMS.Status.Success)
{
    //خروجی
}
```
#### نمونه کد CourseSetActive (PHP)
```php
$url = "https://portal.amootsms.com/webservice2.asmx/CourseSetActive_REST";

$url = $url."?"."UserName=MyUserName";
$url = $url."&"."Password=MyPassword";

$url = $url."&"."CourseID=0";
$url = $url."&"."Active=true";

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
#### نمونه کد CourseSetActive (PHP)
```php
ini_set("soap.wsdl_cache_enabled", "0");

$sms_client = new SoapClient('https://portal.amootsms.com/webservice2.asmx?wsdl', array('encoding'=>'UTF-8'));

$parameters['UserName'] = "MyUserName";
$parameters['Password'] = "MyPassword";

$parameters['CourseID'] = 0;
$parameters['Active'] = true;

$result = $sms_client->CourseSetActive($parameters)->CourseSetActiveResult;
echo $result;
```
### متد دریافت لیست بانک اطلاعاتی (GetBankTree)
از طریق این متد می توانید لیست درختی بانک اطلاعاتی شماره همراه را دریافت نمائید.
#### پارامترهای ورودی GetBankTree
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|UserName|String *|نام کاربری شما در سامانه پیامک آموت|
|Password|String *|رمز عبور شما در سامانه پیامک آموت|
#### نمونه کد GetBankTree (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";

AmootSMS.AmootSMSWebService2SoapClient client = new AmootSMS.AmootSMSWebService2SoapClient("AmootSMSWebService2Soap12");

AmootSMS.BankTreeResult result = client.GetBankTree(UserName, Password);

if (result.Status == AmootSMS.Status.Success)
{
    //خروجی
}
```
#### نمونه کد GetBankTree (PHP)
```php
$url = "https://portal.amootsms.com/webservice2.asmx/GetBankTree_REST";

$url = $url."?"."UserName=MyUserName";
$url = $url."&"."Password=MyPassword";

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
#### نمونه کد GetBankTree (PHP)
```php
ini_set("soap.wsdl_cache_enabled", "0");

$sms_client = new SoapClient('https://portal.amootsms.com/webservice2.asmx?wsdl', array('encoding'=>'UTF-8'));

$parameters['UserName'] = "MyUserName";
$parameters['Password'] = "MyPassword";

$result = $sms_client->GetBankTree($parameters)->GetBankTreeResult;
echo $result;
```
### متد دریافت پیامک ارسالی (GetMessage)
از طریق این متد می توانید  پیامکی که ارسال نمودید را دریافت نمایید.
#### پارامترهای ورودی GetMessage
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|UserName|String *|نام کاربری شما در سامانه پیامک آموت|
|Password|String *|رمز عبور شما در سامانه پیامک آموت|
|MessageID|Long *|کد پیام|
#### نمونه کد GetMessage (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";
long MessageID = 1000;

AmootSMS.AmootSMSWebService2SoapClient client = new AmootSMS.AmootSMSWebService2SoapClient("AmootSMSWebService2Soap12");

AmootSMS.MessageResult result = client.GetMessage(UserName, Password, MessageID);

if (result.Status == AmootSMS.Status.Success)
{
    //خروجی
}
```
#### نمونه کد GetMessage (PHP)
```php
$url = "https://portal.amootsms.com/webservice2.asmx/GetMessage_REST";

$url = $url."?"."UserName=MyUserName";
$url = $url."&"."Password=MyPassword";
$url = $url."&"."MessageID=1000";

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
#### نمونه کد GetMessage (PHP)
```php
ini_set("soap.wsdl_cache_enabled", "0");

$sms_client = new SoapClient('https://portal.amootsms.com/webservice2.asmx?wsdl', array('encoding'=>'UTF-8'));

$parameters['UserName'] = "MyUserName";
$parameters['Password'] = "MyPassword";
$parameters['MessageID'] = 1000;

$result = $sms_client->GetMessage($parameters)->GetMessageResult;
echo $result;
```
### متد دریافت پیامک های دریافتی (RecieveMessages)
از طریق این متد می توانید پیامک های ارسالی خود را  در بازه زمانی مشخص دریافت نمایید.
#### پارامترهای ورودی RecieveMessages
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|UserName|String *|نام کاربری شما در سامانه پیامک آموت|
|Password|String *|رمز عبور شما در سامانه پیامک آموت|
|FromDateTime|Object *|از تاریخ/زمان|
|ToDateTime|Object *|تا تاریخ/زمان|
#### نمونه کد RecieveMessages (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";

DateTime FromDateTime = DateTime.Now.AddMonths(-1);
DateTime ToDateTime = DateTime.Now;

AmootSMS.AmootSMSWebService2SoapClient client = new AmootSMS.AmootSMSWebService2SoapClient("AmootSMSWebService2Soap12");

AmootSMS.ReceiveResult result = client.RecieveMessages(UserName, Password, FromDateTime, ToDateTime);

if (result.Status == AmootSMS.Status.Success)
{
    //خروجی
}
```
#### نمونه کد RecieveMessages (PHP)
```php
$url = "https://portal.amootsms.com/webservice2.asmx/RecieveMessages_REST";

$url = $url."?"."UserName=MyUserName";
$url = $url."&"."Password=MyPassword";

$FromDateTime= new DateTime('2020-01-01');
$url = $url."&"."FromDateTime=".$FromDateTimeIran->format('c');

$ToDateTime= new DateTime('2020-02-01');
$url = $url."&"."ToDateTime=".$ToDateTime->format('c');

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
#### نمونه کد RecieveMessages (PHP)
```php
ini_set("soap.wsdl_cache_enabled", "0");

$sms_client = new SoapClient('https://portal.amootsms.com/webservice2.asmx?wsdl', array('encoding'=>'UTF-8'));

$parameters['UserName'] = "MyUserName";
$parameters['Password'] = "MyPassword";

$parameters['FromDateTime'] = "2020-02-01";
$parameters['ToDateTime'] = "2020-03-01";

$result = $sms_client->RecieveMessages($parameters)->RecieveMessagesResult;
echo $result;
```
### متد دریافت پیامک های جدید دریافتی (RecieveNewMessages)
از طریق این متد می توانید پیامک های جدید دریافتی خود را دریافت نمایید. 

نکته : در هر بار اجرای این متد فقط یکبار پیامک ها را به شما نشان می دهد.
#### پارامترهای ورودی RecieveNewMessages
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|UserName|String *|نام کاربری شما در سامانه پیامک آموت|
|Password|String *|رمز عبور شما در سامانه پیامک آموت|
|Count|Int *|تعداد پیامهای دریافتی|
#### نمونه کد RecieveNewMessages (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";

int Count = 10;

AmootSMS.AmootSMSWebService2SoapClient client = new AmootSMS.AmootSMSWebService2SoapClient("AmootSMSWebService2Soap12");

AmootSMS.ReceiveResult result = client.RecieveNewMessages(UserName, Password, Count);

if (result.Status == AmootSMS.Status.Success)
{
    //خروجی
}
```
#### نمونه کد RecieveNewMessages (PHP)
```php
$url = "https://portal.amootsms.com/webservice2.asmx/RecieveNewMessages_REST";

$url = $url."?"."UserName=MyUserName";
$url = $url."&"."Password=MyPassword";
$url = $url."&"."Count=100";

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
#### نمونه کد RecieveNewMessages (PHP)
```php
ini_set("soap.wsdl_cache_enabled", "0");

$sms_client = new SoapClient('https://portal.amootsms.com/webservice2.asmx?wsdl', array('encoding'=>'UTF-8'));

$parameters['UserName'] = "MyUserName";
$parameters['Password'] = "MyPassword";

$parameters['Count'] = 10;

$result = $sms_client->RecieveNewMessages($parameters)->RecieveNewMessagesResult;
echo $result;
```
### متد افزودن آدرس جهت دریافت دلیوری (RelayMessageDeliveryCreate)
از طریق این متد می تواند یک آدرس جدید به لیست آدرسهای "انتقال دلیوری پیامهای ارسالی" اضافه کند.
#### پارامترهای ورودی RelayMessageDeliveryCreate
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|UserName|String *|نام کاربری شما در سامانه پیامک آموت|
|Password|String *|رمز عبور شما در سامانه پیامک آموت|
|URLAddress|String *|آدرس URL|
#### نمونه کد RelayMessageDeliveryCreate (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";

string URLAddress = "http://Mysite.com/RelayMessageDelivery";

AmootSMS.AmootSMSWebService2SoapClient client = new AmootSMS.AmootSMSWebService2SoapClient("AmootSMSWebService2Soap12");

AmootSMS.RelayMessageDeliveryCreateResult result = client.RelayMessageDeliveryCreate(UserName, Password, URLAddress);

if (result.Status == AmootSMS.Status.Success)
{
    //خروجی
}
```
#### نمونه کد RelayMessageDeliveryCreate (PHP)
```php
$url = "https://portal.amootsms.com/webservice2.asmx/RelayMessageDeliveryCreate_REST";

$url = $url."?"."UserName=MyUserName";
$url = $url."&"."Password=MyPassword";
$url = $url."&"."URLAddress=".urlencode( "http://Mysite.com/RelayMessageDelivery");

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
#### نمونه کد RelayMessageDeliveryCreate (PHP)
```php
ini_set("soap.wsdl_cache_enabled", "0");

$sms_client = new SoapClient('https://portal.amootsms.com/webservice2.asmx?wsdl', array('encoding'=>'UTF-8'));

$parameters['UserName'] = "MyUserName";
$parameters['Password'] = "MyPassword";

$parameters['URLAddress'] = "http://Mysite.com/RelayMessageDelivery";

$result = $sms_client->RelayMessageDeliveryCreate($parameters)->RelayMessageDeliveryCreateResult;
echo $result;
```
### متد دریافت لیست آدرس های دریافت دلیوری (RelayMessageDeliveryList)
از طریق این متد می توانید لیست آدرسهای "انتقال دلیوری پیامهای ارسالی" را دریافت نمایید.
#### پارامترهای ورودی RelayMessageDeliveryList
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|UserName|String *|نام کاربری شما در سامانه پیامک آموت|
|Password|String *|رمز عبور شما در سامانه پیامک آموت|
#### نمونه کد RelayMessageDeliveryList (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";

AmootSMS.AmootSMSWebService2SoapClient client = new AmootSMS.AmootSMSWebService2SoapClient("AmootSMSWebService2Soap12");

AmootSMS.RelayMessageDeliveryListResult result = client.RelayMessageDeliveryList(UserName, Password);

if (result.Status == AmootSMS.Status.Success)
{
    //خروجی
}
```
#### نمونه کد RelayMessageDeliveryList (PHP)
```php
$url = "https://portal.amootsms.com/webservice2.asmx/RelayMessageDeliveryList_REST";

$url = $url."?"."UserName=MyUserName";
$url = $url."&"."Password=MyPassword";

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
#### نمونه کد RelayMessageDeliveryList (PHP)
```php
ini_set("soap.wsdl_cache_enabled", "0");

$sms_client = new SoapClient('https://portal.amootsms.com/webservice2.asmx?wsdl', array('encoding'=>'UTF-8'));

$parameters['UserName'] = "MyUserName";
$parameters['Password'] = "MyPassword";

$result = $sms_client->RelayMessageDeliveryList($parameters)->RelayMessageDeliveryListResult;
echo $result;
```
### متد فعال کردن آدرس انتقال دلیوری (RelayMessageDeliverySetActive)
از طریق این متد می توانید یک آدرس "انتقال دلیوری پیامهای ارسالی" را فعال یا غیرفعال نمایید.
#### پارامترهای ورودی RelayMessageDeliverySetActive
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|UserName|String *|نام کاربری شما در سامانه پیامک آموت|
|Password|String *|رمز عبور شما در سامانه پیامک آموت|
|RelayMessageDeliveryID|Long *|کد آدرس "انتقال دلیوری پیامهای ارسالی"|
|Active|Bool *|فعال بودن|
#### نمونه کد RelayMessageDeliverySetActive (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";

long RelayMessageDeliveryID = 0;
bool Active = true;

AmootSMS.AmootSMSWebService2SoapClient client = new AmootSMS.AmootSMSWebService2SoapClient("AmootSMSWebService2Soap12");

AmootSMS.Status result = client.RelayMessageDeliverySetActive(UserName, Password, RelayMessageDeliveryID, Active);

if (result == AmootSMS.Status.Success)
{
    //خروجی
}
```
#### نمونه کد RelayMessageDeliverySetActive (PHP)
```php
$url = "https://portal.amootsms.com/webservice2.asmx/RelayMessageDeliverySetActive_REST";

$url = $url."?"."UserName=MyUserName";
$url = $url."&"."Password=MyPassword";

$url = $url."&"."RelayMessageDeliveryID=0";
$url = $url."&"."Active=true";

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
#### نمونه کد RelayMessageDeliverySetActive (PHP)
```php
ini_set("soap.wsdl_cache_enabled", "0");

$sms_client = new SoapClient('https://portal.amootsms.com/webservice2.asmx?wsdl', array('encoding'=>'UTF-8'));

$parameters['UserName'] = "MyUserName";
$parameters['Password'] = "MyPassword";

$parameters['RelayMessageDeliveryID'] = 0;
$parameters['Active'] = true;

$result = $sms_client->RelayMessageDeliverySetActive($parameters)->RelayMessageDeliverySetActiveResult;
echo $result;
```
### متد افزودن آدرس انتقال پیامهای دریافتی (RelayRecieveMessageCreate)
از طریق این متد می توانید یک آدرس جدید به لیست آدرسهای "انتقال پیامهای دریافتی" اضافه می کند.
#### پارامترهای ورودی RelayRecieveMessageCreate
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|UserName|String *|نام کاربری شما در سامانه پیامک آموت|
|Password|String *|رمز عبور شما در سامانه پیامک آموت|
|URLAddress|String *|آدرس URL|
#### نمونه کد RelayRecieveMessageCreate (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";

string URLAddress = "http://Mysite.com/RelayRecieveMessage";

AmootSMS.AmootSMSWebService2SoapClient client = new AmootSMS.AmootSMSWebService2SoapClient("AmootSMSWebService2Soap12");

AmootSMS.RelayRecieveMessageCreateResult result = client.RelayRecieveMessageCreate(UserName, Password, URLAddress);

if (result.Status == AmootSMS.Status.Success)
{
    //خروجی
}
```
#### نمونه کد RelayRecieveMessageCreate (PHP)
```php
$url = "https://portal.amootsms.com/webservice2.asmx/RelayRecieveMessageCreate_REST";

$url = $url."?"."UserName=MyUserName";
$url = $url."&"."Password=MyPassword";
$url = $url."&"."URLAddress=".urlencode( "http://Mysite.com/RelayRecieveMessage");

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
#### نمونه کد RelayRecieveMessageCreate (PHP)
```php
ini_set("soap.wsdl_cache_enabled", "0");

$sms_client = new SoapClient('https://portal.amootsms.com/webservice2.asmx?wsdl', array('encoding'=>'UTF-8'));

$parameters['UserName'] = "MyUserName";
$parameters['Password'] = "MyPassword";

$parameters['URLAddress'] = "http://Mysite.com/RelayRecieveMessage";

$result = $sms_client->RelayRecieveMessageCreate($parameters)->RelayMessageDeliveryCreateResult;
echo $result;
```
### متد دریافت آدرس های انتقال پیامهای دریافتی (RelayRecieveMessageList)
از طریق این متد می توانید لیست آدرسهای "انتقال پیامهای دریافتی" را دریافت نمایید.
#### پارامترهای ورودی RelayRecieveMessageList
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|UserName|String *|نام کاربری شما در سامانه پیامک آموت|
|Password|String *|رمز عبور شما در سامانه پیامک آموت|
#### نمونه کد RelayRecieveMessageList (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";

AmootSMS.AmootSMSWebService2SoapClient client = new AmootSMS.AmootSMSWebService2SoapClient("AmootSMSWebService2Soap12");

AmootSMS.RelayRecieveMessageListResult result = client.RelayRecieveMessageList(UserName, Password);

if (result.Status == AmootSMS.Status.Success)
{
    //خروجی
}
```
#### نمونه کد RelayRecieveMessageList (PHP)
```php
$url = "https://portal.amootsms.com/webservice2.asmx/RelayRecieveMessageList_REST";

$url = $url."?"."UserName=MyUserName";
$url = $url."&"."Password=MyPassword";

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
#### نمونه کد RelayRecieveMessageList (PHP)
```php
ini_set("soap.wsdl_cache_enabled", "0");

$sms_client = new SoapClient('https://portal.amootsms.com/webservice2.asmx?wsdl', array('encoding'=>'UTF-8'));

$parameters['UserName'] = "MyUserName";
$parameters['Password'] = "MyPassword";

$result = $sms_client->RelayRecieveMessageList($parameters)->RelayRecieveMessageListResult;
echo $result;
```
### متد فعال کردن آدرس انتقال پیامهای دریافتی (RelayRecieveMessageSetActive)
از طریق این متد می توانید یک آدرس "انتقال پیامهای دریافتی" را فعال یا غیرفعال نمایید.
#### پارامترهای ورودی RelayRecieveMessageSetActive
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|UserName|String *|نام کاربری شما در سامانه پیامک آموت|
|Password|String *|رمز عبور شما در سامانه پیامک آموت|
|RelayRecieveMessageID|Long *|کد آدرس "انتقال پیامهای دریافتی"|
|Active|Bool *|فعال بودن|
#### نمونه کد RelayRecieveMessageSetActive (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";

long RelayRecieveMessageID = 0;
bool Active = true;

AmootSMS.AmootSMSWebService2SoapClient client = new AmootSMS.AmootSMSWebService2SoapClient("AmootSMSWebService2Soap12");

AmootSMS.Status result = client.RelayRecieveMessageSetActive(UserName, Password, RelayRecieveMessageID, Active);

if (result == AmootSMS.Status.Success)
{
    //خروجی
}
```
#### نمونه کد RelayRecieveMessageSetActive (PHP)
```php
$url = "https://portal.amootsms.com/webservice2.asmx/RelayRecieveMessageSetActive_REST";

$url = $url."?"."UserName=MyUserName";
$url = $url."&"."Password=MyPassword";

$url = $url."&"."RelayRecieveMessageID=0";
$url = $url."&"."Active=true";

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
#### نمونه کد RelayRecieveMessageSetActive (PHP)
```php
ini_set("soap.wsdl_cache_enabled", "0");

$sms_client = new SoapClient('https://portal.amootsms.com/webservice2.asmx?wsdl', array('encoding'=>'UTF-8'));

$parameters['UserName'] = "MyUserName";
$parameters['Password'] = "MyPassword";

$parameters['RelayRecieveMessageID'] = 0;
$parameters['Active'] = true;

$result = $sms_client->RelayRecieveMessageSetActive($parameters)->RelayRecieveMessageSetActiveResult;
echo $result;
```
### متد ارسال از طریق بانک اطلاعاتی  (SendBank)
از طریق این متد می توانید پیامک های خود را با استفاده از بانک های اطلاعاتی موجود در سامانه ارسال نمایید.
#### پارامترهای ورودی SendBank
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|UserName|String *|نام کاربری شما در سامانه پیامک آموت|
|Password|String *|رمز عبور شما در سامانه پیامک آموت|
|SendDateTime|Object *|زمان ارسال پیامک|
|SMSMessageText|String *|متن پیامک|
|LineNumber|String *|شماره خط|
|BankID|Long *|کد بانک اطلاعاتی|
|OrderType|String *|ترتیب انتخاب Sequential = 0, Random = 1,|
|FromRow|Int *|از ردیف|
|Count|Int *|تعداد|
#### نمونه کد SendBank (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";
DateTime SendDateTime = DateTime.Now;
string SMSMessageText = "پیامک تستی من";
string LineNumber = "public";

long BankID = 0;
AmootSMS.SendOrderType SendOrderType = AmootSMS.SendOrderType.Sequential;
int FromRow = 0;
int Count = 100;

AmootSMS.AmootSMSWebService2SoapClient client = new AmootSMS.AmootSMSWebService2SoapClient("AmootSMSWebService2Soap12");

AmootSMS.SendResult result = client.SendBank(UserName, Password, SendDateTime, SMSMessageText, LineNumber,

    BankID, SendOrderType, FromRow, Count);

if (result.Status == AmootSMS.Status.Success)
{
    //خروجی
}
```
#### نمونه کد SendBank (PHP)
```php
$url = "https://portal.amootsms.com/webservice2.asmx/SendBank_REST";

$url = $url."?"."UserName=MyUserName";
$url = $url."&"."Password=MyPassword";

$nowIran = new DateTime('now', new DateTimeZone('IRAN'));
$url = $url."&"."SendDateTime=".$nowIran->format('c');

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
#### نمونه کد SendBank (PHP)
```php
ini_set("soap.wsdl_cache_enabled", "0");

$sms_client = new SoapClient('https://portal.amootsms.com/webservice2.asmx?wsdl', array('encoding'=>'UTF-8'));

$parameters['UserName'] = "MyUserName";
$parameters['Password'] = "MyPassword";

$nowIran = new DateTime('now', new DateTimeZone('IRAN'));
$parameters['SendDateTime'] = $nowIran->format('c');

$parameters['SMSMessageText'] = "پیامک تستی من";
$parameters['LineNumber'] = "public";

$parameters['BankID'] = 0;
$parameters['SendOrderType'] = "Sequential";
$parameters['FromRow'] =0;
$parameters['Count'] = 100;

$result = $sms_client->SendBank($parameters)->SendBankResult;
echo $result;
```
### متد ارسال از طریق بانک اطلاعاتی با گارانتی آوانک (SendBankWithAvanak)
از طریق این متد می توانید پیامک های خود را با استفاده از بانک اطلاعاتی و با گارانتی پیام صوتی آوانک ارسال نمایید، در صورتی که  شماره دریافت کننده در لیست سیاه باشد ارسال پیام صوتی به آن شماره انجام میگردد .
#### پارامترهای ورودی SendBankWithAvanak
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|UserName|String *|نام کاربری شما در سامانه پیامک آموت|
|Password|String *|رمز عبور شما در سامانه پیامک آموت|
|SendDateTime|Object *|زمان ارسال پیامک|
|SMSMessageText|String *|متن پیامک|
|AvanakMessageText|String *|متن پیام صوتی آوانک|
|SpeakerType|String *|نوع گوینده پیام صوتی female = 1, male = 2|
|LineNumber|String *|شماره خط|
|BankID|Long *|کد بانک اطلاعاتی|
|OrderType|String *|ترتیب انتخاب Sequential = 0, Random = 1,|
|FromRow|Int *|از ردیف|
|Count|Int *|تعداد|
#### نمونه کد SendBankWithAvanak (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";
DateTime SendDateTime = DateTime.Now;
string SMSMessageText = "پیامک تستی من";
string AvanakMessageText = "پیام صوتی تستی من";
AmootSMS.AvanakSpeakerType AvanakSpeakerType = AmootSMS.AvanakSpeakerType.female;

string LineNumber = "public";

long BankID = 0;
AmootSMS.SendOrderType SendOrderType = AmootSMS.SendOrderType.Sequential;
int FromRow = 0;
int Count = 100;

AmootSMS.AmootSMSWebService2SoapClient client = new AmootSMS.AmootSMSWebService2SoapClient("AmootSMSWebService2Soap12");

AmootSMS.SendResult result = client.SendBankWithAvanak(UserName, Password, SendDateTime, SMSMessageText,
                
    AvanakMessageText,
    AvanakSpeakerType,
    LineNumber,

    BankID, SendOrderType, FromRow, Count);

if (result.Status == AmootSMS.Status.Success)
{
    //خروجی
}
```
#### نمونه کد SendBankWithAvanak (PHP)
```php
$url = "https://portal.amootsms.com/webservice2.asmx/SendBankWithAvanak_REST";

$url = $url."?"."UserName=MyUserName";
$url = $url."&"."Password=MyPassword";

$nowIran = new DateTime('now', new DateTimeZone('IRAN'));
$url = $url."&"."SendDateTime=".$nowIran->format('c');

$url = $url."&"."SMSMessageText=".urlencode("پیامک تستی من");
$url = $url."&"."AvanakMessageText=".urlencode("پیام صوتی تستی من");
$url = $url."&"."AvanakSpeakerType=female";
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
#### نمونه کد SendBankWithAvanak (PHP)
```php
ini_set("soap.wsdl_cache_enabled", "0");

$sms_client = new SoapClient('https://portal.amootsms.com/webservice2.asmx?wsdl', array('encoding'=>'UTF-8'));

$parameters['UserName'] = "MyUserName";
$parameters['Password'] = "MyPassword";

$nowIran = new DateTime('now', new DateTimeZone('IRAN'));
$parameters['SendDateTime'] = $nowIran->format('c');

$parameters['SMSMessageText'] = "پیامک تستی من";
$parameters['AvanakMessageText'] = "پیام صوتی تستی من";
$parameters['AvanakSpeakerType'] = "female";
$parameters['LineNumber'] = "public";

$parameters['BankID'] = 0;
$parameters['SendOrderType'] = "Sequential";
$parameters['FromRow'] =0;
$parameters['Count'] = 100;

$result = $sms_client->SendBankWithAvanak($parameters)->SendBankWithAvanakResult;
echo $result;

```
### متد ارسال کد اعتبار سنجی (SendOTP)
از طریق این متد می توانید پیامک با رمز یکبار مصرف را به شماره انتخابی خود ارسال نمایید ، جهت استفاده از این متد باید نام مجوعه خود را اعلام نموده باشید.
#### پارامترهای ورودی SendOTP
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|UserName|String *|نام کاربری شما در سامانه پیامک آموت|
|Password|String *|رمز عبور شما در سامانه پیامک آموت|
|SendDateTime|Object *|زمان ارسال پیامک|
|SMSMessageText|String *|متن پیامک|
|LineNumber|String *|شماره خط|
|Mobile|String *|لیست موبایل های دریافت کنندگان پیامک|
|CodeLength|Short *|طول کد (حداقل 4 رقم و حداکثر 8 رقم)|
#### نمونه کد SendOTP (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";
DateTime SendDateTime = DateTime.Now;
string SMSMessageText = "پیامک تستی من";
string LineNumber = "public";
string Mobile = "9120000000";
short CodeLength = 4;

AmootSMS.AmootSMSWebService2SoapClient client = new AmootSMS.AmootSMSWebService2SoapClient("AmootSMSWebService2Soap12");

AmootSMS.SendOTPResult result = client.SendOTP(UserName, Password, SendDateTime, SMSMessageText, LineNumber, Mobile, CodeLength);

if (result.Status == AmootSMS.Status.Success)
{
    //خروجی
}
```
#### نمونه کد SendOTP (PHP)
```php
$url = "https://portal.amootsms.com/webservice2.asmx/SendOTP_REST";

$url = $url."?"."UserName=MyUserName";
$url = $url."&"."Password=MyPassword";

$nowIran = new DateTime('now', new DateTimeZone('IRAN'));
$url = $url."&"."SendDateTime=".$nowIran->format('c');

$url = $url."&"."SMSMessageText=".urlencode("پیامک تستی من");
$url = $url."&"."LineNumber=public";

$url = $url."&"."Mobile=9120000000";
$url = $url."&"."CodeLength=4";

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
#### نمونه کد SendOTP (PHP)
```php
ini_set("soap.wsdl_cache_enabled", "0");

$sms_client = new SoapClient('https://portal.amootsms.com/webservice2.asmx?wsdl', array('encoding'=>'UTF-8'));

$parameters['UserName'] = "MyUserName";
$parameters['Password'] = "MyPassword";

$nowIran = new DateTime('now', new DateTimeZone('IRAN'));
$parameters['SendDateTime'] = $nowIran->format('c');

$parameters['SMSMessageText'] = "پیامک تستی من";
$parameters['LineNumber'] = "public";

$parameters['Mobile'] = "9120000000";
$parameters['CodeLength'] = 4;

$result = $sms_client->SendOTP($parameters)->SendOTPResult;
echo $result;
```
### متد آمار پیامک های ارسالی (Statistics)
از طریق این متد می توانید آمار و گزارش پیامک های ارسالی خود را دریافت نمایید.
#### پارامترهای ورودی Statistics
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|UserName|String *|نام کاربری شما در سامانه پیامک آموت|
|Password|String *|رمز عبور شما در سامانه پیامک آموت|
|FromDate|Object *|از تاریخ|
|ToDate|Object *|تا تاریخ|
#### نمونه کد Statistics (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";

DateTime FromDate = DateTime.Now.AddMonths(-1);
DateTime ToDate = DateTime.Now;

AmootSMS.AmootSMSWebService2SoapClient client = new AmootSMS.AmootSMSWebService2SoapClient("AmootSMSWebService2Soap12");

AmootSMS.StatisticsResult result = client.Statistics(UserName, Password, FromDate, ToDate);

if (result.Status == AmootSMS.Status.Success)
{
    //خروجی
}
```
#### نمونه کد Statistics (PHP)
```php
$url = "https://portal.amootsms.com/webservice2.asmx/Statistics_REST";

$url = $url."?"."UserName=MyUserName";
$url = $url."&"."Password=MyPassword";

$url = $url."&"."FromDate=2020-02-01";
$url = $url."&"."ToDate=2020-03-01";

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
#### نمونه کد Statistics (PHP)
```php
ini_set("soap.wsdl_cache_enabled", "0");

$sms_client = new SoapClient('https://portal.amootsms.com/webservice2.asmx?wsdl', array('encoding'=>'UTF-8'));

$parameters['UserName'] = "MyUserName";
$parameters['Password'] = "MyPassword";

$parameters['FromDate'] = "2020-02-01";
$parameters['ToDate'] = "2020-03-01";

$result = $sms_client->Statistics($parameters)->StatisticsResult;
echo $result;
```
### متد ارسال از طریق بانک اطلاعاتی با گارانتی خط تبلیغاتی (SendBankWithBackupLine)
از طریق این متد می توانید پیامک های خود را با استفاده از بانک اطلاعاتی و با گارانتی خط تبلیغاتی (98) ارسال نمایید ، در صورتی که  شماره دریافت کننده در لیست سیاه باشد از طریق خط تبلیغاتی (98) به ایشان ارسال انجام میگردد.
#### پارامترهای ورودی SendBankWithBackupLine
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|UserName|String *|نام کاربری شما در سامانه پیامک آموت|
|Password|String *|رمز عبور شما در سامانه پیامک آموت|
|SendDateTime|Object *|زمان ارسال پیامک|
|SMSMessageText|String *|متن پیامک|
|LineNumber|String *|شماره خط|
|BackupLineNumber|String *|شماره خط پشتیبان|
|BankID|Long *|کد بانک اطلاعاتی|
|OrderType|String *|ترتیب انتخاب Sequential = 0, Random = 1,|
|FromRow|Int *|از ردیف|
|Count|Int *|تعداد|
#### نمونه کد SendBankWithBackupLine (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";
DateTime SendDateTime = DateTime.Now;
string SMSMessageText = "پیامک تستی من";

string LineNumber = "public";
string BackupLineNumber = "98";

long BankID = 0;
AmootSMS.SendOrderType SendOrderType = AmootSMS.SendOrderType.Sequential;
int FromRow = 0;
int Count = 100;

AmootSMS.AmootSMSWebService2SoapClient client = new AmootSMS.AmootSMSWebService2SoapClient("AmootSMSWebService2Soap12");

AmootSMS.SendResult result = client.SendBankWithBackupLine(UserName, Password, SendDateTime, SMSMessageText,

    LineNumber,
    BackupLineNumber,

    BankID, SendOrderType, FromRow, Count);

if (result.Status == AmootSMS.Status.Success)
{
    //خروجی
}
```
#### نمونه کد SendBankWithBackupLine (PHP)
```php
$url = "https://portal.amootsms.com/webservice2.asmx/SendBankWithBackupLine_REST";

$url = $url."?"."UserName=MyUserName";
$url = $url."&"."Password=MyPassword";

$nowIran = new DateTime('now', new DateTimeZone('IRAN'));
$url = $url."&"."SendDateTime=".$nowIran->format('c');

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
#### نمونه کد SendBankWithBackupLine (PHP)
```php
ini_set("soap.wsdl_cache_enabled", "0");

$sms_client = new SoapClient('https://portal.amootsms.com/webservice2.asmx?wsdl', array('encoding'=>'UTF-8'));

$parameters['UserName'] = "MyUserName";
$parameters['Password'] = "MyPassword";

$nowIran = new DateTime('now', new DateTimeZone('IRAN'));
$parameters['SendDateTime'] = $nowIran->format('c');

$parameters['SMSMessageText'] = "پیامک تستی من";

$parameters['LineNumber'] = "public";
$parameters['BackupLineNumber'] = "98";

$parameters['BankID'] = 0;
$parameters['SendOrderType'] = "Sequential";
$parameters['FromRow'] =0;
$parameters['Count'] = 100;

$result = $sms_client->SendBankWithBackupLine($parameters)->SendBankWithBackupLineResult;
echo $result;
```
### متد دریافت  دلیوری پیامک ها (GetDeliveries)
از طریق این متد می توانید وضعیت دلیوری (گزارش تحویل) پیامک های ارسال شده خود را دریافت نمایید.
#### پارامترهای ورودی GetDeliveries
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|UserName|String *|نام کاربری شما در سامانه پیامک آموت|
|Password|String *|رمز عبور شما در سامانه پیامک آموت|
|ListMessageID|Object *|لیست کد پیامک|
#### نمونه کد GetDeliveries (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";
long[] ListMessageID = new long[]{
    10001,
    10002,
};

AmootSMS.AmootSMSWebService2SoapClient client = new AmootSMS.AmootSMSWebService2SoapClient("AmootSMSWebService2Soap12");

AmootSMS.DeliveriesResult result = client.GetDeliveries(UserName, Password, ListMessageID);

if (result.Status == AmootSMS.Status.Success)
{
    //خروجی
}
```
#### نمونه کد GetDeliveries (PHP)
```php
$url = "https://portal.amootsms.com/webservice2.asmx/GetDeliveries_REST";

$url = $url."?"."UserName=MyUserName";
$url = $url."&"."Password=MyPassword";

$url = $url."&"."ListMessageID=10001,10002";

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
#### نمونه کد GetDeliveries (PHP)
```php
ini_set("soap.wsdl_cache_enabled", "0");

$sms_client = new SoapClient('https://portal.amootsms.com/webservice2.asmx?wsdl', array('encoding'=>'UTF-8'));

$parameters['UserName'] = "MyUserName";
$parameters['Password'] = "MyPassword";

$parameters['ListMessageID'] =array("10001", "10002");

$result = $sms_client->GetDeliveries($parameters)->GetDeliveriesResult;
echo $result;
```
### متد دریافت دلیوری پیامک (GetDelivery)
از طریق این متد می توانید وضعیت دلیوری (گزارش تحویل) پیامک ارسال شده خود را دریافت نمایید.
#### پارامترهای ورودی GetDelivery
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|UserName|String *|نام کاربری شما در سامانه پیامک آموت|
|Password|String *|رمز عبور شما در سامانه پیامک آموت|
|MessageID|Long *|کد پیامک|
#### نمونه کد GetDelivery (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";
long MessageID = 10001;

AmootSMS.AmootSMSWebService2SoapClient client = new AmootSMS.AmootSMSWebService2SoapClient("AmootSMSWebService2Soap12");

AmootSMS.DeliveryResult result = client.GetDelivery(UserName, Password, MessageID);

if (result.Status == AmootSMS.Status.Success)
{
    //خروجی
}
```
#### نمونه کد GetDelivery (PHP)
```php
$url = "https://portal.amootsms.com/webservice2.asmx/GetDelivery_REST";

$url = $url."?"."UserName=MyUserName";
$url = $url."&"."Password=MyPassword";

$url = $url."&"."MessageID=10001";

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
#### نمونه کد GetDelivery (PHP)
```php
ini_set("soap.wsdl_cache_enabled", "0");

$sms_client = new SoapClient('https://portal.amootsms.com/webservice2.asmx?wsdl', array('encoding'=>'UTF-8'));

$parameters['UserName'] = "MyUserName";
$parameters['Password'] = "MyPassword";

$parameters['MessageID'] = 10001;

$result = $sms_client->GetDelivery($parameters)->GetDeliveryResult;
echo $result;
```
### متد دریافت پیامک های ارسالی (GetMessages)
از طریق این متد می توانید پیامک های ارسالی خود را  در بازه زمانی مشخص دریافت نمایید.
#### پارامترهای ورودی GetMessages
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|UserName|String *|نام کاربری شما در سامانه پیامک آموت|
|Password|String *|رمز عبور شما در سامانه پیامک آموت|
|FromDateTime|Object *|از تاریخ/زمان|
|ToDateTime|Object *|تا تاریخ/زمان|
#### نمونه کد GetMessages (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";
DateTime FromDateTime = DateTime.Now.Date;
DateTime ToDateTime = DateTime.Now.Date;

AmootSMS.AmootSMSWebService2SoapClient client = new AmootSMS.AmootSMSWebService2SoapClient("AmootSMSWebService2Soap12");

AmootSMS.MessagesResult result = client.GetMessages(UserName, Password, FromDateTime, ToDateTime);

if (result.Status == AmootSMS.Status.Success)
{
    //خروجی
}
```
#### نمونه کد GetMessages (PHP)
```php
$url = "https://portal.amootsms.com/webservice2.asmx/GetMessages_REST";

$url = $url."?"."UserName=MyUserName";
$url = $url."&"."Password=MyPassword";

$FromDateTime= new DateTime('2020-01-01');
$url = $url."&"."FromDateTime=".$FromDateTimeIran->format('c');

$ToDateTime= new DateTime('2020-02-01');
$url = $url."&"."ToDateTime=".$ToDateTime->format('c');

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
#### نمونه کد GetMessages (PHP)
```php
ini_set("soap.wsdl_cache_enabled", "0");

$sms_client = new SoapClient('https://portal.amootsms.com/webservice2.asmx?wsdl', array('encoding'=>'UTF-8'));

$parameters['UserName'] = "MyUserName";
$parameters['Password'] = "MyPassword";

$FromDateTime = new DateTime('2020-01-01');
$parameters['FromDateTime'] = $FromDateTimeIran->format('c');

$ToDateTime= new DateTime('2020-02-01');
$parameters['ToDateTime'] = $ToDateTime->format('c');

$result = $sms_client->GetMessages($parameters)->GetMessagesResult;
echo $result;
```
### متد محاسبه هزینه پیامک (CalculateMessagePrice)
از طریق این متد می توانید با وارد کردن متن پیامک به همراه شماره خط ارسال و شماره های دریافت کنندگان هزینه ارسال خود را محاسبه نمایید.
#### پارامترهای ورودی CalculateMessagePrice
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|UserName|String *|نام کاربری شما در سامانه پیامک آموت|
|Password|String *|رمز عبور شما در سامانه پیامک آموت|
|SMSMessageText|String *|متن پیامک|
|LineNumber|String *|شماره خط|
|Mobiles|Object *|لیست موبایل های دریافت کنندگان پیامک|
#### نمونه کد CalculateMessagePrice (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";
string SMSMessageText = "پیامک تستی من";
string LineNumber = "public";
string[] Mobiles = new string[]
{
    "9120000000",
    "9150000000",
};

AmootSMS.AmootSMSWebService2SoapClient client = new AmootSMS.AmootSMSWebService2SoapClient("AmootSMSWebService2Soap12");

int result = client.CalculateMessagePrice(UserName, Password, SendDateTime, 
```
#### نمونه کد CalculateMessagePrice (PHP)
```php
$url = "https://portal.amootsms.com/webservice2.asmx/CalculateMessagePrice_REST";

$url = $url."?"."UserName=MyUserName";
$url = $url."&"."Password=MyPassword";

$SMSMessageText = urlencode("پیامک تستی من");
$url = $url."&"."SMSMessageText =".$SMSMessageText ;

$url = $url."&"."LineNumber=public";
$url = $url."&"."Mobiles=9120000000,9150000000";

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
#### نمونه کد CalculateMessagePrice (PHP)
```php
ini_set("soap.wsdl_cache_enabled", "0");

$sms_client = new SoapClient('https://portal.amootsms.com/webservice2.asmx?wsdl', array('encoding'=>'UTF-8'));

$parameters['UserName'] = "MyUserName";
$parameters['Password'] = "MyPassword";

$parameters['SMSMessageText'] = "پیامک تستی من";
$parameters['LineNumber'] = "public";
$parameters['Mobiles'] =array("9120000000", "9150000000");

$result = $sms_client->CalculateMessagePrice($parameters)->CalculateMessagePriceResult;
echo $result;
```
### متد ارسال با گارانتی خط تبلیغاتی (SendWithBackupLine)
از طریق این متد می توانید پیامک های خود را با گارانتی خط تبلیغاتی (98) ارسال نمایید ، در صورتی که  شماره دریافت کننده در لیست سیاه باشد از طریق خط تبلیغاتی (98) به ایشان ارسال انجام میگردد.
#### پارامترهای ورودی SendWithBackupLine
|نام|نوع|توضیحات|
| ------------- |:-------------:| -----:|
|UserName|String *|نام کاربری شما در سامانه پیامک آموت|
|Password|String *|رمز عبور شما در سامانه پیامک آموت|
|SendDateTime|Object *|زمان ارسال پیامک|
|SMSMessageText|String *|متن پیامک|
|LineNumber|String *|شماره خط|
|BackupLineNumber|String *|شماره خط پشتیبان|
|Mobiles|Object *|لیست موبایل های دریافت کنندگان پیامک|
#### نمونه کد SendWithBackupLine (C#)
```csharp
string UserName = "MyUserName";
string Password = "MyPassword";
DateTime SendDateTime = DateTime.Now;
string SMSMessageText = "پیامک تستی من";
string LineNumber = "public";
string BackupLineNumber = "98";
string[] Mobiles = new string[]
{
"9120000000",
"9150000000",
};

AmootSMS.AmootSMSWebService2SoapClient client = new AmootSMS.AmootSMSWebService2SoapClient("AmootSMSWebService2Soap12");

AmootSMS.SendResult result = client.SendWithBackupLine(UserName, Password, SendDateTime, SMSMessageText, LineNumber, BackupLineNumber, Mobiles);

if (result.Status == AmootSMS.Status.Success)
{
    //خروجی
}
```
#### نمونه کد SendWithBackupLine (PHP)
```php
$url = "https://portal.amootsms.com/webservice2.asmx/SendWithBackupLine_REST";

$url = $url."?"."UserName=MyUserName";
$url = $url."&"."Password=MyPassword";

$nowIran = new DateTime('now', new DateTimeZone('IRAN'));
$url = $url."&"."SendDateTime=".$nowIran->format('c');

$url = $url."&"."SMSMessageText=".urlencode("پیامک تستی من");
$url = $url."&"."LineNumber=public";
$url = $url."&"."BackupLineNumber=98";

$url = $url."&"."Mobiles=9120000000,9150000000";

$json = file_get_contents($url);
echo $json;

//$result = json_decode($json);
//echo $result->Status;
```
#### نمونه کد SendWithBackupLine (PHP)
```php
ini_set("soap.wsdl_cache_enabled", "0");

$sms_client = new SoapClient('https://portal.amootsms.com/webservice2.asmx?wsdl', array('encoding'=>'UTF-8'));

$parameters['UserName'] = "MyUserName";
$parameters['Password'] = "MyPassword";

$nowIran = new DateTime('now', new DateTimeZone('IRAN'));
$parameters['SendDateTime'] = $nowIran->format('c');

$parameters['SMSMessageText'] = "پیامک تستی من";
$parameters['LineNumber'] = "public";
$parameters['BackupLineNumber'] = "98";
$parameters['Mobiles'] =array("9120000000", "9150000000");

$result = $sms_client->SendWithBackupLine($parameters)->SendWithBackupLineResult;
echo $result;

```
