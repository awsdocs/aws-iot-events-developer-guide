# Using the Lambda function provided by AWS IoT Events<a name="use-alarm-notifications"></a>

The following requirements apply when you use the Lambda function provided by AWS IoT Events to manage your alarm notifications:
+ You must verify the email address that sends the email notifications in Amazon Simple Email Service \(Amazon SES\)\. For more information, see [Verifying email addresses in Amazon SES](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/verify-addresses-and-domains.html), in the *Amazon Simple Email Service Developer Guide*\.

  If you receive a verification link, click the link to verify your email address\. You might also check your spam folder for a verification email\.
+ If your alarm sends SMS notifications, you must use E\.164 international phone number formatting for phone numbers\. This format contains `+<country-calling-code><area-code><phone-number>`\.

  Example phone numbers:    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/iotevents/latest/developerguide/use-alarm-notifications.html)

  To find a country calling code, go to [countrycode\.org](https://countrycode.org/)\.

  The Lambda function provided by AWS IoT Events checks if you use E\.164 formatted phone numbers\. However, it doesn't verify the phone numbers\. If you ensure that you entered accurate phone numbers but didn't receive SMS notifications, you might contact the phone carriers\. The carriers may block the messages\.