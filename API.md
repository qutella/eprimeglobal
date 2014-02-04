
### Basic Virtual Currency API Description:

Payment process consists of two request types: **check** and **pay**.

Request are send using GET method

On the **check** step payment parameters are being checked. A request is sent to the web interface specified in the project settings in the dashboard to verify parameters, provided for the payment.

On the **pay** step payment confirmation comes in. A request is sent to the web interface specified in the project settings in the dashboard to confirm that payment has been received.


#### List of parameters present in every request:


<table>
<tr>
<td>Parameter name</td>	<td>Description</td>
</tr><tr>
<td>command</td>	<td>Request type (check/pay)</td>
</tr><tr>
<td>account</td>	<td>Payment ID (main identification parameter)</td>
</tr><tr>
<td>qxt_[param_name]</td>	<td>Additional identification parameters specified in the project settings. These paramemters get tranfered with qxt_ prefix. All additional parameters, added in the project settings are being sent.</td>
</tr><tr>
<td>sign</td>	<td>md5 signature</td>
</tr><tr>
</table>


#### Forming an md5 signature string: 
**[command][sorted_params][secret_key]**
**[command]** – value of command paremeter
**[secret_key]** – secret key specified in the project settings,
**[sorted_params]**  -  string consisting of all the parameter values in the request **except sign, command and test** (if test parameter was sent) in alphabetic order of  their names.


For example secret key is being set to: **hd1827** and following parameters are present in the request:
<table>
<tr>
<td>Parameter name</td>	<td>Value</td>
</tr><tr>
<td>command</td>	<td>check</td>
</tr><tr>
<td>account</td>	<td>user_login</td>
</tr><tr>
<td>qxt_server</td>	<td>server</td>
</tr><tr>
<td>qxt_group</td>	<td>vip</td>
</tr><tr>
<td>sign</td>	<td>e579c5c8a73221eece608f6f70d12998</td>
</tr><tr>
</table>


In this case generated md5 string will look like:
**checkuser_loginvipserverhd1827**
where **check** – is the value of command parameter, 
**user_login, vip, server** – are values of all the parameters in alphabetic order of their names (account, qxt_group, qxt_server)
and **hd1827** is a secret key of the project

 
#### Check Request Description: 


#### Request format: 
Lets assume that web interface on the projects side is located at: 
http://ecommerce-domain.com/handler.php
and following additional identification parameters are being used : server and group

#### Request example:


http://ecommerce-domain.com/handler.php?command=check&account=[account]&qxt_server=[qxt_server]&qxt_group=[qxt_group]&sign=e579c5c8a73221eece608f6f70d12998&test=1

#### Request parameters description:


<table>
<tr>
<td>Parameter name</td>	<td>Description</td>
</tr><tr>
<td>command</td>	<td>Request type (check/pay)</td>
</tr><tr>
<td>account</td>	<td>Payment ID (main identification parameter)</td>
</tr><tr>
<td>qxt_[param_name]</td>	<td>Additional payment identification parameters</td>
</tr><tr>
<td>sign</td>	<td>md5 signature</td>
</tr><tr>
<td>test</td>	<td>Parameter marking transaction as a test transaction</td>
</tr>
</table>


#### Web interface on the sever side of the project should generate following answer in xml:

<?xml version="1.0" encoding="windows-1251"?>
<response>
	<result>[result]</result>
	<comment>[comment]</comment>
</response>


#### Web interface answer description:

<table>
<tr>
<td>Parameter name</td>	<td>Value</td>	<td>Required</td>
</tr><tr>
<td>Result</td>	<td>Specified in the table below</td>	<td>Yes</td>
</tr><tr>
<td>Comment</td>	<td>Comment</td>	<td>Optional</td>
</tr><tr>
</table>

#### result_code parameter values:

<table>
<tr>
<td>Value</td>	<td>Description</td>
</tr><tr>
<td>0</td>	<td>Payment identification parameters are correct. Payment can be done.</td>
</tr><tr>
<td>2</td>	<td>Payment identification parameters are incorrect</td>
</tr><tr>
<td>3</td>	<td>Invalid md5</td>
</tr><tr>
<td>7</td>	<td>Payment with specified identification parameters cannot be done for technical reasons</td>
</tr><tr>
</table>


#### Pay Request Description:


#### Request format: 
Lets assume that web interface on the projects side is located at: 
http://ecommerce-domain.com/handler.php
and following additional identification parameters are being used : server and group

