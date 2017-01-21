##curl返回cookies
1.首先要设置同时返回头部  

	curl_setopt($oCurl,CURLOPT_HEADER,1);

2.设置以文本格式返回  

	curl_setopt ( $oCurl, CURLOPT_RETURNTRANSFER, 1 );

3.通过正则获取cookies字符串

        $sContent = curl_exec ( $oCurl );

        preg_match('/Set-Cookie:(.*);/iU',$sContent,$str);//

