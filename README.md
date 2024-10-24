# google_2fa
google 验证

// 示例

// 登陆验证

$google2fa = new GoogleAuthenticator();

$valid = $google2fa->verifyCode($google_code_saved, $request->googlecode, 2);

if (!$valid) {

    return $this->validationErrorsResponse(["googlecode" => "GOOGLE验证码错误"]);
    
}



// 绑定验证器

$ga = new GoogleAuthenticator();

$secret = $ga->createSecret();

$companyName=config('app.name');

$username=$info['username'];

//利用随机密钥生成二维码

$qrCodeUrl = $ga->getQRCodeGoogleUrl($username, $secret, $companyName);

//验证页面输入的code口令

//$checkResult = $ga->verifyCode($secret, $code, 2);

$info['qrcode'] = $qrCodeUrl;//验证器扫描的二维码

$info['secret'] = $secret;//验证器密钥


        
// 验证是否绑定，并保存google_code

$ga = new GoogleAuthenticator();

$secret=$input['secret'];  //验证器密钥

$code=$input['code'];

$checkResult = $ga->verifyCode($secret, $code, 2);  //验证

if (!$checkResult)  return Response::make()->error('验证码错误')->refresh();

$user_info->google_code=$secret;  //开始保存验证器密钥