#### Request example:
http://ecommerce-domain.com/handler.php?command=pay&account=[account]&qxt_server=[qxt_server]&qxt_group=[qxt_group]&sum=[sum]&user_payed=[user_payed]&pay_system_id=[pay_system_id]&game_sum=[game_sum]&currency_id=[currency_id]&price=[price]&rate=[rate]&user_fee=[user_fee]&fee=[fee]&game_count=[game_count]&client_sum=[client_sum]&sign=d9db3650cb33e3fdae0efa696cffc9f2

<table>
<tr>
<td>Parameter name</td>	<td>Description</td>	<td>Value</td>	<td>Always present</td>
</tr><tr>
<td>command</td>	<td>Request type (check/pay)</td>	<td>pay</td>	<td>Yes</td>
</tr><tr>
<td>account</td>	<td>Payment ID (main identification parameter)</td>	<td></td>	<td>Yes</td>
</tr><tr>
<td>qxt_[param_name]</td>	<td>Additional payment identification parameters</td> <td></td>		<td>If no additional parameters are specified in project settings, no qxt_ parameters are sent</td>
</tr><tr>
<td>id</td>	<td>Transaction ID</td>	<td>Integer</td>	<td>Yes</td>
</tr><tr>
<td>merchant_id</td>	<td>Merchant transaction ID</td> 	<td>Integer</td>	<td>Is only sent in repeating request. Is not sent in initial request or value sent is blank.</td>
</tr><tr>
<td>sum</td>	<td>Payment sum to be converted into amount of product</td>	<td>Float</td>	<td>Yes</td>
</tr><tr>
<td>user_fee</td>	<td>Transaction fee paid by user</td>	<td>Float</td>	<td>Yes</td>
</tr><tr>
<td>client_sum</td>	<td>Payment sum for the payout</td>	<td>Float</td>	<td>Yes</td>
</tr><tr>
fee	Transaction fee paid by project	Float	Yes
</tr><tr>
user_payed	Payment sum paid by user	Float	Yes
</tr><tr>
pay_system_id	ID of payment method used	Float	Yes
</tr><tr>
price	Price of the virtual currency unit on the moment of invoice creation	Float	Yes
</tr><tr>
currency_id	Payment currency ID	Float	Yes
</tr><tr>
rate	Currency exchange rate to the rate of virtual currency	Float	Yes
</tr><tr>
product_amount	Number of virtual currency units to be transferred to user	Float	Yes
</tr><tr>
date	Date and time of payment	YYYY-MM-DD HH:MM:SS	Yes
</tr><tr>
sign	md5 signature	md5 hash	Yes
</tr><tr>
test	Transaction marked as a test transaction	0/1	Only transferred in test transactions. If parameter test is present user should not get virtual currency transferred.
</tr><tr>
</table>




#### Web interface on the sever side of the project should generate following answer in xml:


<?xml version="1.0" encoding="windows-1251"?>
<response>
<id[id]</id>
<merchant_id[shop_id]</merchant_id>
<sum[sum]</sum>
<result[result]</result>
<comment[comment]</comment>
</response>


# Web interface answer description

Parameter name	Description	Value	Required
id	Qutella transaction ID	Integer	Yes
merchant_id	Projects transaction ID	Integer	Yes
sum	Number of virtual units transeffed to user	Float	Yes
result	Result	See table of possibe result below	Yes
comment	Comment		No

!	
Please note that all parameters except comment are required. Even in case of Error, answer with all required parameters has to be sent. 0 value should be sent if valid results cannot be sent due to the Error. If some of the required parameters are not sent or sent with blank values the answer won’t be accepted as valid.






result parameter values:

Value	Description	Fatal
0	Success	
1	Temporary error, please try again later	No
2	Payment identification parameters are incorrect	Yes
3	Invalid MD5	Yes
4	Invalid request (invalid sum, all or some of required parameters are not present)	Yes
5	Other error	Yes
7	Payment with specified identification parameters cannot be done for technical reasons	Yes

«Timeout» error is associated with payment if no response is sent whithin 7 seconds.

If no fatal error occurred 3 repeating requests will be sent with interval of 10 minutes.
One more request will be sent in 30 minutes, one after 60 minutes and 2 more in 180 minutes interval. If no answer comes after that, “no answer” fatal error is associated with payment. 

Repeating requests are sent with the same values as the first one with merchant_id parameter present.