# AmootSMS

## معرفی وب سرویس پیامک آموت
پیامک آموت (پیشرفته ترین سامانه پیامکی ایران) یک سامانه دارای وب سرویس کامل برای ارسال و دریافت پیامک و مدیریت کامل خدمات دیگر است که براحتی میتوانید از آن استفاده کنید.

آدرس وب سایت
https://www.amootsms.com

آدرس پرتال
https://portal.amootsms.com

آدرس مستندات وب سرویس 
https://doc.amootsms.com

آدرس وب سرویس
https://portal.amootsms.com/webservice2.asmx





## نمونه کد ارسال پیامک
```C#
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


## نمونه کد وضعیت حساب کاربری
```C#
string UserName = "MyUserName";
string Password = "MyPassword";

AmootSMS.AmootSMSWebService2SoapClient client = new AmootSMS.AmootSMSWebService2SoapClient("AmootSMSWebService2Soap12");

AmootSMS.AccountStatusResult result = client.AccountStatus(UserName, Password);

if (result.Status == AmootSMS.Status.Success)
{
    //خروجی
}
```
